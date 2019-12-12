## DBMS Uses
### ER Diagram
entity, relationship, degree + cardinality

### three elements in sharing structured data
meta data + logical & physical data independence + tx

### Storeage Engine & Relational Engine
#### Storeage Engine
indexing, backup and restore, concurrency control, logging, and recovery.
#### Relational Engine
SQL parser, catalogs, query optimization, query execution.

## SQL
### 5 integrity constraints
Not NULL, CHECK, UNIQUE, PK, FK
 - PK: functional dependency

### 5 data type

### 3 Join Type
inner join, semi join, outer join

## Query execution algorithms
### aggregate (group by)/dup removal (distinct) Basic Methods
*in stream*
- sorted input
*in sort*
- sort intermediate result (min/max, count, sum)
*hash*
- unsorted input and in-memory hash table

### relation algebra
label aggregation (group by what and compute what)

## Query Optimization
### query execution plan
requirements similar to relation algebra

### 4 Join Alg & pros and cons
- join predicate
>> equality?
- input requirement
>> in mem (input size)? sorted? idx?
- output 
>> pipeline?

*naive nlp*
- easy to implement~

*idx nlp*
- \\; deal with any size of input; sorted outer input may help but not required; idx on inner input 

*merge join*
- require at least one equality predicate; \\; sorted; \\
- not friendly for dup join key values

*hash join*
- require at least one equality predicate; build input fit in mem; \\; \\

*hash join VS hash aggregation*
1. hash join stores each build row, so the total memory requirement is proportional to the number and size of the build rows; 
hash aggregate stores one row for each group, so the total memory requirement is proportional to the number and size of the output groups or rows; 
2. hash aggregation collapse duplicate rows into one group

## Tree-based Storage Structures
- num of comparison: log<sub>2</sub>N
- height: log<sub>fan-out</sub>N
### b tree / hash Index
#### Hash-Based Index
- Put records into buckets according to hash(key)
- can also add Axulleries key in each bucket (sort in each bucket)

## Transaction Management
### ACID
- Atomicity: ,ultiple actions indivisible, all or nothing
- Consistency: for every trax, it sees the same database. integrity constraints, keep in valid state. 
- Isolation: concurrency control, illusion of single user access
- Durability: Can survivce craches.
### WAL
record any changes in the persistent database. log first and then modify the database. 
### 3 Failure
tx, sys, media
### Tx F Recovery
scan log + log the rollback + rollback 
### Sys F Recovery
- checkpoint: scan active tx, most recent log rec, dirty pg in buf pool
- log analysis + redo + undo
- incomplete tx + mem failure + good rec log + good db (slightly out of date)

## Concurrency Control
### OCC & PCC
#### Optimistic Concurrency Control
Assume most transactions won't have conflicts
1. Read: the transaction executes, readling values from the database and writing to a private workspace.
2. Validation: if the transaction decides that it wants to commit, the DBMS checks whether the transaction could possibly have conflicted with any other concurrently executing transaction. If there is a possible conflict, abort.
3. Write: if validatipon determines that there are no conflicts, the changes to the data objects will be copied from the private workspace to the database.
#### Timestamps(Timestamp based concurrency control)
Each transaction can be assigned a timestamp at startup, and we can ensure , at execution time, that if action ai of transaction Ti conflicts with action aj of transaction Tj, ai occurs before aj if TS(Ti) < TS(Tj)

If an action violates this ordering, the transaction is aborted and restarted.

#### Snapshot isolation
1. The basic idea is take a snapshot(a copy of the entire database at a certain moment) once a while. 
2. Delete logs that don't have new information on tip of the snapshot.
3. By doing so, we could have a much more compact log which save time for recovery. 

### 3 locks
KVL, IM, KRL

## Columnar Storage
### application scenario
many rows(1‰) & few cols

## Scaling & parallel query execution
### partitioning & pipelining
Splitting the data across the machine; Splitting query operations
### cost metric
response time, total resource consumption
### information transfer
- 2 phase commit
- redundancy: mirroring/log shipping

