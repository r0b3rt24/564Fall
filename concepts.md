## DBMS Uses
### ER Diagram
entity, relationship, degree + cardinality

### three elements in sharing structured data
meta data + logical & physical data independence + tx

## SQL
## 5 integrity constraints
Not NULL, CHECK, UNIQUE, PK, FK
 - PK: functional dependency

## 5 data type

## 3 Join Type
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
### b tree / hash

###




## Transaction Management
### 3 Failure
### Tx F Recovery
### Sys F Recovery

## Concurrency Control
### OCC & PCC
### 3 locks
KVL, IM, KRL

## Columnar Storage
###

## Scaling & parallel query execution
### 
