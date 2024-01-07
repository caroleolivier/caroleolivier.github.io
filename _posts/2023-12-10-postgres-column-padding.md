---
layout: post
title: PostgreSQL - column padding
date: 2023-12-10
---

Today, I am looking at padding and what it means for storage in Postgres.

TL/DR: it's bytes lost in storage but useful for fast disk access so linked to how processor reads memory.

These 2 tables do not occupy the same space on disk!

```
CREATE table packed (a int8, b int8, c int2, d int2);
CREATE table padded (c int2, a int8, d int2, b int8);
```

- int2: occupies 2 bytes and must be stored at an address divisible by 2
- int8: occupies 8 bytes and must be stored at an address divisible by 8.

So stored bytes looks like this for the `packed` table (increase your screen size to display each line as 1)

```
0   1   2   3   4   5   6   7   8   9   10  11  12  13  14  15  16  17  20  21
a   a   a   a   a   a   a   a   b   b   b   b   b   b   b   b   c   c   d   d
i8  i8  i8  i8  i8  i8  i8  i8  i8  i8  i8  i8  i8  i8  i8  i8  i2  i2  i2  i2
```

And like this for `padded` table

```
0   1   2   3   4   5   6   7   8   9   10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26  27  28  29  30  31
c   c                           a   a   a   a   a   a   a   a   d   d                           b   b   b   b   b   b   b   b
i2  i2  pad pad pad pad pad pad i8  i8  i8  i8  i8  i8  i8  i8  i2  i2  pad pad pad pad pad pad i8  i8  i8  i8  i8  i8  i8  i8
```

I inserted the following integers into the table and looked at the heap files:

```
insert into packed(a, b, c, d) values(170, 187, 204, 221);
```

where:

- 170 => `aa` in hex
- 187 => `bb` in hex
- 204 => `cc` in hex
- 221 => `dd` in hex

This is what I saw:

`packed` table:

```
00000000  00 00 00 00 d0 52 a8 01   00 00 00 00 1c 00 d0 1f  |....�R�.......�.|
00000010  00 20 04 20 00 00 00 00   d0 9f 58 00 00 00 00 00  |. . ....�.X.....|
00000020  00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00  |................|
*
00001fd0  03 03 00 00 00 00 00 00   00 00 00 00 00 00 00 00  |................|
00001fe0  01 00 04 00 00 08 18 00👉 aa 00 00 00 00 00 00 00  |........�.......|
00001ff0  bb 00 00 00 00 00 00 00   cc 00 dd 00 00 00 00 00  |�.......�.�.....|
00002000
```

`padded` table:

```
00000000  00 00 00 00 88 53 a8 01   00 00 00 00 1c 00 c8 1f  |.....S�.......�.|
00000010  00 20 04 20 00 00 00 00   c8 9f 70 00 00 00 00 00  |. . ....�.p.....|
00000020  00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00  |................|
*
00001fc0    00 00 00 00 00 00 00 00   04 03 00 00 00 00 00 00  |................|
00001fd0    00 00 00 00 00 00 00 00   01 00 04 00 00 08 18 00  |................|
00001fe0👉  cc 00 00 00 00 00 00 00   aa 00 00 00 00 00 00 00  |�.......�.......|
00001ff0    dd 00 00 00 00 00 00 00   bb 00 00 00 00 00 00 00  |�.......�.......|
00002000
```

If I am not mistaken, the row starts at 👉 and is stored over 3 x 8 bytes for the `packed` table while it's stored over 4 x 8 bytes for the `padded` table.  
And so more bytes are used!

Heavily taken from this [series](https://www.youtube.com/watch?v=SFN4maoWkYA) again 🙏
