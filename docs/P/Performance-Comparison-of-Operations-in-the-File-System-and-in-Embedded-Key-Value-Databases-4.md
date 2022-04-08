---
layout: default
title: 4. Methodology 
parent: § Performance Comparison of Operations in the File System and in Embedded Key-Value Databases
grand_parent: P 
nav_order: 40 
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

## 4. Methodology
An pseudocode outline of the algorithm used for the benchmark can be seen in Listing 1. The algorithm loops over different combinations of store type, data type (compressible or incompressible), size range, and count range (Line 3). For each combination it benchmarks 1,000 iterations of each of the 4 key-value operations (Lines 11, 16, 20 and 25) using a random access pattern and random data. It then records the peak memory usage and disk space efficiency for each combination (Lines 30 and 33). Finally it outputs the results to a CSV file (Line 35).

***
<sup>7</sup>. http://rocksdb.org
<sup>8</sup>. https://www.oracle.com/database/technologies/related/berkeleydb.html

***

![Figure 2. flat structure](https://statics.bsafes.com/images/papers/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-fig-2.png)
Figure 2. flat structure

![Figure 3. nested structure](https://statics.bsafes.com/images/papers/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-fig-3.png)
Figure 3. nested structure

![Listing 1. Pseudocode of benchmark]Listing 1. Pseudocode of benchmark(https://statics.bsafes.com/images/papers/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-list-1.png)
Listing 1. Pseudocode of benchmark

### 4.1. Comparing Storage Methods
4 different embedded key-value databases were compared, SQLite3, LevelDB, RocksDB, and Berkeley DB. While SQLite is a fully featured SQL database, for our purposes we treat it as key-value database by just having one table with a primary key column and a value column.

Then, the embedded databases were compared with two strategies for storing key-value records on disk, flat and nested. The flat storage strategy places all the records as files with their key as their name under one directory, as seen in Figure 2. The nested storage strategy uses a nested hash file structure as described in Section 3.2 and Figure 3. 3 levels of nesting were chosen with 2 hexadecimal chars per level. We choose a depth of 3 as it can hold up to 16,777,216 (2563 ) records while still keeping around 256 records in the leaf nodes, and our benchmark is only testing up to 10,000,000 records.

As seen in Figure 4, wrappers were created for each of the 6 different storage methods that implemented an abstract interface that could be used by the benchmark. The wrappers had methods for insert, update, get, and remove, and kept an count of how many records were in the database.

![Figure 4. Class diagram](https://statics.bsafes.com/images/papers/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-fig-4.png)
Figure 4. Class diagram

Both the SQLite and Berkeley DB C interfaces return char pointers that become invalid after doing another database operation. The wrappers did a copy of the buffer memory managed by our code. This does add some overhead that could be avoided if a developer only needed the data briefly and did not need to do any other database operations until they were finished with the data. But we assume that the most common use case will do a copy to avoid risking dangling pointers and memory safety issues.

### 4.2. Comparing Usage Patterns
Multiple different usage patterns for each of the storage methods were compared. 4 record size ranges were compared, less than 1 KiB, 1 - 10 KiB, 10 - 100 KiB, and 100 KiB - 1 MiB, and 3 record count ranges, 100 - 1,000, 10,000 - 100,000, and 1,000,000 - 10,000,000. All combinations of sizes and counts that led to databases smaller than 10 GiB were benchmarked (Line 5).

This benchmark measured random access reads and updates. A valuable direction for future work could be to benchmark different access patterns.

LevelDB and RocksDB implement data compression. Since this could have a significant impact on the size of the records on disk and the cost of IO operations, both compressible and incompressible data were tested. When working with compressible data both LevelDB and RocksDB were configured to use the Snappy<sup>9</sup> compression algorithm. Compression was disabled when working with incompressible data.

### 4.3. Generating Data
Data for values in insert and update statements was randomly generated. Incompressible data was generated with the Mersenne Twister 19937 algorithm (as in the C++ standard library) to generate pseudorandom bytes. Compressible text was selected from public domain books downloaded from the Gutenberg project<sup>10</sup>. We downloaded 250 English language books from Gutenberg and selected text randomly from them.

Keys were generated using a SHA-1 hash of the index, based on order keys were inserted (Line 12). Using this method let us emulate having random keys, but still be able to pick random keys from the store by hashing a random number from the range [0, store.count). This is important to implement random access get (Line 17), update (Line 21), and remove (Line 26) without storing a list of all keys inserted. This strategy does cause a complication when benchmarking the remove operation however. After a random remove we can no longer assume the keys are in a continuous range. This is easily remedied by replacing the key after each removal to keep the keys continuous (Line 28).

### 4.4. Taking Measurements
For each combination of store and usage patterns we measured multiple attributes. The primary attribute we measured was the time taken for each of the fundamental key-value operations, insert, update, get, and remove. For each combination we ran each operation 1,000 times and recorded the average, minimum, and maximum values in nanoseconds.

Since the memory usage of an application is also an important factor of performance, we measured the peak memory usage for each combination as well (Line 30). To measure peak memory usage we used the Linux getrusage syscall<sup>11</sup>. This syscall returns the peak resident set size used by the process in kilobytes. We reset the peak memory between configurations by using the special file /proc/self /clear_refs<sup>12</sup>. Writing a “5” to this file resets the process’s peak memory usage or “high water mark” measurement and allows us to get the peak memory usage for a specific period of time. We then subtracted a baseline measurement of the memory usage taken at the beginning of the benchmark to show only the memory used by the store.

Another factor besides performance that may be important for developers is the “space efficiency” of a storage solution, especially if they are working with large amounts of

***
<sup>9</sup>. https://google.github.io/snappy
<sup>10</sup>. https://www.gutenberg.org/
<sup>11</sup>. https://man7.org/linux/man-pages/man2/getrusage.2.html
<sup>12</sup>. https://man7.org/linux/man-pages/man5/proc.5.html
***

data or limited space. Embedded databases can add storage overhead or, if they have compression, greatly reduce the size used. And the file system itself can waste space when storing many tiny files. We measured the approximate space efficiency of each store by comparing the amount of data put in versus the actual space taken up on disk (Line 33). We measured space efficiency by counting the number of bytes actually inserted into the store, and then using the du command<sup>13</sup> to measure the space taken on disk including wasted block space.

### 4.5. Implementing the Algorithm
We choose to implement the benchmark in C++. All the embedded databases we compared were written in C or C++ and so could be used in a C++ project. Using C++ allowed us to compile and link the embedded databases giving us more control over their configuration. Additionally, using C++ ensures there is no extra overhead from the language bindings.

The benchmark is open source and the code is available on GitHub<sup>14</sup>.

### 4.6. Choosing Hardware
The benchmark was run on an virtual machine provide by Southern Adventist University. The machine was running Ubuntu Server 21.10 with 8 GiB of RAM and 250 GiB hard drive. TODO get more details of the server.

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
