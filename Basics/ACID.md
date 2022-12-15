# [ACID / Transactions](https://www.youtube.com/watch?v=NpQ-S89jVzE)

# Overview:
* Transaction is a bundle of read/write operations that either all succeded or they are all failed
* Entire point of having transactions is to implement ACID

# ACID:
* **Atomicity** - If a client makes several writes, but the fault occurs after only some of the writes are completed, the existing completed writes will be rolled back
* **Consistency** - Refers to an application-specific notion of the database being in a good state. You have certain statements about your data that must be true. More related to app property than database property
* **Isolation** - Concurently executed transactions are isolated from one another(serializability). Each transaction can pretend that it is the only transaction running on the entire database
* **Durability** - Once transaction is completed successfully, any data it has written will not be forgotten, even if there is a hardware fault or the database crashes

# Implementing Serializable Isolation

* **Actual Serial Execution** - Implement all database queries on a single thread. Thi is possible under the follwing conditions:
    * In memory database
    * Use a stored procedure
    * Attempt to keep all transactions limited to one partition
** **Pros**
    * Very simple to implement, non anlytics transactions are mostly short
** **Cons**
    * Throughput limited to a single CPU core, have to use stored procs(hard to version control and can be in wierd languages)

* **Two Phase Locking** - Each object has a lock on it, which can be held in either shared mode or exclusive mode. Multiple transactions can concurently read from a row if they are holding the lock in shared mode, but if they want to write to it they must grab the lock in exclusive mode. They can only do that if no other transactions are currently holding a shared or exclusive lock on the object



