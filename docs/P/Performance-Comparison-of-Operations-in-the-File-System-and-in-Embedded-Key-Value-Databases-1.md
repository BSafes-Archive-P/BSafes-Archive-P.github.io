---
layout: default
title: 1. Introduction
parent: § Performance Comparison of Operations in the File System and in Embedded Key-Value Databases  
grand_parent: P 
nav_order: 10 
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

## 1. Introduction
A common scenario when developing local desktop applications is the need for persisting many small files or records as application data and needing to retrieve and manipulate those records with some unique ID, essentially forming a key-value store. For example a game developer may need to store records for each game entity or game level or note-taking software would need to store a large number of small text records.

In this kind of scenario, a developer has two main choices: leveraging the file system for storage or using an embedded key-value database. If the developer chooses to use the file system to store the records, they can simply save each record to disk with its unique ID as the filename. This has the advantage of being simple to implement and adding no extra dependencies.

However, file systems can have space and performance issues when handling large numbers of small files [1] [2]. As an alternative to the file system, the developer can choose to use an embedded database. However, adding a dependency on an embedded database adds a fair amount of technical overhead to a project and increasing overall system complexity. If the embedded database is statically linked with an application, it will typically increase compilation time and executable size [3]. If it is linked dynamically, it can complicate installation of an application as external dependencies will need to be installed. Therefore, a developer will likely want to avoid adding a dependency on an embedded database if their use case is not significantly benefited by it.

Despite this being a common scenario, little research has been done comparing simple file system options to embedded key-value databases. Our contribution is the analysis and comparison of the performance of four popular open source embedded databases – SQLite3, LevelDB, RocksDB, and Berkeley DB – with storing records on the file system. This research will enable developers to make informed decisions on what tools are best for their scenario

This paper is organized as follows. Section 2 presents the existing research work on file system and embedded database performance. Section 3 presents the background knowledge behind our approach. Section 4 presents the methodology of the research and Section 5 presents the evaluation of the results. Section 6 presents directions for future research, and Section 7 presents our conclusions.

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
