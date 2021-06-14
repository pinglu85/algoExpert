# Minimum Waiting Time

### Understanding the problem

I am given an array of positive integers, where each integer represents the duration of a query that needs to be executed. Only one query can be executed at a time, but the queries can be executed in any order. I am asked to write a function that returns the minimum amount of total waiting time for all of the queries, and I am allowed to mutate the input array.

Suppose the input array is going to be `[1, 3, 2]`. The first query can be executed immediately, therefore the waiting time of the first query is 0. The second query have to wait until the first one finishes before it can execute, so the waiting time of the second query is going to be 1 second, the duration of the first query. The third query has to wait for both the first query and the second query to finish executing before it can start. Since the second query takes 3 seconds to execute, the waiting time of the third query is going to be 1 + 3 seconds. So if the queries are executed in that order, then the total awaiting time for all of the queries is going to be `(0) + (1) + (1 + 3) = 5`.

#

### Approach 1

Since I am asked to compute the minimum amount of total waiting time, I am going to sort the input array in ascending order at the very beginning. Suppose the input array is going to be `[6, 1]`, the total waiting time is going to be `(0) + (6) = 6`. However, if they are executed in the order of `[1, 6]`, then the waiting time is going to be `(0) + (1) = 1`. Therefore, to get the minimum amount of the total waiting time, I need to figure out the optimal execution order. It can be also noticed, if the largest query is executed first, all of the queries after it will have to wait at least the duration of the largest query; if the shortest query is executed first, then all queries after it are for sure waiting the minimum amount of time, and since the largest query is going to be executed at the very end, that means no queries need to wait for the largest query to finish executing.

Then I would calculate the waiting time of each individual query and sum all of the waiting time up.:

- Initialize a variable to keep track of the time that each query needs to wait; initially, set it to 0.
- Initialize another variable to store the total waiting time so far.
- Traverse the sorted array element by element. At each query,
  - add the time that current query have to wait to the total waiting time;
  - update the waiting time that next query needs to wait by adding the duration of the current query to the waiting time.
- Return the amount of total waiting time.

### Time & Space Complexity

O(nlog(n)) time | O(1) space, where n is the number of queries.

### Solution 1

```js
function minimumWaitingTime(queries) {
  queries.sort((a, b) => a - b);

  let waitingTime = 0;
  let totalWaitingTime = 0;
  for (const duration of queries) {
    totalWaitingTime += waitingTime;
    waitingTime += duration;
  }

  return totalWaitingTime;
}
```

#

### Approach 2

Suppose the input array is `[8, 9, 10, 11]`. If the queries are executed in that order, the total waiting time is going to be `0 + 8 + (8 + 9) + (8 + 9 + 10) = 52`.

```
query time:  [8, 9,  10,      11]
              ^  ^   ^        ^
waiting time: 0, 8, (8 + 9), (8 + 9 + 10)
```

`0 + 8 + (8 + 9) + (8 + 9 + 10)` can also be written as `0 + 8 * 3 + 9 * 2 + 10 * 1`. It can be noticed that `3` is equal to the number of the queries that are after `8` in the array, so are `2` and `3`. Therefore, if there are `n` queries in the input array `queries`, the total waiting time is going to be

`duration of queries[0] * (n - 1) + duration of queries[1] * (n - 2) ... + duration of queries[n - 3] * 2 + duration of queries[n - 2] * 1`

Or

`duration of queries[0] * (n - 1) + duration of queries[1] * (n - 2) ... + duration of queries[n - 2] * 1 + duration of queries[n - 1] * 0`.

The reason is that all queries after a query need to wait the duration of that query. So to compute the total waiting time, I would initialize a variable that is going to keep track of the total waiting time so far, then iterate through every query in the input array, keeping track of the index each query is at, since I need to know how many queries are remaining in the array; at each query, multiply the duration of the query by the number of queries that are left, and add the result to the total waiting time.

Since the function needs to return the minimum amount of total waiting time, it would sort the input array at the very beginning.

### Time & Space Complexity

O(nlog(n)) time | O(1) space, where n is the number of queries.

### Solution 2

```js
function minimumWaitingTime(queries) {
  queries.sort((a, b) => a - b);

  let totalWaitingTime = 0;
  for (let idx = 0; idx < queries.length; idx++) {
    const duration = queries[idx];
    const queriesLeft = queries.length - (idx + 1);
    totalWaitingTime += duration * queriesLeft;
  }

  return totalWaitingTime;
}
```
