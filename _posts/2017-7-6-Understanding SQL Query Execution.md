# Understanding SQL Execution 
http://rusanu.com/2013/08/01/understanding-how-sql-server-executes-a-query/

- SQL parsing: --> AST
- Query optimization: cost based on statistics
- Query execution: Tasks(requests) vs Workers(thread pool threads)
- Execution Plan Caching and Reuse. EF Linq queries are by default cached. Impact on server memory foot print, Impact on recompile time.
- SQL execution: Execution tree, leaf nodes reads data. open(), next(), close(). Intermediate node filters/joins data
- complexity of execution: Parallel Query Evaluation. Top(), sort, nested look join, hash join.
- result: send back as a steam as data is being produced.
- output parameter: not streamed. send back after query is done.
- memory grant: resource semaphore. sys.dm_exec.query_memory_grants
- event class: execution warning, exchange spilling, sort warning, hash warning, ...
- memory are expected in analytic but not transaction databases

## Data Organization
- Heaps: heap of rows without indexes
- Clustered Indexes: B+trees contains the rows
- Noneclustered Indexes: B+trees references the rows
- clustered,  none clustered Columnstore

## Data Access
- Scan: index scan, table scan, remote scan
- Seek: in b-trees, seek for complex keys
- Bookmark Lookup
- Log Row Scan

## Reading Data
- Buffer Pool, page
- Read Ahead
- Latches

## Latches
- read a page --> Shared latches
- modify a page --> exclusive 
- [Diagnosing and Resolving Latch Contention](http://www.microsoft.com/en-us/download/details.aspx?id=26665)

## Locks <--> Transactions
- [Lock Compatibility](http://technet.microsoft.com/en-us/library/ms186396.aspx)
- schema stability lock: SCH-S
- data intent lock : IS is acquired by queries
- schema modification lock: SCH-M
- S(shared), X(exclusive), U(update) lock
- U lock: read and later on write(upgrade to X). prevents other U locks
- intent locks: locks placed on parent object announcing intention on the child object(e.g. SIX on a table-> locking the table in shared mode with an intent to lock a row in exclusive mode)
- Key-Range locks to prevent Phantom Reads
- write operator(row insert, delete, update) will: fixed the page, exclusive latch the page, write the modification log, modify the in memory page, modify the page header(LSN), release the page latch, write the commit log and write the log to disk. pages are periodically written to disk
- minimally logged operation: e.g. bulk insert, logs are generated for the pages what is written, not for the rows that are inserted.

## Performance tunning
- [Waits and Queues](http://technet.microsoft.com/en-us/library/cc966413.aspx)