# Introduction to Database Management FALL 2019

[TOC]

## Database design and DR diagrams

### Steps

1. Requirement analysis
   1. What's data is going to store in the database
   2. What applications are going to run top on thiss database
   3. What functions will be called most frequently
   4. This step is usually very informal. 
2. Comceptual Database Design
   1. Produce an ER diagram: a high level sementaic for data models used in the database
3. Logical Databse Design
   1. The step produces a logical schema based on a specific DBMS that people chose to use.

### Beyond ER 

1. Schema Refinement
   1. Detect potential problems, and fix them
   2. Normalizing, etc.
2. Phsical Database Design
   1. Support further refine the database
   2. To meet the performance criteria: index, etc.
3. Application and Security Design
   1. Analysis the requirement of the application using UML
   2. Which part of DB can be accessed by which part of the Application (Security)



### Relationship and Relationship Sets

1. A relationship is an associate among two or more entities.
2. Relationships Set is a set of similar relationships.
3. Descriptive Attributes: record information about relationships reather than the entity participates the relationships.
4. Ternary relationship: 

### Key constraints

1. One-to-Many
2. Many-to-One
3. Two distinct tuples in a legal instance cannot have identical values in all the fields of a key
4. No subset of the set of fields in a key is a unique identifier for a tuple
5. Super key is a set of fields that contains a key
6. **5 keys:  check, not null, primary key, foriegn key, unique.** 

### Storage Engine & Relational Engine

#### Storeage Engine:

indexing, backup and restore, concurrency control, logging, and recovery.

#### Relational Engine:

SQL parser, catalogs, query optimization, query execution.



## SQL

***What is required within a database to efficiently support a non-null constraint?***

- A simple predicate is applied during all updates.

### Relational Algrebra

#### Select Operation($\sigma$)

It selects tuples that satisfy the given predicate from a relation.

#### Project Operation($∏$)

It projects column(s) that satisfy a given predicate.

#### Cross Product (x)

#### Rename($\rho$)

$\rho(relation1, relation2)$



## DBMS Buffer Managment

**buffer manager:** The code manage to transfer data from disk to buffer pool and reverse

**replacement policy:** LRU FIFO etc.

**buffer pool:** The part of memeory that stores the frames from disk that might be used by the DBMS

**frames:** a unit on the memory



### When page request happens:

1. ping increase by one, the object O waiting for the trax to commit
2. ...
3. after commit, the ping_count will decrease by one



### Buffer replacemeny policy:

##### LRU

Least Recent Used

##### LFU

Least frequently Used

##### ARC

Adaptive Replacement Cache ( track both frequent and recently used pages)

##### FIFO

First in first out



### RAID (not on the exam, also you learnd this in OS)

##### RAID-4

##### RAID-5

##### RAID-6



### DBMS VS OS

DBMS has a better sense than OS about what data will be preocessed later. It will fetch the data from the disk to memory early, this is called **prefetching**

#### Search in arryys

1. sequential: O(n)
2. binary: O(logn)
3. interpolation: O(loglogn)
4. extraplation



## Tree-based Storeage Structures

B+ tree

## Sorting

#### Use case of sorting:

- index creation
- searching
- database operations
- oftenly use quick sort



#### External Merge-Sort

Phase one: read-sort-write cycle load M bytes uin memeory, sort write to disk

- One step merge: merge all the runs and returns the merged output (only if the memory is enough)
- if memory is not enough, merge according to the fan-in.

![Screen Shot 2019-12-11 at 5.20.16 PM](/Users/hcao/Documents/ScreenShots/Screen Shot 2019-12-11 at 5.20.16 PM.png)

## Query execution algorithms

##### Table Scan

- column scans
- Index scans
- index intersection

##### Duplicate removal:

- in-stream
- In-sort
- Hash-distinct

##### Grouping

- In-stream

- in-sort
- hash
- aggregation

##### nested loops join:

Naive, block, index temporary index, poor man's merge join

##### nested queries:

nested iteration, caching, side-way information passing

##### merge join:

Duplicate kety values, zigzag, sort on normalized keys or hash values

##### Hash join: 

internal, external, recursive, graceful degreadation





## Database Metadata

### Catalog Table

table that stores information about other tables and indexes in the database.

#### Information in the Catalog

##### for each table:

1. table name, file name, file structure(head file)
2. attribute name and tupe of each of its attributes
3. index name of each index on the table
4. integrity constraints

##### for each index:

1. index name and structure of the index
2. search key arrtibutes

##### for each view:

1. It's view name and definition



Cardinality: The number of tuples for each table R

Size: The number of pages for each table R

Index Cardinality: The number of distinct keyt values for each index 1

Index size: the number of pages for each index I.

Index Height: The number of nonleaf leavls for each tree index.

Index Range: The minimum present key value and maximum present key value for each index I. 



## Query Optimization

### Cardinality Estimation

- Row count: How many rows are there in the table?
- histogram: A histogram is a data strcuture maintained by a DBMS to approximate a data distribution.
- Multiply for conjunctions
- magic numbers: used for setting cutoffs for using different operations.

### example rewrites & optimizations

1. pushing down selection and projection

2. join order – dynamic programming vs incremental transformations

3. equivalence classes, inferred predicates
4. interesting orderings
5. pushing down grouping operations
6. functional dependencies in “order by” and “group by” – group-join
7. nested queries ⇒ semi-joins, (local) rewrite – caches
8. star join plans: semi-join reduction, Cartesian products“count (distinct) > 1”
9. pivot operations?
10. SQL Server query plans





## Transaction Management

### AICD:

- Atomicity: either commit or abort

- Isolation: Protect each trax from other concurrent traxs

- Consistency: for every trax, it sees the same database.

- Durability: Can survivce craches.

### Write-ahead logging(WAL)

The changes are first recorded in the log, which must be written to stable storage, before the changes are written to the database.

- stable storage: is a storage place that is gaurenteed to survive a crush or system failiure. 

### Transactions roll back

When a system crashed, you can roll back all the uncommited transactions according to the log.

### 3 Failure:
sys
media
tx

#### Recovery from a system crush

- LSN: Log Sequence Number is just an ID given to each log record

##### Analysis Phase

1. determine the point in the log at which to start the Redo pass
2. It determines pages in teh buffer pool that were ditry at the time of the crash
3. it identifies transactions that wrere actibe at the time of the crash and much be undone.

This phase is done by examingning the most recent begin_checkpoint log record and initializing the dirty page table and transaction table to the copies of those sturctures in the next end_checkpoint record.

- If an end log record for a transaction T is encountered, T is removed from the transaction table because it is no longer active
- if a log record other than an end record for a transction T is encountered, an entry for T is added to the transaction table if it is not already.
- If a redoable log record affecting page P is encountered, and P is not in the dirty page table, an entry is inserted into this table with page id P and recLSN equal to the LSN of this redoable log records. 

##### Redo Phase

Reapplies the updates of all transactions committed or otherwise. 



##### Undo Phase

The goal of this phase is to undo the actions of all transcations active at the time of the crash, that is to effectively abort them.

Start from the begining of the table generated by the Analysis phase

### Traditional restart
### Instant restart
Not on exam 


## Concurrency Control

**action**: an action to an object involving read or write

**schedule**: a list of actions

**complete schedule**:

**serial schedule**: deal with objects serially

**serializeble schedule**: concurent traxs can be scheduled as deal with one object then another.



#### Conflicts:

1. WR (dirty read) : T2 read after T1 wrote but hasn't committed yet.
2. RW (Unrepreatable reads):  T2 write after T1 read.
3. WW (Overwritting Uncommitted Data) : T2, T1 are trying to write the same object.

In a concurent schedule, there will be conflicts when write involves. 



#### Strict 2PL

If trax T wants to read/modify an Object O, it first requests a shared lock lock on the object.

all locks held by a tranx are released when trax is finished.

### OCC & PCC
application scenarios


## Indexes and other DBMSs

### Tree-Based Index

B+ Tree (You know this well enough)



### Hash-Based Index

- Put records into buckets according to hash(key)
- can also add Axulleries key in each bucket (sort in each bucket)



## Column Storage and Compression

#### Row storage:

- Put as much as rows in each disk block.

-  Better for applications that need all the columns of a record, but not all records in a table.

#### Column Storage:

- put entire column in a table in each disk block. 
- Better for applications which need entire column in a table but not all columns for a records. e.g.: deep learning ect.

**No Compression on the exam**



## Scaling & parallel query execution



## Cloud services/Distributed Database

#### Two Phase Commit

To solve weather or not to commit a distributed transaction.

##### assumptions:

1. there are local logs but no global log
2. there is a coordinator decide to commit or not

##### Phase 1:

1. The coordinator places a log record <Prepare T> on the log at its side.
2. The coordinator sends to each component's site the message prepare T.
3. Each site receiving the msg prepare T decides whether to commit or abort it's component of T. The side can delay if the component has not yet completed its activiry, **but must eventually send a response.**
4. It a site wants to commit its opponent, it must enter a state called precommitted. Once in the precommited state, the site cannot abort its component of T without a directive to do so from the coordinator. 
   1. Place the record Ready T on the local log and flush the log to disk
   2. Send to coordinator the message ready T
5. if the site decides to abort its compoenent of T, then it logs the record "dont't commit T" and sends the message don't commiut T to the coordinator. It is safe to abort the component at this time since T will surely abort if even one compoenent wants to abort.

##### Phase 2：

1. if the coordinatior has received ready T from all compoenents of T, then it decides to commit T. The coordinator logs <Commit T> at its site and then sends message commit T to all sites involved in T.
2. However, if the coordinator has received don't commit T from one or more sides, it logs <Abort R> as its site and then sends abort T msg to all sites involved in T.
3. if a sute recieves a commit T, it commit the compoennt of T at that side and looging <Commit T> as it does.
4. If a site receives the message abort T, it aborts T and writes the log record<Abort T>.



#### Log shipping

Refer to the previous section. 



#### partioning vs piplining

Partioning:  splitting the data across the machine

Piplining: Splitting query operations



## In memory database
basic concepts
