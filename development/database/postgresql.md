# PostgreSQL

## Disable parallel query execution

```text
SET max_parallel_workers_per_gather = 0;
```

## Check invalid indexes

```text
SELECT * FROM pg_class, pg_index 
WHERE pg_index.indisvalid = false 
  AND pg_index.indexrelid = pg_class.oid;
```

The query can be useful to check if `CONCURRENT` index has been already created.

