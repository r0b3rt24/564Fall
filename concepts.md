## DBMS Uses
### ER Diagram
entity, relationship, degree + cardinality

### three elements in sharing structured data
meta data + logical & physical data independence + tx

## SQL
## 5 constraints
Not NULL, CHECK, UNIQUE, PK, FK
PK: functional dependency

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
- \; deal with any size of input; sorted outer input may help but not required; idx on inner input 
*merge join*
- require at least one equality predicate; \; sorted; \
- not friendly for dup join key values
*hash join*
- require at least one equality predicate; build input fit in mem; \; \ 



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
