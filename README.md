# Multi-clutser

Multi cluster configurations are hard. Hard because there's a whole series of "little details" that will make that a totally valid solution be invalid when those details change. 

This repo hosts the different configuration and files for [multicluster.net](https://multicluster.net) where I try to group and documenr all the different configurations and gotchas I find along the way.

## What is multi cluster, anyway?

Multi-cluster is basically running two or more clusters in paral.lel. The idea is that you will deploy a cluster add the appliation endpoint to a global load balancer and... that's it, pretty much.

The diagram could be something like this:


![Multi cluster diagram](https://github.com/ipedrazas/multicluster.net/raw/master/assets/diagram01.png "Multi cluster diagram")


Notice that the top entry is a `GLB` (Global Load Balancer). A GLB allows to distribute traffic globally. We're going to be using a [Google cloud load balancer](https://cloud.google.com/load-balancing/) and a [Cloudflare load balancer](https://www.cloudflare.com/load-balancing/)

Each load balancer is slightly different, so having a consistent terminology is very important. We will talk about:

* Global Load Balancer: Public entry point for our application
* Backend pool: Application / Pods
* Ingress: Public entry point into a kubernetes cluster

# Factors that affect High availa

## Questions

**What's the difference between `multi-cluster` and `multi-cloud`?**

There's not much difference really. Multi-cloud is multi-cluster, but not the other way around. The key of multi-cluster is to undertsand where the boundaries and responsabilities of  cluster are.

**Does multi-cloud support inter-cluster communication?**

Inter-cluster communication is covered by cluster federation, but this is our of scope of the multi-cluster approach. Bear in mind that to have intercluster communication you need 2 things: routable access from one cluster to the other and, a control plane that holds the state of the federation. Multi-cluster is a more lightweight approach. Once your requests is inside of a cluster, does not leave that cluster.

This helps with operating and managing the clusters.
