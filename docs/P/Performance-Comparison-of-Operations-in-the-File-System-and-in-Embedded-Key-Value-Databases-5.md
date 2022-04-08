---
layout: default
title: 5. Results 
parent: § Performance Comparison of Operations in the File System and in Embedded Key-Value Databases
grand_parent: P 
nav_order: 50 
---
<style>
.dont-break-out {
  /* These are technically the same, but use both */
  overflow-wrap: break-word;
  word-wrap: break-word;

     -ms-word-break: break-all;
  /* This is the dangerous one in WebKit, as it breaks things wherever */
  word-break: break-all;
  /* Instead use this non-standard one: */
  word-break: break-word;
}

.youtube-container {
    position: relative;
    width: 100%;
    height: 0;
    padding-bottom: 56.25%;
}
.youtube-video {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

</style>

<div class="dont-break-out" markdown="1">

1. TOC
{:toc}

## 5. Results
This section describes notable patterns and analysis of the results of the benchmark. A condensed summary of the results is listed in Figure 5, which lists the most efficient storage option for each usage pattern. The operation measured and record size are listed on the vertical axis, while data compressibility and record count are listed on the horizontal. Combinations of record size and count that would result in more 50 GiB of files were skipped and left blank in the figure. The full results of the of the benchmark are available on GitHub<sup>15</sup> in both CSV and Excel format with charts.

### 5.1. Filesystem vs DBs
The choice of when to use the file system vs. an embedded database is complex and very dependent on the particular usage pattern what types of operations are being performed.

For records less than 1 KiB, an embedded database is faster, in this benchmark, Berkeley DB is the fastes in most scenarios with tiny records.

***
<sup>13</sup>. https://man7.org/linux/man-pages/man1/du.1.html
<sup>14</sup>. https://www.github.com/TODO/host/on/github
<sup>15</sup>. https://www.github.com/TODO/host/on/github
***

At the 1 to 10 KiB range, databases are still better in most scenarios, though the file system is faster on get and inserts when there are 100,000 records.

At the 10 to 100 KiB range, file system performance is better than or very close to the database’s for get and insert while RocksDB or LevelDB is faster removes with record counts less than 10,000 as long as compression is turned off. Berkeley DB leads on the update benchmarks.

The results with 100 KiB to 1 MiB records is similar, the file system is generally faster for get and insert, though LevelDB without compression is faster for 1,000 records or less. Berkeley DB performs well on updates. RocksDB without compression is fastest for removes until 10,000 then the file system is faster.

As expected, the file system had poor space efficiency of about 12% on the tiny records due to the block allocation overhead, while the embedded databases were able to compact multiple records into a block. The baseline storage overhead of the embedded databases appears to be minimal, and both Berkeley DB and RocksDB were more space efficient than the flat folder store even with only 100 records. At large sizes the differences in space efficiency between databases and the file system are minimal, as the wasted space at the end of each block becomes less of a factor. However, in the case of compressible data, LevelDB and RocksDB can use compression to get space efficiency of up to 156% at the cost of more CPU overhead.

### 5.2. Effects of Compression
For the databases that implemented compression, LevelDB and RocksDB, turning compression on increased space efficiency of course, at the cost of notably decreased performance. The reduced IO does not seem to make up for the extra CPU overhead of performing the compression.

### 5.3. SQLite3 Patterns
SQLite scales well in most scenarios, for instance with less than 1 KiB records the get operation is only 60% slower at 1,000,000 records than for 100. An interesting deviation from this is at 1,000,000, 1KiB and 10KiB records of incompressible data, where SQLite take 64x longer than at 100,000 records. This spike does not occur when using compressible data however, so perhaps SQLite handles text and binary data differently n certain scenarios. The update operation took about 20-30% long the incompressible binary data than compressible text data. Space efficiency increased as record size and record count increased, reaching nearly 100% at the highest sizes.

However, the constant factor is very high and SQLite is one of the worst performing storage options. This is unsurprising as SQLite is a full relational database and has many more features and overheads than necessary for a simple key-value store.

![Figure 5. Summary of the best storage options for each usage pattern](https://statics.bsafes.com/images/papers/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-fig-5.png)
Figure 5. Summary of the best storage options for each usage pattern

### 5.4. LevelDB Patterns
Compression adds significant overhead to LevelDB. With compression turned on, performance on all operations at large record sizes or large record counts is up to 20x slower than with it off. Even without compression, LevelDB has some major performance degradation for gets starting at 1,000,000 records for less than 1 - 100 KiB records, and 100,000 for 100 KiB to 1 MiB records as well as inserts at the 100 KiB to 1 MiB size range and over 100,000 records.

With compression enabled, space efficiency of text data can reach as high as 155 percent space efficiency.

### 5.5. RocksDB Patterns
Since RocksDB is a fork of LevelDB, we expected to see similar results, however their performance had quite a few differences. RocksDB performance degrades significantly at 100 KiB to 1 MiB records for all the operations, and the performance degradation for get with large record counts and compression was more pronounced for than in LevelDB. RocksDB did not notable performance degradation for inserts on high record counts, unlike LevelDB.

### 5.6. Berkeley DB Patterns
Berkeley DB is one of the best performing databases in this benchmark. With the time of the operation increasing slowly as count increases. However, there’s some notable spikes. At 1,000,000, 1 KiB - 10 KiB records there was an up to 100x slow down for get, update, remove operations with both incompressible and compressible data.

### 5.7. File System Patterns
The file system scales to large numbers of folders quite well until about 1,000,000, 1 - 10 KiB records where get time increases 500x for both compressible and incompressible data. This spike does not occur on smaller records. Insert and update time is primarily dominated by the record size factor.

File system operations have several outliers. For instance on the insert, compressible, 1 - 10 KiB, 1,000,000 record benchmark, the nested folder store has a 33x slowdown compared to incompressible. Since the file system does not do any compression, it seems unlikely that text vs binary data would make such a large difference and if it did it should affect both flat and nested, which it doesn’t. Looking at the measurements, the total sum of the 1000 repetitions was 776 ms, while the maximum record was 707 ms, one single iteration took 90% of the time. Similar spikes occur elsewhere in file system operations. We suspect that some OS factor is causing intermittent stalls in file system operations.

### 5.8. Flat Folder vs. Nested Folder
Using the nested directory structure seems to be mostly counter productive. In most cases the nested folder is slower than the simpler flat structure. It was also less space efficient than the flat folder store, unsurprisingly, due to the extra directory overhead. In a few scenarios it offers some benefits, on tje remove operation at 10 - 100 KiB with 100,000 records, the nested structure is about 30% faster than the flat and nested is faster for updates with 10 - 100 KiB sized records and a record count of 100,000.

The benchmark did not show the file system performance degrading significantly at large file sizes. In fact file system access time was near constant, varying only by 1-2x on most count ranges, with for notable exception. At the 1 - 10Kb record size range and 1,000,000 count range the file system has a 500x degradation in speed compared to the 100,000 range. Interestingly, this degradation did not occur on any of the other record sizes.

***

#### Table of Contents
{: .no_toc}

<ul><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-1/">
1. Introduction</a></li><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-2/">
2. State of the Art</a></li><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-3/">
3. Underpinnings of our Approach</a></li><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-4/">
4. Methodology</a></li><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-5/">
5. Results</a></li><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-6/">
6. Future Work</a></li><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-7/">
7. Conclusion</a></li><li> <a href="/docs/P/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-8/">
References</a></li></ul>

***

</div>
