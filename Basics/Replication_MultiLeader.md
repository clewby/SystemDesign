# [Multi Leader replication](https://www.youtube.com/watch?v=1BXCxpcsmzc)

## Overview:
* Multi Leader Database, possibly accross datacenters
* Leader Dbs send writes to each other and do conflict resolution
* Reads - can come from any database

## Replication Topologies:
* Star - can easly fail - center node crash 
* Circle - can easly fail - any node crash for circle
* All-to-All - writes can be sent out of order due to network race conditions

## Multi Leader Replication Tradeoffs:
* Pros:
    * By havind a leader in each data center, can make app more resilient
    * Writes are scalable / not limited by a single node
* Cons:
    * Write conflicts between multiple leaders

## Conflict Resolution:
* **Conflict Avoidance**:
    * All writes for the same item of data go to the same node/leader
    * Not always possible if leader  is down
* **Last Write Wins(LWW)**:
    * Pros:
        * Very easy to implement
    * Cons:
        * Do we use client or server time?
        * Clock skew on servers means that their times are not exactly sync
* On read:
    * When database detects two concurent writes, store both writes
    * Next read databse returns both values to the user for a manual merge values and store resulting merge in the databse
* On write:
    * When database detects conflicting writes, call snippet of code to merge them (app specific)
    * Use conflict free data types

## Detecting Conflicts:
* **Concurent write** if each client did not know about write of the other when they made the write
* If one client knew about about the write of the other we wouuld have a relationship between two
* Concurency it's not about phisical time it's a knowledge about dependencies

## Version Vectors:
* For each key: keep track of increasing version number for each key for each replica