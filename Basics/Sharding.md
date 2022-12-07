# [**Database Sharding**](https://www.youtube.com/watch?v=IM6urSjtMrQ)

# Overview:
* In large systems, we are dealing with a huge amount of data and as a result tabels may become too big / perform to many queries for one single machine
* **Sharding** is splitting table/data up into many chunks to go on various database nodes
* Sharding = Prtitionig. But sometimes when people say sharding they mean splitting data all accross multiple databases. But
in case of partition they often mean splitting data accross multiple tabels of the same database
* Sharding is often used in conjunction with replication

# Objectives of sharding:
* A relatively similar amount of data for each node
* A relatively similar amount of reads/writes to the data for each node
* If we are unable to achieve this, certain nodes will be data overloaded (**skew** node) or read/write overloaded (**hot spot** node)

# Methodologies for Partition/Sharding: 
* **By range of keys** - take a set of keys devide it into ranges and assign partition to each range
    * Keep keys sorted within partition to best support range queries
    * Not necessarilly even ranges of keys, some ranges may be hotter
    * **Pros**
        * Simple and allows effective range queries
    * **Cons**
        * Can easily lead to hotspots
        * Have to actually determine the ranges to make sure they are relatively even in data and load
* **By ranges of hash keys** - take the hash of the key and put it into proper partition
    * **Pros**
        * Keys are evnly distributed between nodes (assuming good hash function)
    * **Cons**
        * No more range queries on the partition key, have to check every partition
        * If a hash key has a lot of activity will still lead to hot spots

# **Secondary indexes**:
* **Local Indexes**
    * Holds a secondary index that only holds rows from the partition the index is located on
        * **Pros**
            * Fast write - all data about index is stored locally on the partition
        * **Cons**
            * Slow on read - if using a secondary index have to query every partition to accumulate the index results
* **Global Indexes**
    * The index can contain references to data on any partition
        * **Pros**
            Fast on read because all data for that index is being kept on one partition node
        * **Cons**
            * Slow on write  - need to write to multiple partitions to update all of the various secondary indexes
            * May require a distributed transaction (imagine the case one write succeeds and the other fails)

# **Rebalancing Partitions**:
The goal is to keep the majority of the keys in the same place and only move a few from each node so we do not use a ton of bandwidth remapping every key. While rebalancing is happening nodes should accept reads and writes

* **Fixed Number of Partitions** - we have fixed number of partitions no matter how many nodes there are.
    * Choose a number of partitions that is reasonable
        * If low, each partition will get too big and we will not be able to scale the application further. Additionally transferring the partition to another node will take a super long time
        * If to high, there will be a lot of verhead on disk devoted to each partition
    * If your dataset is going to vary significantly in the future, maybe this isn't for you

* **Dynamic Partitioning** - some databases will adjast partition ranges dynamically so they can reduce hot spots as the data access patterns change over time
    * Once a partition becomes too big, it is split into two pieces and ono is assigned to another node
    * Sometimes dynamic partitioning is not good because if database incorrectly assumes a node is down, when there is actually just network delay, it will repartition leading to more strain on the network

* **Fixed Number of Partitions per Node**
    * Each node has a certain number of partitions on it which grow in size proportionally to the dataset
    * If a new node joins the cluster it will split some of the partitions on existing nodes into two pieces, and take those for itself
    * Very similar to consistent hashing algorithm to avoid unfair data splits

# **Request Routing**
    * Zoo Keeper - coordination service. Key range -> Partition -> Node -> IP address
    * Gossip protocol - make request to any node in cluster and it can redirect you to node with partition that you need