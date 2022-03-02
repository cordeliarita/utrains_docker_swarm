# Docker Swarm Vagrant


# Docker Swarm

Docker Swarm is a Docker clustering solution, it turns multiple physical (or virtual) hosts into a one cluster, which practically behaves as a single Docker host. Swarm additionally gives you tools and mechiasms to easily scale your containers and create managed services with automatic load balancing to the exposed ports. 

Swarm uses [Raft Consensus Algortihm](http://thesecretlivesofdata.com/raft/) to manage the cluster state. Swarm can tolerate `(N-1)/2` failures and needs `(N/2)+1` nodes to agree on values. 

# Customize

By default `vagrant up` spins up 3 machines: `manager`, `worker1`, `worker2`. You can adjust how many
workers you want in the `Vagrantfile`, by setting the `numworkers` variable. Manager, by default, has address "192.168.10.2", workers have consecutive ips. 

