---
layout: post
title: PostgreSQL - heap files and FSM
date: 2023-12-03
---

Heapfiles contain the data of a single table.  
They're saved as rows and are unordered.

They're divided into page of fixed size of 8kB.

💡 This fixed size helps with managing disk space and memory as it corresponds to the block size the OS operates with when it does I/O operations.

💡 Adjacent pages are faster to read.

What happens when a file is larger and can't contain all records??

## Free Space Management

Basically for each heapfile, there's an extra file called heapfile*\_fsm*.  
It's used to indicate the free space in a heapfile.

The way it works is by building a tree to indicate the maximum free space in a file with its location.

💡 There are utilities to explore the fsm files.  
This command helps with this: `create extension if not exists pg_freespacemap;`

Now when running this command, one can view how many pages (blocks) are needed to store the data of 1 heapfile.

```
SELECT *
FROM pg_freespace('table_name');
```

💡 It's possible to force a compression of the heapfiles, not sure whether the db do it automatically or whether it's good practice to do it regularly.

Another great video [here](https://www.youtube.com/watch?v=ep87Pskkp14&list=PL1XF9qjV8kH0ghGRGo3_f-FWqWvAbv1dh&index=5) with this [one](https://www.youtube.com/watch?v=SFN4maoWkYA&list=PL1XF9qjV8kH0ghGRGo3_f-FWqWvAbv1dh&index=11) with Tetris row haha.

## Vacuuming

When exploring the FSM, it's good to run the VACUUM [command](https://www.postgresql.org/docs/current/sql-vacuum.html) to force the heapfiles and fsm to be up-to-date:

```
$ VACUUM table_name;
```
