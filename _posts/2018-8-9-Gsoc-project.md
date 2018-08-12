---
layout: post
title: Summary of GSOC 2018 project
---

## Overview
I am Guobao LI, an undergraduate from SCUT and graduate from Polytech Nantes. It is my pleasure to take part in the project [SYSTEMML-2083](https://issues.apache.org/jira/browse/SYSTEMML-2083) with the Apache community and my mentor Matthias Boehm in the GSOC. The objective of [SYSTEMML-2083](https://issues.apache.org/jira/browse/SYSTEMML-2083) is to design and implement the language and runtime of parameter server in Apache SystemML. Finally, we have realized and tested the local and Spark data-parallel parameter server including the update strategy (BSP, ASP) and update frequency (Batch, Epoch), data partitioning schemes (DC, DR, DRR, OR) in this project. I am glad to see that our contribution will be integrated in the next release of Apache SystemML.

## Subject in JIRA:
Here is the [link](https://issues.apache.org/jira/browse/SYSTEMML-2083) pointing to all the issues in JIRA.

## Pull Requests:
Here is a list of Pull Requests related to the project and all the code has already been merged into Apache SystemML master.

| Title | Pull Request | Commit |
| ------------- |-------------|-------------|
|[SYSTEMML-2084,2317-20] Language and compiler support paramserv builtin|[https://github.com/apache/systemml/pull/764](https://github.com/apache/systemml/pull/764)|[https://github.com/apache/systemml/commit/e270960ca41c7c0197373b53960cae6e7aca84ab](https://github.com/apache/systemml/commit/e270960ca41c7c0197373b53960cae6e7aca84ab)|
|[MINOR] Fix incorrect list indexing range check, extended name handling|[https://github.com/apache/systemml/pull/766](https://github.com/apache/systemml/pull/766)|[https://github.com/apache/systemml/commit/96954b4ca980d928bf2c4c4eb012efe0188a4ac6](https://github.com/apache/systemml/commit/96954b4ca980d928bf2c4c4eb012efe0188a4ac6)|
|[SYSTEMML-2085] Initial version of local backend for paramserv builtin|[https://github.com/apache/systemml/pull/771](https://github.com/apache/systemml/pull/771)|[https://github.com/apache/systemml/commit/97018d4e688ba7eeaaa4567ca1e174a3c5525468](https://github.com/apache/systemml/commit/97018d4e688ba7eeaaa4567ca1e174a3c5525468)|
|[SYSTEMML-2344,48,49,52] Various improvements local paramserv backend|[https://github.com/apache/systemml/pull/777](https://github.com/apache/systemml/pull/777)|[https://github.com/apache/systemml/commit/d44b3280f2132deb303e955ff5b9a17daac4c31e](https://github.com/apache/systemml/commit/d44b3280f2132deb303e955ff5b9a17daac4c31e)|
|[SYSTEMML-2359] Additional paramserv update frequency: per-epoch|[https://github.com/apache/systemml/pull/780](https://github.com/apache/systemml/pull/780)|[https://github.com/apache/systemml/commit/51057e4712d6ab9a190a9c1f8e9f36d48a8a1fd5](https://github.com/apache/systemml/commit/51057e4712d6ab9a190a9c1f8e9f36d48a8a1fd5)|
|[SYSTEMML-2364,66-88] Extended paramserv data partitioning schemes|[https://github.com/apache/systemml/pull/781](https://github.com/apache/systemml/pull/781)|[https://github.com/apache/systemml/commit/69ef76c06a6aea0c1e9b6750a37997a950a81794](https://github.com/apache/systemml/commit/69ef76c06a6aea0c1e9b6750a37997a950a81794)|
|[SYSTEMML-2380] Fix paramserv shutdown of agg service thread pool|[https://github.com/apache/systemml/pull/782](https://github.com/apache/systemml/pull/782)|[https://github.com/apache/systemml/commit/c10e509a78232f8cacfa9c7485395792d6af24e8](https://github.com/apache/systemml/commit/c10e509a78232f8cacfa9c7485395792d6af24e8)|
|[SYSTEMML-2381] Rework paramserv data partitioner API and tests|[https://github.com/apache/systemml/pull/783](https://github.com/apache/systemml/pull/783)|[https://github.com/apache/systemml/commit/0871f260e6fc6d6fed57d2cc249bf4d8beb0a31f](https://github.com/apache/systemml/commit/0871f260e6fc6d6fed57d2cc249bf4d8beb0a31f)|
|[SYSTEMML-2389] Fix paramserv calculation of iterations per worker|[https://github.com/apache/systemml/pull/785](https://github.com/apache/systemml/pull/785)|[https://github.com/apache/systemml/commit/ebfe327e4474d0692679efc847216131d38777e8](https://github.com/apache/systemml/commit/ebfe327e4474d0692679efc847216131d38777e8)|
|[SYSTEMML-2392/8,2401/2/6] Paramserv statistics and various fixes|[https://github.com/apache/systemml/pull/787](https://github.com/apache/systemml/pull/787)|[https://github.com/apache/systemml/commit/b06f390ecf75dcf24e8143aafdba533440326861](https://github.com/apache/systemml/commit/b06f390ecf75dcf24e8143aafdba533440326861)|
|[SYSTEMML-2413] Fix paramserv contention on DAG recompilation|[https://github.com/apache/systemml/pull/789](https://github.com/apache/systemml/pull/789)|[https://github.com/apache/systemml/commit/7027a532dff62304cbde8d18a3f45633c078423b](https://github.com/apache/systemml/commit/7027a532dff62304cbde8d18a3f45633c078423b)|
|[SYSTEMML-2416] Simplified paramserv aggregation service|[https://github.com/apache/systemml/pull/790](https://github.com/apache/systemml/pull/790)|[https://github.com/apache/systemml/commit/e7fccd1c764ec470f6460e4e3cec90913e606798](https://github.com/apache/systemml/commit/e7fccd1c764ec470f6460e4e3cec90913e606798)|
|[SYSTEMML-2403] Fix accuracy issue paramserv BSP batch updates|[https://github.com/apache/systemml/pull/791](https://github.com/apache/systemml/pull/791)|[https://github.com/apache/systemml/commit/63a1e2ac59f3201ab99a6e5e71636133eec96b1b](https://github.com/apache/systemml/commit/63a1e2ac59f3201ab99a6e5e71636133eec96b1b)|
|[SYSTEMML-2418] New paramserv distributed spark data partitioners|[https://github.com/apache/systemml/pull/793](https://github.com/apache/systemml/pull/793)|[https://github.com/apache/systemml/commit/8def2040be8e9704c8ee8083e8b949ffc0d74927](https://github.com/apache/systemml/commit/8def2040be8e9704c8ee8083e8b949ffc0d74927)|
|[SYSTEMML-2446] Fix paramserv model list cleanup for partial updates|[https://github.com/apache/systemml/pull/802](https://github.com/apache/systemml/pull/802)|[https://github.com/apache/systemml/commit/bca1f1c758b076ceb39febe3c4a6f8757655005d](https://github.com/apache/systemml/commit/bca1f1c758b076ceb39febe3c4a6f8757655005d)|
|[SYSTEMML-2420,2422] New distributed paramserv spark workers and rpc|[https://github.com/apache/systemml/pull/805](https://github.com/apache/systemml/pull/805)|[https://github.com/apache/systemml/commit/15ecb723e39e3154412ca8f8824c4554ee64ca35](https://github.com/apache/systemml/commit/15ecb723e39e3154412ca8f8824c4554ee64ca35)|
|[SYSTEMML-2420,2457] Improved distributed paramserv comm and stats|[https://github.com/apache/systemml/pull/808](https://github.com/apache/systemml/pull/808)|[https://github.com/apache/systemml/commit/e0c271fe4cb05d3a53eb0143ab298e26831d1ed7](https://github.com/apache/systemml/commit/e0c271fe4cb05d3a53eb0143ab298e26831d1ed7)|
|[SYSTEMML-2469] Performance distributed paramserv (partition/serialize)|[https://github.com/apache/systemml/pull/809](https://github.com/apache/systemml/pull/809)|[https://github.com/apache/systemml/commit/b586d16913196276d5bbd0c0828389aed7e4d9e3](https://github.com/apache/systemml/commit/b586d16913196276d5bbd0c0828389aed7e4d9e3)|
|[MINOR] Various paramserv refactorings and code cleanups|[https://github.com/apache/systemml/pull/814](https://github.com/apache/systemml/pull/814)|[https://github.com/apache/systemml/commit/382f847de6e33cdb5386b5eb5912eb5da0dff8d6](https://github.com/apache/systemml/commit/382f847de6e33cdb5386b5eb5912eb5da0dff8d6)|
|[SYSTEMML-2090] Language documentation for paramserv builtin function|[https://github.com/apache/systemml/pull/816](https://github.com/apache/systemml/pull/816)|[https://github.com/apache/systemml/commit/fb90e3bff41e9c0d58a80867243641faec437c09](https://github.com/apache/systemml/commit/fb90e3bff41e9c0d58a80867243641faec437c09)|
|[SYSTEMML-2299] Cleanup paramserv language API, incl defaults|[https://github.com/apache/systemml/pull/817](https://github.com/apache/systemml/pull/817)|[https://github.com/apache/systemml/commit/78e9d836ea16296fcf3bbd647b60638ce2bc24c3](https://github.com/apache/systemml/commit/78e9d836ea16296fcf3bbd647b60638ce2bc24c3)|

## Experiments:
While implementing the parameter server, we also launch the experiments for analyzing the performance and detecting the potential bugs proactively. All the experimental scripts and results have been pushed to this github [repository](https://github.com/mboehm7/paramserv).

## Documentation:
Here is the [link](https://edgarlgb.github.io/systemml/dml-language-reference.html#parameter-server-built-in-function) pointing to the documentation about paramserv function. Note that this is a snapshot of the Apache SystemML language documentation as of 08/05/2018.

## Presentation:
I have got a very precious opportunity to give a presentation about my work before the Apache SystemML community. Inside the provided slides, there is a complete overview of the project including what is parameter server, how to use paramserv function, the demonstration and the experimental result.
Please feel free to take a look on the [slides](https://drive.google.com/file/d/1KxVeck--Rfx8nw0eJfMGit-PlxAX00MO/view?usp=sharing).