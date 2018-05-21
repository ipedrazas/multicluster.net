# Multi-clutser

Multi cluster configurations are hard. Hard because there's a whole series of "little details" that will make that a totally valid solution be invalid when those details change. 

This repo hosts the different configuration and files for [multicluster.net](https://multicluster.net) where I try to group and documenr all the different configurations and gotchas I find along the way.

## What do we understand by High Availablity?

The undderlying idea behind multi-cloud or multi-cluster is to increase availability of our applications. There are a few things to consider when aiming for a high availability.


* Size of the cluster or Number of Nodes: small clusters are more brittle than bigger clusters. What is the right size of a cluster? to answer this question you should think on how much sapre capacity your cluster has and how many nodes lost that spare capacity can handle.
* Number of replicas per application: how many replicas should you run per application?
* HPA (horizontal pod autoscaler): lower and upper boundaries are as important.
* affinity/antiaffinity rules: how toplace/schedule your pods. These settings help with placement: spreading the pods across different nodes or bin packing them. Be councious of the number of nodes and the number of replicas (with or without HPA).
* Readiness/Liveness Probes: Defining proper probes is critical for the good health of a cluster. Bear in mind the implications of long/expensive health checks
* Requests/Limits: specifying the requested resources helps the scheduler to place the pod in a certain node. Specifying the limits will help to guarantee that your application behaves well with the other citizens.


## What is multi cluster, anyway?

Multi-cluster is basically running two or more clusters in paral.lel. The idea is that you will deploy a cluster add the appliation endpoint to a global load balancer and... that's it, pretty much.

The diagram could be something like this:


![Multi cluster diagram](https://github.com/ipedrazas/multicluster.net/raw/master/assets/diagram01.png "Multi cluster diagram")


Notice that the top entry is a `GLB` (Global Load Balancer). A GLB allows to distribute traffic globally. We're going to be using a [Google cloud load balancer](https://cloud.google.com/load-balancing/) and a [Cloudflare load balancer](https://www.cloudflare.com/load-balancing/)

Each load balancer is slightly different, so having a consistent terminology is very important. We will talk about:

* Global Load Balancer: Public entry point for our application
* Backend pool: Application / Pods
* Ingress: Public entry point into a kubernetes cluster


## Questions

**What's the difference between `multi-cluster` and `multi-cloud`?**

There's not much difference really. Multi-cloud is multi-cluster, but not the other way around. The key of multi-cluster is to undertsand where the boundaries and responsabilities of  cluster are.

**Does multi-cloud support inter-cluster communication?**

Inter-cluster communication is covered by cluster federation, but this is our of scope of the multi-cluster approach. Bear in mind that to have intercluster communication you need 2 things: routable access from one cluster to the other and, a control plane that holds the state of the federation. Multi-cluster is a more lightweight approach. Once your requests is inside of a cluster, does not leave that cluster.

This helps with operating and managing the clusters.
