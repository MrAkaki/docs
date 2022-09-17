# DevCluster
This guide is for set up a K8S cluster over Docker containers, for local development/testing.

The idea is to get a configuration similar to what we get on a production cluster (with way less resources).

## Requirements
Have [Docker](https://www.docker.com/) installed.
Have [chocolatey](https://chocolatey.org/) installed.

## Install kubectl
*kubectl* is the management tool for k8s by default.
`choco install kubernetes-cli`

## Install k3d
K3D (K3s over docker) is an lightweight wrapper to run [k3s](https://k3s.io)(Lighweight K8s).

`choco install k3d`

## Configure the cluster 

### Create our first cluster
`k3d cluster create first_cluster`
As simple as that we get a k8s cluster(if can be called cluster) of 1 node.
You can check that is working running:
`kubectl get nodes`
For delete it just run:
`k3d cluster delete first_cluster`

### Create a more real cluster

`k3d cluster create second_cluster --servers 1 --agents 2`
You can check that is working runing:
`kubectl get nodes`
This will create a cluster(now it is) of 3 nodes, 1 server and 2 workers
But with this we can only deploy workload and will no be able to access it.
For delete it just run:
`k3d cluster delete second_cluster`

### Create a more real cluster with exposed ports
This will create a cluster(now it is) of 3 nodes, 1 server and 3 workers, with the ports 80 and 443 of the load balancer mapped to the host 8080 adn 8443 ports.
*NOTE:* Setup as many agents/workers as you want and your system can handle.
`k3d cluster create dev_cluster --servers 1 --agents 3 -p "8080:80@loadbalancer" -p "8443:443@loadbalancer"`

## Setting up the basics

