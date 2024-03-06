---
layout: post
title: PostgreSQL - B+Tree Index
date: 2024-02-20
---

[B-Tree indexes](https://www.postgresql.org/docs/16/btree-intro.html) are a type of index in PostgreSQL.

> PostgreSQL includes an implementation of the standard btree (multi-way balanced tree) index data structure. **Any data type that can be sorted into a well-defined linear order can be indexed by a btree index**. [...]
>
> Because each btree operator class imposes a sort order on its data type, btree operator classes (or, really, operator families) have come to be used as PostgreSQL's general representation and understanding of sorting semantics.  
> Therefore, **they've acquired some features that go beyond what would be needed just to support btree indexes**, and parts of the system that are quite distant from the btree AM make use of them.

Fun fact: we don't know what the `B` in B-Tree stand for.  
It's not balanced or Bayer or anything else, even if it's a **B**alanced tree and it was invented by somebody called **B**ayer 🤷‍♀️.

### Internal Structure

👉 Indexes are stored in their own heap files.  
Their internal structure looks like this:

![PostgreSQL B-Tree]({{ "/assets/blog/postgres_btree.png" | absolute_url }}){:height="80%" width="80%"}

This is taken from this great [video](https://www.youtube.com/watch?v=78MgONXf5Es&list=PL1XF9qjV8kH0ghGRGo3_f-FWqWvAbv1dh) (from Professor Torsten Grust again 🙂)

Basically values are stored in a tree where the leaf contains a pointer to where the row for that value is actually stored.

### Inspecting

This creates a table with an index

```sql
CREATE TABLE indexed (a int primary key, b text, c numeric(3,2));
INSERT INTO indexed(a,b,c)
SELECT i, md5(i::text), sin(i) from generate_series(1,1000000) as i;
```

THe index shows in the `pg_class` table similarly to table:

```sql
SELECT relname, relfilenode
FROM pg_class
WHERE relname like 'indexed%'
```

```
   relname    | relfilenode
--------------+-------------
 indexed      |       41436
 indexed_pkey |       41441
```

(And there's a heapfile at `$PG_DATA/base/<db_id>/41441`.)

There are useful utilities to inspect indexes and their leaf nodes.  
For instance this shows a few leaves and their content.

```sql
CREATE EXTENSION IF NOT EXISTS pageinspect;

SELECT node.*
FROM generate_series(1,2744) as p, lateral bt_page_stats('indexed_pkey', p) as node
WHERE node.type = 'l'
ORDER BY node.blkno
LIMIT 3;
```

Returns

```
-[ RECORD 1 ]-+-----
blkno         | 1
type          | l
live_items    | 367
dead_items    | 0
avg_item_size | 16
page_size     | 8192
free_size     | 808
btpo_prev     | 0
btpo_next     | 2
btpo_level    | 0
btpo_flags    | 1
-[ RECORD 2 ]-+-----
blkno         | 2
type          | l
live_items    | 367
dead_items    | 0
avg_item_size | 16
page_size     | 8192
free_size     | 808
btpo_prev     | 1
btpo_next     | 4
btpo_level    | 0
btpo_flags    | 1
-[ RECORD 3 ]-+-----
blkno         | 4
type          | l
live_items    | 367
dead_items    | 0
avg_item_size | 16
page_size     | 8192
free_size     | 808
btpo_prev     | 2
btpo_next     | 5
btpo_level    | 0
btpo_flags    | 1
```

### B Tree vs B+ Tree

The Postgres doc references B-Tree (or B Tree) but it seems the structure of its index is closer to a B+ Tree.

I am not really sure what the official definition of either is. There doesn't seem to be a general agreement on it.  
BUT the main differences are:

- B+ Tree store pointers only in leaf nodes (and not internal nodes)
- B+ Tree contains links between leaf nodes
