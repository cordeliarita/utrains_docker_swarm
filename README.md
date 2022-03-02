# Docker Swarm Vagrant


# Docker Swarm

Docker Swarm is a Docker clustering solution, it turns multiple physical (or virtual) hosts into a one cluster, which practically behaves as a single Docker host. Swarm additionally gives you tools and mechiasms to easily scale your containers and create managed services with automatic load balancing to the exposed ports. 

Swarm uses [Raft Consensus Algortihm](http://thesecretlivesofdata.com/raft/) to manage the cluster state. Swarm can tolerate `(N-1)/2` failures and needs `(N/2)+1` nodes to agree on values. 

# Customize

By default `vagrant up` spins up 3 machines: `manager`, `worker1`, `worker2`. You can adjust how many
workers you want in the `Vagrantfile`, by setting the `numworkers` variable. Manager, by default, has address "192.168.10.2", workers have consecutive ips. 


If your provisioner is `Virtualbox`, you can modify the vm allocations for memory and cpu by changing these variables:

```ruby
vmmemory = 512
```

```ruby
numcpu = 1
```


`/etc/hosts` on every machine is populated with an IP address and a name of every other machine, so that names are resolved within the cluster. This mechanism is not idempotent, reprovisioning will append the hosts again. 

# Auto mode

By default, vagrant will create pure machines with docker installed. You can run 
`AUTO_START_SWARM=true vagrant up` to provision swarm automatically. You will get an already running Docker swarm cluster.

# Play

After starting swarm, you can use my testing Docker image to play with. It is called `darek/goweb` and is a super simple Web app, displaying the hostname, and a version. There are three tags: `1.0`, `2.0` and `latest`. They can be used to play with swarm rolling update feature. The container exposes port 8080. 

Go to the master node and start docker swarm:

```bash
   (host)# vagrant ssh manager
(manager)# docker swarm init --advertise-addr 192.168.10.2

docker swarm join \
--token SWMTKN-1-59h28hcbb8gzs2xs24oyh7hjvc7fp8skjzvnpw9cksmp96m4y2-35er9ai3u1f1ae5esb7x8l1hx \
192.168.10.2:2377
```

Now join the swarm on the nodes with the command from the manager, do it on both nodes. You can verify that nodes are in the cluster by doing `docker nodes ls` on the manager.

```bash
vagrant@manager:~$ docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
0rtuyz07e0wazmxvoed1llmx3 *  manager   Ready   Active        Leader
1iab1w9m5znyzmh7hpzyn7rrw    worker2   Ready   Active
4mmca5rxrxxhb0tb7s5du5hpe    worker1   Ready   Active
```


