+++
title = "MapReduce"
date = 2023-10-29
+++
**Table of Contents:**
- [1 This part is from MIT course](#1-this-part-is-from-mit-course)
  - [1-1 Implement simple Map and Reduce specification.](#1-1-implement-simple-map-and-reduce-specification)
  - [1-2 Finish the implementation contains master and multiple workers.](#1-2-finish-the-implementation-contains-master-and-multiple-workers)
  - [1-3 Add durability against the worker failures.](#1-3-add-durability-against-the-worker-failures)
- [2 This part is from DDIA book](#2-this-part-is-from-ddia-book)
  - [2-1 From syntax perspective](#2-1-from-syntax-perspective)
  - [2-2 From data processing perspective](#2-2-from-data-processing-perspective)
  - [2-3 Sort merge joins](#2-3-sort-merge-joins)
  - [2-4 Group by with MapReduce](#2-4-group-by-with-mapreduce)
  - [2-5 Handling skew](#2-5-handling-skew)
  - [2-6 Map side joins](#2-6-map-side-joins)


## 1 This part is from MIT course

I'm studying about distributes systems and one of my resources is MIT Distributed systems course (6.824). In this course as a first lesson you read the map reduce paper (original one from google) and do a really simple example on top of already built application. So the tasks are:

### 1-1 Implement simple Map and Reduce specification.
---
Yep it's the same one in the paper, but bit simpler and sequentially processed one.

I'm quite impressed how easy to create a specification for MapReduce. It's really impressive to be able to define a simple task and run distributed on big data.

### 1-2 Finish the implementation contains master and multiple workers.
---
Actually they have the implementation in place. Only thing you need to do is the writing the master. Some simple worker group code, just use channels and goroutines.

The impressive part in here for me is how easy to manage work from the master. The abstraction was nicely done with small chunk of code. Also tests are impressive too. I didn't think testing this kind of separated system to be that simple.

### 1-3 Add durability against the worker failures.
---
In google's paper it says for every MapReduce run there is 1.2 workers failing in average. Thinking that the durability is really important.

So for this one impressive part is having the idempotent implementation. Because executing same job twice doesn't effect the final solution it was much easier to handle errors. Also in original paper I read that google runs functions with backups, even the main program doesn't fail it's possible that backup finishes the work faster.

## 2 This part is from DDIA book
DDIA: Designing Data Intensive Applications

There is two mentions to the MapReduce in here.

### 2-1 From syntax perspective
In this part, book compares SQL statements, MapReduce and MongoDB aggregation pipeline. Between these three MapReduce is the imperative one. The other two is declarative.

Also it's mentioned how useful MapReduce when it's idempotent. But even though a declarative SQL allows engine to optimize functions much better.

### 2-2 From data processing perspective
It mentions that MapReduce is really similar to the unix tooling. You have a splitter, then you map entries coming from splitting, after you done with it you reduce the results. The main advantage is you can run them in multiple workers across the cluster.

Also mentions that usually single MapReduce is not enough for the wanted result. So it's a common practice to chain jobs into each other. Just like pipelining unix commands.

### 2-3 Sort merge joins
So in case of joining data into MapReduce workflow the book suggest to get a snapshot of the resource to be joined and include it inside the pipeline. So you can have your main data and join data included into same reduce worker. And even you can sort the files in a order to always have 1 metadata and records back to back. I don't want to rewrite same example but it's really well explained in book. (page 403) Most insane part for me is being able to do a complex operation in a really high performance and total abstraction. Imagine as a data analyst writing MapReduce specification, you don't deal with most things.

### 2-4 Group by with MapReduce
Lol it's literally picking the correct KEY to emit from the map. Reduce will do the grouping in the output file.

### 2-5 Handling skew
This is a new term to me. But basically it means handling the hot keys. Imagine you're processing the tweet interactions through your map reduce workflow. What you gonna do when the twit_author key is @elonmusk? This guy has insane interaction. Basically means F for the reducer handling that @elonmusk tweets.

I tried to write some but I realized I didn't understand the part about handling skewed data.
TODO
### 2-6 Map side joins

TODO