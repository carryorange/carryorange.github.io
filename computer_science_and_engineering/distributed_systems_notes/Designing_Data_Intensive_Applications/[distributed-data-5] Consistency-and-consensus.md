# Consistency and Consensus
__Designing Data-Intensive Applications__ Chapter 9 and miscellaneous notes

## Consistency Guarantees
### Eventual consistency
* A very weak guarantee that is not very useful
  * It guarantees system eventually converges but doesn't say WHEN

### Linearizability
* Basic idea
  * Make a system appear as if there were only on e copy of the data, and all operations on it are atomic
  * For formal definition, refer to
    * [Wikipedia](https://en.wikipedia.org/wiki/Linearizability#Definition_of_linearizability)
    * [Original paper](https://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
* Linearizability vs serializability
  * A database may provide _both_ (so they are two different guarantees)
    * aka __strict serializability__ or __strong one-copy serializability__(strong-1SR)
  * Linearizability of serializability implementations
    * Actual serial execution and 2PL - typically also linearizable
    * Serializable snapshot isolation - NOT linearizable
      * Reads from snapshots are not linearizable since it exclude more recent writes
* Use cases
  * Locking and leader election
  * Constraints and uniqueness guarantees
  * Cross-channel timing dependencies
* Implementation
  * Single leader replication - potentially linearizable
  * Consensus algorithms - linearizable
    * Some consensus algorithms bear a resemblance to single-leader replication
  * Multi-leader replication - NOT linearizable
  * Leaderless replication - probably NOT linearizable
    * _May_ be Linearizable when quorum reads and writes are required
    * LWW conflict resolution, sloppy quorum etc. are not linearizable
* Cost of linearizability
  * The CAP theorem

## Ordering Guarantees
### Ordering and Causality
* Causality imposes ordering on events
* Causally consistent system - a system that obeys the ordering imposed by causality
* Causal order is NOT a __total order__
  * Total order - _any_ two elements from the set can be compared
  * Linearizability has a __total order__
  * Causality has a __partial order__ (allow concurrent events)
* Linearizability implies causality, but not the other way around
* Causal consistency is the _strongest_ possible consistency model that does not slow down due to network delays, and remains available in the face of network failures (CAP theorem doesn't apply)
  * It's proven that to have linearizability, a system needs to slow down proportional to the network delay time (which is highly variant and can be large)

### Sequence Number Ordering
* Non-causal sequence number generators
  * When there are multiple nodes, it's easy to generate increasing sequence numbers on each node
  * But these sequence numbers are NOT consistent with causality across nodes
* Lamport timestamps
  * Each node has an ID and a counter
  * Lamport timestamp: (counter, nodeID)
  * Sync counter across nodes to ensure causality
* Timestamp ordering is necessary but not sufficient (to build interesting systems)
  * e.g. unique id creation cannot be solved by timestamp ordering alone

### Total Order Broadcast
* Properties
  * Reliable delivery
    * No messages are lost - if a message is delivered to one node, it should be delivered to all nodes
  * Totally ordered delivery
    * Messages are delivered to all nodes in the same order
* Implementing linearizable storage using total order broadcast
* Implementing total order broadcast using linearizable storage
* Relationships
  * Linearizable compare-and-set (or increment-and-get) register and total order broadcast are both _equivalent to_ consensus.
  * Solve one, then the solution can be transformed into a solution for the others


## Distributed Transactions and Consensus
### Two-phase commit
* 2PC is a kind of consensus algorithm - but not a very good one
* Algorithm
  * Need a coordinator (aka transaction manager)
  * Phase 1 - prepare
    * If any of the nodes replies `abort`, the transaction is aborted
    * Coordinator needs to write the decision to its transaction log (on disk) - __commit point__
  * Phase 2 - commit
    * Coordinator needs to retry _forever_ until all commit requests succeed.
  * Coordinator failures
    * before prepare phase (phase 1) - a participant is free to abort
    * after prepare phase - a participant must wait (the state is called _in doubt_ or _uncertain_)
* 2PC is a __blocking__ atomic commit protocol

### Three-phase commit
* Nonblocking commit protocol, but assumes _bounded_ network delay.
* In general, nonblocking atomic commit requires a perfect failure detector (a reliable mechanism for telling node crashes)


## Fault Tolerant Consensus
* Best-known fault-tolerant consensus algorithms
  * Viewstamped replication (VSR) [94,95]
  * Paxos [96-99]
  * Raft [22,100,101]
  * Zab [15,21,102]
  * They are NOT the same [103]

## Membership and Coordination Services
* Features provided by ZooKeeper
  * Linearizable atomic operations
  * Total ordering of operations
  * Failure detection
  * Change notification