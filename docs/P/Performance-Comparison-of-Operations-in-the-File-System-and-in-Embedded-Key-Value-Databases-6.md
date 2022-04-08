---
layout: default
title:  6. Future Work 
parent: § Performance Comparison of Operations in the File System and in Embedded Key-Value Databases 
grand_parent: P
nav_order: 60 
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

## 6. Future Work
This benchmark has shown that key-value store performance is a complex issue that is highly depedent on a particular workload. Research examining other factors is needed. This benchmark ran on the ext4 file system, research comparing the performance of different file systems, in particular the wide-spread NTFS, may have different performance profiles. Examining the effects of each embedded database’s configuration as well as different access patterns, such as sequential access, would also be valuable avenues for research.

Performance may be affected by complex caching patterns or the embedded databses running background processes such as LevelDBs background compaction16. More research in to the exact causes for the various outliers and performance degradations noted in this dataset would be valuable as well.

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
