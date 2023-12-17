---
layout: post
title: PostgreSQL - database and table storage
date: 2023-11-26
---

Now that we've got a Postgres server running, let's look at where/how the data is actually stored.

## Database location

PostgreSQL files are located under `echo $PGDATA`.

```
--- PGDATA
    |--- global
    |--- lots of other stuff
    |--- base
        |--- 1
        |--- 4
        |--- 16388
```

I haven't explored everything yet, but each database has its own directory under `base`.

We can find this directory by running the following command:

```
SELECT oid, datname
FROM pg_database
WHERE datname = 'database_name';
```

To change the connected database with psql:

```
\c carole_database;
```

## Tables

Tables data is stored in heapfiles.

The query below tells what file the data of a table is in:

```
SELECT relfilenode, relname
FROM pg_class
WHERE relname = 'table_name';
```

The heap file can be read using this command:

```
$ hexdump -C 16395
```

An example :)

```
00000000  00 00 00 00 e8 19 9a 01  00 00 00 00 28 00 78 1f  |............(.x.|
00000010  00 20 04 20 00 00 00 00  e0 9f 3a 00 b8 9f 42 00  |. . ......:...B.|
00000020  98 9f 3a 00 78 9f 3a 00  00 00 00 00 00 00 00 00  |..:.x.:.........|
00000030  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00001f70  00 00 00 00 00 00 00 00  ed 02 00 00 00 00 00 00  |................|
00001f80  00 00 00 00 00 00 00 00  04 00 01 00 02 09 18 00  |................|
00001f90  0b 6c 75 6b 65 00 00 00  ed 02 00 00 00 00 00 00  |.luke...........|
00001fa0  00 00 00 00 00 00 00 00  03 00 01 00 02 09 18 00  |................|
00001fb0  0b 6c 65 69 61 00 00 00  ed 02 00 00 00 00 00 00  |.leia...........|
00001fc0  00 00 00 00 00 00 00 00  02 00 01 00 02 09 18 00  |................|
00001fd0  13 68 61 6e 20 73 6f 6c  6f 00 00 00 00 00 00 00  |.han solo.......|
00001fe0  ed 02 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00001ff0  01 00 01 00 02 09 18 00  0b 79 6f 64 61 00 00 00  |.........yoda...|
00002000
```

⚠️ When creating and populating a table, Postgres won't write data to the heapfile straight away.  
It does this asynchronously at some point.  
So it's possible that after populating a table, the changes are not visible in the heapfile yet and doesn't show anything for instance.  
It's possible to trigger a checkpoint by calling:

```
CHECKPOINT;
```

(This is related to [WAL](https://www.postgresql.org/docs/current/wal-intro.html) Write Ahead Log, another thing to explore in details!)

## Row Identifier (RID)

RIDs are unique within a table.  
They indicate the location of the row in the heap file.

The row is indicated with the hidden column `ctid`

```
SELECT ctid, a as col
FROM your_table
```

Shows

```
   ctid   | my_col
----------+--------
 (0,1)    | 1
 (0,2)    | 2
 (0,3)    | 3
```
