# [**Single Leader Replication*](https://www.youtube.com/watch?v=X687PvgOWzQ&t)

# Overview:
* All writes go to ONE Master/Leader database/node
* Master database sends list of writes to other(Slave/Follower) databases via replication log
* Reads can come from any database/node

# Dealing with a crash:
* Adding new follower - simple
* Follower crash - simple
* Leader crash(failover) - find a new leader (perhaps the most up to date follower)

# Failover issues:
* Some writes from old Leader can be lost (durability problem)
* If the old leader comes back we need to make sure it stops accepting writes and doesn't think it still Leader (double brain problem)

# Types of Single Leader Replication:
* **Synchronous**:
    * Client doesn't receive success message until all replicas complete the write
    * Strong Consistency
        * Data always up to date, but writes take much longer
* **Asynchronous**:
    * Client receives success message after master completes the write
    * Eventual consistency
        * Data will be up to date eventually, faster writes, client might read stale data

# Dealing with Eventual Consistency:
* **Read your own writes**
    * Problem: client doesn't see write after making it
    * Solutions:
        * Read user specific data from Master
        * During some amount of time forse client read data from Master
* **Monotonic Reads**
    * Problem: reads look like they are going back in time
    * Solution: having each client read from the same replica
* **Consistent Prefix Reads**
    * Problem: sequence of writes is lost due to being on different partitions
    * Put related writes on same partition

# Replication Log Implementation Options
* **Statement based /  Copy over SQL statements**
    * Bad - some SQL statements are not deterministic/constant (NEWID, CURRENTTIME)
* **Write ahead log**
    * Ok - but if changing database engine some data might different locations on disk
* **Row based / logical log**
    * Good - describes what rows were inserted, updated or deleted
* **Trigger based**
    * Happeng on the application layer (triggers, stored procs). To track portion of changes - fields, tabels


