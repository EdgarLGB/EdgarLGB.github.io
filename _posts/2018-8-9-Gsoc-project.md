---
layout: post
title: Summary of GSOC 2018 project [[SYSTEMML-2083]](https://issues.apache.org/jira/browse/SYSTEMML-2083)
---

##Overview

I'm Guobao LI, undergraduate of SCUT and graduate of Polytech Nantes. As a participant of GSOC, I have spent a very exciting summer with the Apache community and especially my mentor Matthias. My project (in [JIRA](https://issues.apache.org/jira/browse/SYSTEMML-2083)) is to design and implement the language and runtime of parameter server in Apache SystemML. In conclusion, the local and spark data-parallel parameter server including the update strategy (BSP, ASP), update frequency (Batch, Epoch), data partitioning schemes (DC, DR, DRR, OR) are implemented as expected and we achieve a good experimental result. And I'm happy to see that in the next release of SystemML, this new feature will be integrated.

##Pull Requests:

| features | PR link |
| ------------- |-------------|
| language extension | [SYSTEMML-2299](https://github.com/apache/systemml/commit/78e9d836ea16296fcf3bbd647b60638ce2bc24c3) [SYSTEMML-2084,2317-20](https://github.com/apache/systemml/commit/e270960ca41c7c0197373b53960cae6e7aca84ab) |
| local runtime | [SYSTEMML-2416](https://github.com/apache/systemml/commit/e7fccd1c764ec470f6460e4e3cec90913e606798) [SYSTEMML-2389](https://github.com/apache/systemml/commit/ebfe327e4474d0692679efc847216131d38777e8) [SYSTEMML-2381](https://github.com/apache/systemml/commit/0871f260e6fc6d6fed57d2cc249bf4d8beb0a31f) [SYSTEMML-2380](https://github.com/apache/systemml/commit/c10e509a78232f8cacfa9c7485395792d6af24e8) [SYSTEMML-2364,66-88](https://github.com/apache/systemml/commit/69ef76c06a6aea0c1e9b6750a37997a950a81794) [SYSTEMML-2359](https://github.com/apache/systemml/commit/51057e4712d6ab9a190a9c1f8e9f36d48a8a1fd5) [SYSTEMML-2344,48,49,52](https://github.com/apache/systemml/commit/d44b3280f2132deb303e955ff5b9a17daac4c31e) [SYSTEMML-2085](https://github.com/apache/systemml/commit/97018d4e688ba7eeaaa4567ca1e174a3c5525468)  |
| spark runtime | [MINOR](https://github.com/apache/systemml/commit/382f847de6e33cdb5386b5eb5912eb5da0dff8d6) [SYSTEMML-2420,2457](https://github.com/apache/systemml/commit/e0c271fe4cb05d3a53eb0143ab298e26831d1ed7) [SYSTEMML-2420,2422](https://github.com/apache/systemml/commit/15ecb723e39e3154412ca8f8824c4554ee64ca35) [SYSTEMML-2419](https://github.com/apache/systemml/commit/cffefca30e89ce249c3030d23123c1b3aba1757a) [SYSTEMML-2418](https://github.com/apache/systemml/commit/8def2040be8e9704c8ee8083e8b949ffc0d74927)  |
| bug fix | [SYSTEMML-2469](https://github.com/apache/systemml/commit/b586d16913196276d5bbd0c0828389aed7e4d9e3) [SYSTEMML-2446](https://github.com/apache/systemml/commit/bca1f1c758b076ceb39febe3c4a6f8757655005d) [SYSTEMML-2403](https://github.com/apache/systemml/commit/63a1e2ac59f3201ab99a6e5e71636133eec96b1b) [SYSTEMML-2413](https://github.com/apache/systemml/commit/7027a532dff62304cbde8d18a3f45633c078423b) [SYSTEMML-2392/8,2401/2/6](https://github.com/apache/systemml/commit/b06f390ecf75dcf24e8143aafdba533440326861) [MINOR](https://github.com/apache/systemml/commit/96954b4ca980d928bf2c4c4eb012efe0188a4ac6)|
| documentation | [SYSTEMML-2090](https://github.com/apache/systemml/commit/fb90e3bff41e9c0d58a80867243641faec437c09) |

##Experiments:
While implementing the parameter server, we also launch the experiments for analysing the performance and detecting the potential bugs proactively. All the experimental scripts and results are pushed to this github [repository](https://github.com/mboehm7/paramserv).

##Documentation:
Here is the [link](https://edgarlgb.github.io/systemml/dml-language-reference.html#parameter-server-built-in-function) pointing to the documentation about paramserv function.

##Presentation:
I've got a very precious opportunity to give a presentation about my work before some people. Inside the provided slides, there is a complete overview of the project including what is parameter server, how to use paramserv function, the demonstration and the experimental result.
Please feel free to take a look on the [slides](https://drive.google.com/open?id=1rFoSaH94ssbD-ySyzxPcNbQdufMaq4m9M7HMBwimKlk).