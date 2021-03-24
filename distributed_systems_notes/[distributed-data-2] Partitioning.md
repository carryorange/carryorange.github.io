# Partitioning
__Designing Data-Intensive Applications__ Chapter 6 and miscellaneous notes

## Partitioning of Key-Value Data
* Partitioning by key range
  * Cons
    * Possible skewed partitioning and hotspot
  * Pros
    * Easy range query
* Partitioning by hash of key
  * Cons
    * Not easy to do efficient range queries

### Skewed Workloads and Relieving Hot Spots
* No automatic approach yet, application is responsible for distributing the skewed workload

## Partitioning and Secondary Indexes
* Partitioning secondary indexes by document
  * Each partition uses local index
  * Scatter and gather
  * Examples: MongoDB, Riak, Cassandra, Elasticsearch, SolrCloud
* Partitioning secondary indexes by term
  * Use partitioned global index (and can be partitioned differently from the primary key)
  * Reads are more efficient, but writes are slower and more complicated
    * Thus updates to global secondary indexes are often asynchronous
  * Examples: Amazon DynamoDB, Riak's search feature, Oracle data warehouse

## Rebalancing Partitions
* Fixed number of partitions
  * Number of partitions >> number of nodes
  * Examples: Riak, Elasticsearch, Couchbase
* Dynamic partitioning
  * Split a partition  when it grows to exceed a configured size
    * New partition can be transferred to a different node
    * Number of partitions is proportional to the size of the dataset
  * Examples: HBase, RethinkDB
* Partitioning proportionally to nodes
  * Examples: Cassandra

## Request Routing
### Routing Strategies
* Allow clients to contact any node, and any node can do request routing
* Send all requests from clients to a routing tier (acting as a partition-aware load balancer)
* Let client be aware of the partitioning and the assignment of partitions to nodes

### Coordination service
* e.g. ZooKeeper
* Gossip protocol
  * e.g. Cassandra, Riak


## Consistent Hashing
* Map hash values to a `hash ring`
* Virtual nodes - to reduce variance
* Consistent hashing a special case of [rendezvous hashing](https://en.wikipedia.org/wiki/Rendezvous_hashing)