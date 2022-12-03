# [Leaderless Replication](https://www.youtube.com/watch?v=atDe7qka6q8)

## Overview:
* Any replica can accept writes from client
* Send reads and writes to all nodes in parallel, once predefined threshold of nodes return a success value, the client told read/write was successful

## Keep data up to date:
* **Anti-Entropy**:
    * Background process that looks at multiple nodes and their stored values, use version numbers to make sure each replica holds most up to date data
* **Read Repair**:
    * Since we are reading from multiple replicas in-parallel, take most up to date piece of data and propogate it to the other replicas that had outdated data

# Quorums: 
* **Problem** - Can we guarantee that at least one of the replicas we read from has most up to date data?
* **Idea**: lets imagine we have **n** database replicas
    * Client only has successful write if at least **w** nodes are writen to
    * Client only has successful read if at least **r** nodes are read from
    * If **w + r > n** at least one node that is read from will have an updated copyof the data and this read can repair others
    * Typically, **n** set to be an odd number and **w = r = (n + 1) / 2**

* **Quorums are not perfect**:
    * Data can be asyncronosly written to **w** nodes to make a quorum, but fewer than **w** writes succeed. Client told write has been failed. But in reality some nodes dont undo write
    * If we restore node/s with old values, we might lose up to date values
    * Write conflicts
        * 
    * Sloppy quorums
        * 

# Conclusion:
* **Pros**:
    * Can work quite well with having quorum write parameter to be small enough that all writes can go to one datacenter
* **Cons**:
    * Slower reads as a result of queries to multiple nodes (if using read repair)
    * Have Write conflicts
    * Quorums are not perfect, provides illusion of strong concistency when in reality this is ofthen not true