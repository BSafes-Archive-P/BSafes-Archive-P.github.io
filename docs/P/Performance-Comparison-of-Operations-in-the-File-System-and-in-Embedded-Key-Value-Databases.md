---
layout: default
title: § Performance Comparison of Operations in the File System and in Embedded Key-Value Databases  
parent: P 
has_children: true
nav_order: 9780800
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
This is the mobile-friendly web version of the [original article](https://knowledge.e.southern.edu/cgi/viewcontent.cgi?article=1110&context=crd).
# Performance Comparison of Operations in the File System and in Embedded Key-Value Databases
{: .no_toc }


### Jesse Hines *School of Computing, Southern Adventist University, Collegedale, USA, *jessehines 'at' southern.edu*
{: .no_toc }

### Nicholas Cunningham *School of Computing, Southern Adventist University, Collegedale, USA, nicholascunningham 'at' southern.edu*
{: .no_toc }

### German H. Alf´erez ´ *School of Computing, Southern Adventist University, Collegedale, USA harveya 'at' southern.edu*
{: .no_toc }


<div class="youtube-container">
<iframe width="100%" src="https://www.youtube.com/embed/p4jAgaaIzJ0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen class="youtube-video"></iframe>
</div>
**Video:** Southern Adventist University, An Honest Campus Tour
 
1. TOC
{:toc}

### *Abstract*—A common scenario when developing local PC applications such as games, mobile apps, or presentation software is storing many small files or records as application data and needing to retrieve and manipulate those records with some unique ID. In this kind of scenario, a developer has the choice of simply saving the records as files with their unique ID as the filename or using an embedded on-disk key-value database. Many file systems have performance issues when handling large numbers of small files, but developers may want to avoid a dependency on an embedded database if it offers little benefit or has a detrimental effect on performance for their use case. Despite the need for benchmarks to enable informed answers to this design decision, little research has been done in this area. Our contribution is the comparison and analysis of the performance for the insert, update, get, and remove operations and the space efficiency of storing records as files vs. using key-value embedded databases including SQLite3, LevelDB, RocksDB, and Berkeley DB.

### *Index Terms*—databases, file systems, database performance
</div>
