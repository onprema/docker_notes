## Networking

`docker network ls` lists all networks
`docker network inspect <network>` shows network configs
`docker network create --driver` creates a new network
`docker network connect <net1><net2>` connects two networks
`docker network disconnect <net1><net2>` disconnects two networks

`bridge` (aka `docker0`) is the default network that bridges containers to the
physical network the host is connected to. This is a virtual network which is
NAT'ed behind the Host IP.

NAT = Network Address Translation. 

`host` network is a special network that skips the virtualization and has
containers directly attach to it.

The network **driver** is builtin or 3rd party extension that gives you virtual
network features. By default, it is the bridge driver when you do `docker
network create my_network`

Docker uses the container names as the hostname for containers talking to
eachother. This allows you to do something like `docker container exec -it
nginx1 ping nginx2` instead of `docker container exec -it nginx1 ping
172.12.???.???`. The container name is the host name.

It's probably best to make a new network for the container you want to talk to
eachother rather than `--link`ing them together.
