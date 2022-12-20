# [**Stream Processing**](https://www.youtube.com/watch?v=XFAx53P9NWE)

# Backgroung:
* We have a lot of data coming and we want to process it asyncronosly in the background
* Streaming works on unbounded set of data
* Messages are created by **Producer** node and eventually handeled by at least one **Consumer** node
* Messages can be sent from **Producer** to **Consumer** directly but it's a common practice to use **Message Broker** to buffer these messages in a centralized place

# Message Brokers:
* A type of database that handels streaming, both producers and consumers can connect to it. Typically puts messages in a queue if there are many of them
* Some will keep messages durably, others will delete them after they are successully consumed.
* Two delivery patterns: 
    * Fan Out - send message to all consumers
    * Load Balancing - one message per consumer
# In order messages:
* We can garantee streaming messages in order, but cannot garantee in order processing in case of multiple consumers(one consumer can fail and next message can be proceessed by another consumer faster)
 
 # Types of Message Brokers:
 * **In Memory** - no persistense (messages are hold in memory)
    * Can be done on something like Redis instance
    * Messages that are acknowledged by Consumer are deleted
 * **Log Based** - on disk (messages are hold on disk in some sort of the log)
    * Every new message sent to an append only log on disk
    * Log can be partitioned and replicated to improve performance and fault tolerance
    * Producer based on a hash function can put messages to proper partition
    * By having One Partition per Consumer we can make sure all messages will be handled in proper order
        * One hard to handle/process message can slow down partition processing
# Common Uses of Streams:
* Logging and Metrics
* Change Data Capture
* Event Sourcing

# Stream Joins - ?
* Stream_Stream joins - Joining two diff events in a stream
* Strem_Table joins
* Table_Table joins

# Fault Tolerance - ?
* Every Message processed at least once
* Messages not processed more than once


