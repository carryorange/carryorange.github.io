# Replication
__Designing Data-Intensive Applications__ Chapter 5 and miscellaneous notes

## Theme
handle changes to replicated data

## Popular algorithms
  * Single leader
  * Multiple leaders
  * Leaderless

## Leaders and Followers
### Synchronous vs asynchronous
  * All followers being synchronous is in practical
  * Synchronous usually means _semi-synchronous_: one follower is kept synchronous, and the others are asynchronous
  * Often leader-based replication is completely asynchronous

### Handling node outages
* Leader failure: failover process
  1. Determine the leader has failed - use timeout
  2. Choose a new leader - election algorithm
  3. Reconfigure the system to use the new leader

### Implementation of Replication Logs
* Statement-based replication
  * Idea: Log every write request (statement)
  * Cons
    * Any statement that calls a nondeterministic function, e.g. NOW() or RAND(), is likely to generate a different value on each replica
    * The same execution order of concurrent transactions
    * Nondeterministic side effects (e.g. triggers, stored procedures, user-defined functions etc.)
  * Pros
    * Compact
  * Example: Old version of MySQL
* Write-ahead log (WAL) shipping
  * Idea: use the storage engine's WAL log
  * Cons
    * The log describes the data on a very low level -> replication closely coupled with storage engine -> hard to upgrade storage engine on one node without impacting others
  * Example: PostgreSQL, Oracle
* Logical (row-based) log replication
  * Idea: similar to WAL shipping, but use a higher level log format (logical log) instead
  * Example: MySQL binlog when configured to use row-based replication
* Trigger-based replication
  * Idea: application-specific
  * Cons
    * greater overheads, prone to bugs and limitations
  * Pros
    * Flexible

### Problems with Replication Lag
* Reading your own writes
  * Problem scenario
    * Write to leader, read from follower where the write hasn't reached
  * __Consistency requirement__
    * read-after-write consistency: a user sees their _own_ writes (no promise about other users' updates)
* Monotonic reads
  * Problem scenario
    * User reads from different replicas that are not in sync, and may see things moving backward in time.
  * __Consistency requirement__
    * Monotonic reads: for several reads in sequence, users don't see older data appear after newer data
* Consistent prefix reads
  * Problem scenario
    * A user reads others' writes in a wrong order that violates causality
    * This is a particular problem in _partitioned_ databases
  * __Consistency requirement__
    * Consistent prefix reads: if a sequence of writes happens in a certain order, then anyone reading those writes see them appear in the same order.

## Multi-Leader Replication
* Use case
  * Multi-datacenter operation
  * Clients with offline operation
    * offline (local) database also acts as a leader
  * Collaborative editing
* Notice: multi-leader replication is considered dangerous territory that should be avoided if possible.

### Handling Write Conflicts
* Conflict avoidance
* Converging to a consistent state
  * Last write wins (LWW) - popular but prone to data loss
  * Merge writes
  * Keep all conflicts and let application code/user resolve them

### Leaderless Replication
* All replicas accept writes from any clients.

### Writing to the Database When a Node Is Down
* As long as a some writes are successful, clients think the write is successful.
* Handling failed nodes
  * Read repair
    * client makes a read from several nodes in parallel, and detect any stale response
  * Anti-entropy process
    * Background process that looks for differences between replicas.

### Quorums for Reading and Writing
* Setup
  * If there are `n` replicas, every write must be confirmed by `w` nodes, and we must query every `r` nodes
* Guarantee up-to-date reads
  * `w + r > n`
  * Fault tolerance: 
    * e.g. If `w < n`, we can still process writes if some nodes are unavailable
* Limitation
  * Concurrent writes: not clear which happens first
  * Concurrent write and read: read may not get the latest write
  * A write succeeds on some replicas but fails on others - unsuccessful writes roll back but successful ones won't
* Monitoring leaderless replication
  * No one satisfying solution

### Sloppy Quorums
* When failure happens, write to available nodes that may not be in the original quorum.

### Detecting Concurrent Writes
* Problem scenario
  * Events may arrive in a different order at different nodes
* The `happens-before` relationship
  * Event `A` `happens before` event `B` means `B` knows about `A`
  * If `A` doesn't happen before `B` and `B` doesn't happen before `A` then they are _concurrent_
* Version vectors
  * To capture dependencies between operations when there are multiple replicas