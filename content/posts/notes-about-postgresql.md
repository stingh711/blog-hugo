+++
title = "Notes about postgresql"
date = 2018-05-08
tags = ["database", "postgresql"]
type = "post"
draft = false
+++

## How to generate and insert test data? {#how-to-generate-and-insert-test-data}

For example, my table is like: `create table (time timestamp, value double precision, sensor integer)`. If I want to insert some test data, I can use function `generate_series`.
Following sql will insert 5000 rows:

```sql
    insert into test (time, sensor, value) select now(), i, random() from generate_series(1, 5000) s(i)
```


## How to view disk usage? {#how-to-view-disk-usage}


### View table size {#view-table-size}

```sql
     select pg_size_pretty(pg_relation_size('pressure_01'))
```


### View database size {#view-database-size}

```sql
     select pg_size_pretty(pg_database_size('pressure_01'))
```