---
layout: default
title: 2. State of the Art
parent: § Performance Comparison of Operations in the File System and in Embedded Key-Value Databases 
grand_parent: P 
nav_order: 20 
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

## 2. State of the Art
There is an extent of existing research comparing various databases [4], [5]. Additionally, there is existing research comparing the performance of storing blob data in SQL servers vs. the file system [6], [7]. However, most the existing research is not recent, and is mostly focused on database servers. Minimal research has been done on this question in the realm of local desktop applications and comparing the file system to embedded key-value databases such as LevelDB, RocksDB, and Berkeley DB.

One of the few research works on comparing key-value databases to file system is the work done by Patchigolla et. al [8] in 2011. This research performed detailed benchmarks on database IO performance on mobile devices vs. file IO.

The research was specifically focused on Android devices, and the benchmarks compared SQLite and Perst embedded databases. This research concluded that if the number of records is less than 1,000, then using files is a good decision, otherwise an embedded DB is a better option. We believe extending this research into the realm of local desktop applications and taking embedded key-value databases into account will be valuable to future developers.

A related question to the comparison of embedded databases and the file system, but within the scope of SQL database servers, is whether to store large pieces of data such as images in the database directly or on the file system. On SQL servers, the the two typical methods for this are to either store the data directly in an SQL BLOB column or to only store the file path in the database and save the data as a file. A widely cited paper on this question is the work done Sears et. al. [6] which analyzed the performance of storing files in SQL Server as BLOBs vs. files. They concluded that data smaller than 256KB is more efficiently handled by database BLOBs, while the file system is better for data larger than 1 MB. Stancu-Mara and Baumann [7] did detailed benchmarking on BLOB handling in various SQL database servers, including PostgreSQL and MySQL. They benchmarked read and write for blobs of various sizes, concluding that MySQL was the fastest at handling BLOBs. However, both of these benchmarks were done in the late 2000s, and their conclusions may no longer be applicable to modern systems.

There are many benchmarks comparing NoSQL databases. For instance, Gupta et. al. compare the performance and functionality of 4 NoSQL databases with widely different focuses: MongoDB, Cassandra, Redis, and Neo4j [4]. Puangsaijai et. al. [5] compare the key-value database Redis with the relational database MariaDB for insert, update, remove, and select operations.

Related to measuring the performance gains of using key-value databases is research on using key-value databases to improve performance. Tulkinbekov et. al. [9] solve the problem of write amplification with key-value databases. The problem of write amplification is when writes are repeated multiple times which can lead to performance issues and extra wear and tear on the memory. Their fix is to introduce their own key-value store CaseDB, which not only solves write amplification but also outperforms LevelDB and WiscKey, a LSM-tree-based key-value store. Similarly, Chandramouli et. al. introduced FASTER [10], a new embedded key-value database system to improve performance in large scale applications. Their research was focused on improving concurrent performance on large scale applications with millions of accesses a second.

An alternative to using key-value databases to replace the file system, is to use key-value databases to improve the file system itself. Chen et al . [11] applied key-value database technology to improve performance of the file system. They created a file system middleware named FILT which uses key-value database technology to achieve up to 5.8x performance over existing file systems.

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
