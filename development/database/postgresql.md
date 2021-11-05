# PostgreSQL

## Disable parallel query execution

```
SET max_parallel_workers_per_gather = 0;
```

## Check invalid indexes

```
SELECT * FROM pg_class, pg_index 
WHERE pg_index.indisvalid = false 
  AND pg_index.indexrelid = pg_class.oid;
```

The query can be useful to check if `CONCURRENT` index has been already created.

## Lock

```
SELECT ... FOR UPDATE
  SKIP LOCKED -- non-blocking
```
