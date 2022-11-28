# [Gossip Protocol](https://www.youtube.com/watch?v=8dz0riI13j8)

## Backgroung and The Problem:
* To communicate across many nodes in the cluster without using centralized coordination service;

## How it works:
* We send message to one/cuple of nodes in the cluster. Each 'infected node' rebrodcast message to next nodes; After a couple of round all nodes will get the message;


## In Cassandra database:
* Used to pass details about node membership/failure in a cluster
* Each node is gossiping with other nodes regarding it's own local load
* Each node is gossiping with other nodes about heartbits it receives from all other nodes
* All nodes use the most recent heartbeat timestamp to determine if other nodes are down

## Takeaways:
* Good for quickly broadcasting messages to many nodes with relatevly low overhead/coordination!
* Useful in large clusters for a failure detection
