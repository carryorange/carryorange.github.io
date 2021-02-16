# Transactions
__Designing Data-Intensive Applications__ Chapter 7 and miscellaneous notes

## ACID
* Atomicity
  * Property of a single transaction - all or nothing
* Consistency
  * Property of the application - invariants the system should stick to
* Isolation
  * Property of concurrent transactions - they don't interfere with each other
* Durability
  * Property of the database

## Weak Isolation Levels
### Read Committed
* Guarantees
  * No dirty reads - only see committed reads
  * No dirty writes - only overwrite data that has been committed
* Implementation
  * No dirty writes -> Row-level locks
    * hold the lock until the transaction is committed or aborted
  * No dirty reads -> remember old value and new value
    * Only return new value after it's committed
* Unsolved issues
  * Non-repeatable read (aka __Read skew__)
    * Concurrent reads that are not consistent with application invariants
    * This happens when one read gets an old value whereas one read gets a new value in one transaction (assuming there's another write transaction in between)
* Examples
  * Default isolation level in PostgreSQL, Oracle 11g.

### Snapshot Isolation (aka Repeatable Read)
* Context
  * Read-only transaction when there are concurrent writes
* Guarantees
  * Each transaction reads from a __consistent__ snapshot of the database
* Implementation
  * Multi-version concurrency control (MVCC)
    * Each transaction has a unique id that's ever-increasing
    * Each object is tagged with transaction id
* Unsolved issues
  * Lost updates, e.g. concurrently increasing a counter
* Examples
  * PostgreSQL, MySQL with InnoDB
  * Commonly used in long-running, read-only queries such as backups and analytics

### Preventing Lost Updates
* Context
  * Concurrent read -> modify -> write
* Solutions
  * Atomic write operations
    * Prevent application from using read-modify-write cycle
    * DB implements this by using exclusive locking
  * Explicit locking
    * Let the application lock the objects to be updated
  * Automatically detecting lost updates
    * DB transaction manager detects lost updates and forces retry
    * This can be done efficiently with snapshot isolation
  * Compare-and-set
  * Conflict resolution and replication
* Unsolved issues
  * __Write skew__ and phantoms
    * Concurrent writes that are not consistent with application invariants
    * The concurrent writes may be updating different objects 
      * lost updates becomes a special case of write skew when concurrent writes try to update the same object
    * __Phantom__: a write in one transaction that changes the result of a search query in another transaction
      * Snapshot isolation avoids phantoms in read-only queries, but in read-write transactions can lead to write skew

## Serializability
* Guarantees
  * Concurrent transactions have the same outcomes as if they were executed serially
### Approach 1: Actual Serial Execution
* Examples: 
  * Redis, VolgDB/H-Store

### Approach 2: Two-Phase Locking (2PL)
* Implementation
  * Using _shared_ lock or _exclusive_ lock
  * Two-phase
    * First phase - when transaction is executing: require lock
    * Second phase - when transaction is ended: all locks are released
  * Preventing write skews/phantoms
    * Predicate locks
      * Concept
        * A lock belonging to all objects that match some search conditions (corresponding to the phantom)
        * A predicate lock applies to objects that do not yet exist in the database (but matches the search condition)
    * Index-range locks
      * A simplified approximation of predicate locking
        * To improve performance
        * May lock a bigger range of objects than is strictly necessary by predicate locks
* Cons
  * Performance
  * More frequent deadlock that requires one transaction to abort

* Examples
  * MySQL (Serializable isolation level)
  
### Approach 3: Serializable Snapshot Isolation (SSI)
* Concept
  * Pessimistic concurrency control (2PL) vs optimistic concurrency control (SSI)
  * Let transactions proceed, and when it's time to commit, the DB checks for whether isolation has been violated
    * When contention is high, SSI is a bad idea
* Implementation
  * Detecting stale MVCC reads
  * Detecting writes that affect prior reads