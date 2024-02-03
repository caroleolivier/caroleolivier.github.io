---
layout: post
title: PostgreSQL - MVCC
date: 2024-02-03
---

Here are a few things I learnt after reading and watching videos about MVCC (Multi Version Concurrency Control).

The best definition I found comes from PosgreSQL [website](https://www.postgresql.org/docs/current/mvcc-intro.html):

> each SQL statement sees a snapshot of data (a database version) as it was some time ago, regardless of the current state of the underlying data.  
> This prevents statements from viewing inconsistent data produced by concurrent transactions performing updates on the same data rows, providing transaction isolation for each database session.

The implementation of everything around MVCC in PostgreSQL is quite complex and I am just starting to grasp it.  
But here are concepts I found pretty interesting and cool:

## Versioning

Every transaction that runs in SQL has a unique incremental version.

```sql
SELECT txid_current();
```

This displays the transaction id of the current transaction.  
Whenever a statement is run, it's incremented.

There are more interesting columns that indicate when a row was inserted:

`xmin` and `xmax` are called [system columns](https://www.postgresql.org/docs/16/ddl-system-columns.html).  
They're not visible by default, but we can explicitly query them.

```sql
SELECT *, xmin, xmax FROM names;
```

(`names` is just a random table I created with 2 columns `first_name` and `last_name`)

```shell
first_name | last_name | xmin | xmax
------------+-----------+------+------
 will       | iam       |  773 |    0
(1 row)
```

These 2 columns are super useful and determine what a transaction can and cannot see.

🚨 Some rows sometimes have `xmax` but are actually visible, see [this](https://www.cybertec-postgresql.com/en/whats-in-an-xmax/).  
So `xmax` alone doesn't indicate whether a row is visible or not.

### A little experiment

It's quite fun to start 2 transactions in parallel and observe what happens when calling and modifying tables from the different transactions.

With the default configuration of PostgreSQL `Read Committed`, transaction sees the effect of committed transactions (no matter if the transaction started before or after, if it's committed, then the current transaction sees it).

**Before starting anything**

```sql
SELECT *, xmin, xmax FROM names;
```

```shell
first_name | last_name | xmin | xmax
------------+-----------+------+------
 will       | iam       |  773 |    0
(1 row)
```

**TRANSACTION 1**

```sql
BEGIN;
INSERT INTO names VALUES('jon', 'doe');
SELECT *, xmin, xmax FROM names;
```

```shell
 first_name | last_name | xmin | xmax
------------+-----------+------+------
 will       | iam       |  773 |    0
 jon        | doe       |  775 |    0
(2 rows)
```

If I start a second transaction session, this is what I see:

**TRANSACTION 2**

```shell
first_name | last_name | xmin | xmax
------------+-----------+------+------
 will       | iam       |  773 |    0
(1 row)
```

=> Can't see `jon doe` yet.

if I commit the 1st transaction and run the `SELECT` again in the 2nd transaction I see:

**TRANSACTION 2**

```shell
first_name | last_name | xmin | xmax
------------+-----------+------+------
 will       | iam       |  773 |    0
 jon        | doe       |  775 |    0
(2 rows)
```

=> I can see the committed tuple (row)!

And in the 2nd transaction, If I run an update on `jon`, what would happen?

=> It'll be updated.

💡 Another fun thing to try it to update a record that was inserted in another older not committed transaction.  
In this case, the **2nd transaction just hangs** and waits until transaction 1 has either committed or rolled back.

## Transaction Isolation

In the little experiment above, I was surprised/not surprised when I committed `jon doe` and it suddenly became visible in the 2nd ongoing transaction.  
Should it have been?  
It meant that the same `SELECT` returned different results within the same transaction, was this a good thing? a bad thing? 🤷‍♀️

And that's when I remember reading about transaction isolation and how there were different levels:

They're all defined and explained [here](https://www.postgresql.org/docs/current/transaction-iso.html).

By default PostgreSQL servers are `Read Committed` for which `Nonrepeatable Read` are possible which explained why indeed the same `SELECT` statement could return different results within the same transaction and even if it hadn't changed anything.
