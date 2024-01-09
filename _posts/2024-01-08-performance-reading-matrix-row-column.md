---
layout: post
title: Performance of reading a matrix by columns or rows in JavaScript
date: 2024-01-08
---

I wrote a little script to compare how long it'll take to read a matrix (array of arrays) depending on whether I access it by rows or columns.

Reading by rows is faster than reading by columns.

This is because the JavaScript engine stores arrays as contiguous space in memory (as much as it can and I guess?). I had a quick look at V8 and can't easily find that information.  
I guess there will also be variation depending of whether the array has a fixed size or not? If one keeps adding elements to an array, its size will vary and maybe it won't be stored contiguously?

[EDIT] Found this StackOverflow [post](https://stackoverflow.com/questions/37645340/does-v8-still-have-c-style-arrays-that-are-contiguous-blocks-of-memory-and-how) which seems to say the same thing.

Here are the results:

- 100,000,000 x 8: ~ 900ms vs 8,200ms
- 8 x 100,000,000: ~ 400ms vs 1,200ms
- 50000 x 50000: ~ 1,500ms vs 3,000ms

```
🏗  Building and filling matrix...
ℹ️  Number of rows: 100000000 and columns: 8
⏱  Reading by rows: 921 ms
⏱  Reading by rows: 981 ms
⏱  Reading by rows: 921 ms
⏱  Reading by rows: 942 ms
⏱  Reading by rows: 944 ms
⏱  Reading by rows for-of: 8238 ms
⏱  Reading by rows for-of: 8142 ms
⏱  Reading by rows for-of: 8062 ms
⏱  Reading by rows for-of: 8309 ms
⏱  Reading by rows for-of: 8448 ms
⏱  Reading by columns: 1199 ms
⏱  Reading by columns: 1148 ms
⏱  Reading by columns: 1145 ms
⏱  Reading by columns: 1177 ms
⏱  Reading by columns: 1184 ms

ℹ️  Number of rows: 8 and columns: 100000000
⏱  Reading by rows: 435 ms
⏱  Reading by rows: 396 ms
⏱  Reading by rows: 401 ms
⏱  Reading by rows: 393 ms
⏱  Reading by rows: 480 ms
⏱  Reading by rows for-of: 573 ms
⏱  Reading by rows for-of: 499 ms
⏱  Reading by rows for-of: 503 ms
⏱  Reading by rows for-of: 502 ms
⏱  Reading by rows for-of: 497 ms
⏱  Reading by columns: 1225 ms
⏱  Reading by columns: 1225 ms
⏱  Reading by columns: 1223 ms
⏱  Reading by columns: 1224 ms
⏱  Reading by columns: 1225 ms

ℹ️  Number of rows: 50000 and columns: 50000
⏱  Reading by rows: 1509 ms
⏱  Reading by rows: 1494 ms
⏱  Reading by rows: 1503 ms
⏱  Reading by rows: 1498 ms
⏱  Reading by rows: 1500 ms
⏱  Reading by rows for-of: 1500 ms
⏱  Reading by rows for-of: 1491 ms
⏱  Reading by rows for-of: 1494 ms
⏱  Reading by rows for-of: 1494 ms
⏱  Reading by rows for-of: 1492 ms
⏱  Reading by columns: 3067 ms
⏱  Reading by columns: 3056 ms
⏱  Reading by columns: 3038 ms
⏱  Reading by columns: 3284 ms
⏱  Reading by columns: 3200 ms
```

Here is the script:

```js
function readByRows(matrix: number[][], rows: number, columns: number) {
  const start = performance.now();

  for (let row = 0; row < rows; row++) {
    for (let col = 0; col < columns; col++) {
      const _ = matrix[row][col];
    }
  }

  const end = performance.now();
  console.log(`⏱  Reading by rows: ${Math.round(end - start)} ms`);
}

function readByColumns(matrix: number[][], rows: number, columns: number) {
  const start = performance.now();

  for (let col = 0; col < columns; col++) {
    for (let row = 0; row < rows; row++) {
      const _ = matrix[row][col];
    }
  }

  const end = performance.now();
  console.log(`⏱  Reading by columns: ${Math.round(end - start)} ms`);
}

function readUsingForOf(matrix: number[][], rows: number, columns: number) {
  const start = performance.now();

  for (const row of matrix) {
    for (let col = 0; col < columns; col++) {
      const _ = row[col];
    }
  }

  const end = performance.now();
  console.log(`⏱  Reading by rows for-of: ${Math.round(end - start)} ms`);
}

function main() {
  console.log("🏗  Building and filling matrix...");
  const ROWS = 100000000;
  const COLUMNS = 8;
  const matrix: number[][] = new Array(ROWS).fill(
    new Int8Array(COLUMNS).fill(1)
  );

  console.log(`ℹ️  Number of rows: ${ROWS} and columns: ${COLUMNS}`);

  readByRows(matrix, ROWS, COLUMNS);
  readByRows(matrix, ROWS, COLUMNS);
  readByRows(matrix, ROWS, COLUMNS);
  readByRows(matrix, ROWS, COLUMNS);
  readByRows(matrix, ROWS, COLUMNS);

  readUsingForOf(matrix, ROWS, COLUMNS);
  readUsingForOf(matrix, ROWS, COLUMNS);
  readUsingForOf(matrix, ROWS, COLUMNS);
  readUsingForOf(matrix, ROWS, COLUMNS);
  readUsingForOf(matrix, ROWS, COLUMNS);

  readByColumns(matrix, ROWS, COLUMNS);
  readByColumns(matrix, ROWS, COLUMNS);
  readByColumns(matrix, ROWS, COLUMNS);
  readByColumns(matrix, ROWS, COLUMNS);
  readByColumns(matrix, ROWS, COLUMNS);
}

main();
```

An interesting read about [Elements Kind](https://v8.dev/blog/elements-kinds) in v8.
