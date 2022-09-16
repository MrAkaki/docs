# High Availability Mode with an embedded etcd database
## Install first Server
```bash
curl -sfL https://get.k3s.io | sh -s - server \
--token=YOUR-SECRET \
--tls-san your-dns-name --tls-san your-lbip-address \
--cluster-init
```
## Install additional Servers
```bash
curl -sfL https://get.k3s.io | sh -s - server \
--token=YOUR-SECRET \
--tls-san your-dns-name --tls-san your-lb-ip-address \
--server https://IP-OF-THE-FIRST-SERVER:6443
```

## Notes:
### Server only ( no workload )
If you want to set up a master server that only run server things (no workloads) can add this flag
`--node-taint CriticalAddonsOnly=true:NoExecute`

### Disable default ingress
K3S come by default with trafik as ingress, if you want to install another you can disable traefik adding this flag
`--no-deploy=traefik`
For my user= case I prefer to use [nginx](https://kubernetes.github.io/ingress-nginx/).

### Disable default internal load balancer
If you plan to install an loadbalancer can disable the buildint adding this flag
`--disable servicelb`
For my user case I prefer to use [MetalLB](https://metallb.org/).

### Example with all the flags
```bash
curl -sfL https://get.k3s.io | sh -s - server \
--token=YOUR-SECRET \
--tls-san your-dns-name --tls-san your-lbip-address \
--node-taint CriticalAddonsOnly=true:NoExecute \
--no-deploy=traefik \
--disable servicelb \
--cluster-init
```


## Get a registered Address
To achieve a high-available scenario you also need to load balance incoming connections between the server nodes.
This depends a lot of your current setup/network, if you dont have a load balancer check haproxy. #TODO

## Install Agents
You can still add additional nodes without a server function to this cluster.
```bash
curl -sfL https://get.k3s.io | sh -s - agent \
--server https://your-lb-ip-address:6443 \
--token YOUR-SECRET
```

# Some Knowloadge

## ETCD Fault Tolerance
The `--cluster-init` initializes an HA Cluster with an embedded etcd database. The fault tolerance requires an odd number, minimum three, nodes to function.

Total Number of nodes | Failed Node Tolerance
---|---
1|0
2|0
3|1
4|1
5|2
6|2
...|...

This is why most of the people recomends a cluster of 3/5/7 nodes


curl -sfL https://get.k3s.io | sh -s - agent \
--token=1tAlYeR3945adb89aSj \
--server https://172.22.89.20:6443