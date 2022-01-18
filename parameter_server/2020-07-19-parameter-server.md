---
layout: default
title: Parameter server part 1
parent: Parameter Server
nav_order: 1
---
Recently I spent some time looking at the news around a technology in distributed machine learning
called Parameter Server and would like to share what I read.

I knew about the parameter server when I worked on an OpenSource project Apache SystemML during GSoC 2018.
I designed and implemented a model training architecture using the data-parallel parameter server strategy 
including the model synchronisation strategy BSP and ASP upon Apache Spark.

From the result of the experiment, we can notice some issues: 

* Network contention: too much time wasted at transferring the data among the network during an iteration. This is caused
by the fact that in BSP, a lot of workers finish the gradient computation nearly at the same time and have to compete each other to send
 data to the parameter server. They use a very low bandwidth to communicate which is not efficient.
* Heterogeneous cluster: how to efficiently distribute the workload by taking the fact that there are some
servers less powerful than others into account.
* Straggling worker: the slowest worker to finish the calculation of gradients is a bottleneck
to speed up the whole training process. The issue is also caused by the fact that some less
powerful workers receive much more job than they can resolve in a reasonable time.
* Network topology: how the parameter server is connected to workers can also affect the efficiency

Here is what I read from the papers:

* Coded computation is a good solution to address the issue of straggling workers. The main idea is
to avoid waiting for the result coming from the straggler at an expense of replicating the 
training data across the workers. [[1]](https://arxiv.org/abs/1512.02673) 

* Non-persistent stragglers can also be utilised to complete a part of the gradient computation work. The work done by the slowest
worker will be discarded even though they compute a part of the result at least. In reality, those partial work
can be utilised to update the parameters. This paper proposes that workers can send the partial gradients to the server multiple times
during an iteration so that the work done by the straggler is not wasted. [[2]](https://arxiv.org/abs/1808.02240)

* A round-robin communication way is proposed by this paper to solve the network contention issue. Instead of transfering gradients in the network nearly at the same time, workers are
scheduled to do the communication in a round-robin way trying to mitigate the network contention. [[3]](https://www.cse.ust.hk/~weiwa/papers/chen-infocom19.pdf)
