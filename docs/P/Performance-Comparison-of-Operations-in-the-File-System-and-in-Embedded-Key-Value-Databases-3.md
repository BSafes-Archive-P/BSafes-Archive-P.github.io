---
layout: default
title: 3. Underpinnings of our Approach
parent: § Performance Comparison of Operations in the File System and in Embedded Key-Value Databases  
grand_parent: P
nav_order: 30 
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

## 3. Underpinnings of our Approach
This section presents the fundamental concepts of our approach as shown in Figure 1.

![Figure 1. Underpinnings of our approach](https://statics.bsafes.com/images/papers/Performance-Comparison-of-Operations-in-the-File-System-and-in-Embedded-Key-Value-Databases-fig-1.png)
Figure 1. Underpinnings of our approach

### 3.1. Embedded Databases
Embedded databases are databases that are included in an application rather than as a separate server [12]. Since they are embedded in the application itself, they do not require a separate server or internet access to use. Embedded databases are meant for scenarios where an application is storing data that is only needed on the local machine.

Some embedded databases, such as SQLite, are full relational databases. However, simpler NoSQL alternatives have become popular such as key-value databases. Keyvalue databases only allow efficient access of each entry by a unique key, and do not support more complex queries. While this is restrictive, it is sufficient for many use cases and allows the database to be much simpler and optimized.

SQLite3<sup>1</sup> is one of the most popular embedded databases. SQLite3 is an open source C library that implements an embedded SQL database engine. It it used extensively in mobile devices, Python apps, and browsers, among many more applications<sup>2</sup>. While SQLite is a fully featured SQL database, for our purposes we treat it as keyvalue store by just having one table with a primary key.

LevelDB<sup>3</sup> is a open-source, key-value embedded database sponsored by Google. As a key-value database, it maps arbitrary binary string keys and values and does not support SQL queries. LevelDB offers features such as high performance, compression of data, and “snapshots” to provide consistent read-only views of the data. LevelDB is used in many applications including Chrome’s IndexedDB<sup>4</sup> , Bitcoin Core<sup>5</sup> , and Minecraft Bedrock Edition<sup>6</sup>.

***
<sup>1</sup>. https://www.sqlite.org
<sup>2</sup>. https://www.sqlite.org/mostdeployed.html
<sup>3</sup>. https://github.com/google/leveldb
<sup>4</sup>.https://chromium.googlesource.com/chromium/src/+/a77a9a1/thirdparty/blink/renderer/modules/indexeddb/docs/idbdatapath.md
<sup>5</sup>. https://bitcoindev.network/understanding-the-data
<sup>6</sup>. https://github.com/Mojang/leveldb-mcpe

***

RocksDB<sup>7</sup> is another open-source, key-value embedded database. RocksDB is sponsored by Facebook, and is used in much of their infrastructure. RocksDB actually started as a fork of LevelDB, and has a similar API. Facebook found that LevelDB didn’t perform well with massive parallelism or datasets larger than RAM, so they created RocksDB with a focus on scaling to large applications.

Berkeley DB<sup>8</sup> is an open source embedded database provided by Oracle. Berkeley DB advertises itself as “a family of embedded key-value database libraries that provides scalable, high-performance data management services to applications.” Berkeley DB is very feature rich, and encompasses multiple different database technologies. Our benchmark makes use of the efficient key-value API, but Berkeley DB can also create relational SQL databases, and use multiple different index types, among many other features. Some programs that use Berkeley DB are Bogofilter, Citadel, and SpamAssassin.

### 3.2. File System Performance
File systems can have issues when handling large numbers of small files. For instance, most modern file systems, including FAT, ext3, and ext4, allocate space for files in a unit of a cluster or block, no matter how small the file is. On large files this is inconsequential, but if there is a very large number of files smaller than the cluster size it can lead to a large amount of space being wasted [1]. Directory lookup can also take a performance hit if a large number of files are directly under a single folder [2].

This file system performance bottleneck has led many applications, including browsers and web caches, to use “nested hash file structures” with files placed into intermediate sub-directories based on their hash [13]. This avoids placing too many files directly under a single folder and can improve performance. As seen in Figure 3, the top level directory may contain folders 00 through ff, where each sub-directory contains files whose hash digest starts with those characters. Files would be spread evenly across all 256 sub-directories. This process can be repeated for as many levels of nested as is required to get a reasonable number of files in the “leaf” directories.

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
