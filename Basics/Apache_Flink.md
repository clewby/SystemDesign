# [**Apache Flink**](https://www.youtube.com/watch?v=YgxiRs4y4q0&t)

# Backgroung:
* Flink is a software that you run on a cluster of nodes in order to provide statefull computations on a stream processing. Not a message broker but stream Consumer
* Flink allows distibuted processing of a message queue, while also persisting state about the seen messages among the nodes in a cluster.

# State in Stream Processing:
* Why is it so important to keep state if each evnt should be processed independently?
    * Grouping events by time
    * Holding a local copy of database table for a stream enrichment
        * Table updated over time by ingesting changes from a stream
    * Tinder use case
        * Every like placed into a stream. Flink processes likes and stores them in state. If another like comes from opposite user - we have a match
        * Flink forwards match data into a second stream

# Fault Tolerance:
* Each node has it's own local state and we want to make sure if one of the nodes crashes, we can recover it's local state as well as process any messages that it had yet to process on a new node. It does so by occasionally storing checkpoints of node state to durable storage such as HDFS or S3.

# Architecture:
* Flink uses Log Based message brokers such as Kafka as it's data source.
* Job Manager sends "checkpoint barrier" message to all nodes in cluster
    * For each node - Give me your local state and offset of the message that you have processed
    * Job Manager saves state of each node to distributed file system
    * If node crashes we can restore it to prev state from checkpoint state and start processing messages from corresponding offsets
        * Some messages might be processed more than once!
    * Snapshots created in the background, considered successfull if all nodes report successfully storing local snapshot to Job Manager

# Changing Cluster Size:
* If we want to change the size of the cluster, we have to redistribute state over the nodes
* States/Checkpoints are just basically lists of keys/values, they can be easily re-partitioned using some sort of range schema

# Conclusion:
* Unlike Spark, which process messages in microbatches, Flink is fully real time stream processing engine which provides ability to make statefull computations with a high availability