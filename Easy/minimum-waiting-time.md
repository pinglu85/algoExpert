# Minimum Waiting Time

### Understanding the problem

We are given an array of positive integers. Each integer in the input array represents the duration of a query that needs to be executed. Only one query can be executed at a time, but the queries can be executed in any order. We are asked to write a function that is going to return the minimum amount of total waiting time for all the queries. We are allowed to mutate the input array.

Suppose the input array is `[1, 3, 2]`. The first query can be executed immediately, therefore the waiting time of the first query is `0`. The second query have to wait until the first one finishes before it can execute, so the waiting time of the second query is going to be `1` second, the duration of the first query. The third query has to wait for both the first query and the second query to finish executing before it can start. Since the second query takes 3 seconds to execute, the waiting time of the third query is going to be 1 + 3 seconds. So if the queries are executed in that order, then the total awaiting time for all of the queries is going to be `(0) + (1) + (1 + 3) = 5`.

#

### Approach

Suppose the input array is `[6, 1]`. If the queries are executed in that order, the total waiting time is going to be `(0) + (6) = 6`. However, if they are executed in the order of `[1, 6]`, then the waiting time is going to be `(0) + (1) = 1`. Therefore, to get the minimum amount of the total waiting time, we need to figure out the optimal execution order. We can also noticed that if the largest query is executed first, all of the queries after it will have to wait at least the duration of the largest query. Conversely, if the shortest query is executed first, then all queries after it are for sure waiting the minimum amount of time, and since the largest query is going to be executed at the very end, that means no queries need to wait for the largest query to finish executing.

Based on the above observation, we could compute the minimum amount of total waiting time by first sorting the queries in ascending order and then summing up all of the waiting time of each individual query.

**Algorithm**

- Sort the input array in ascending order.

- Initialize a variable `nextWaitingTime` to `0`. It is going to keep track of the time that next query needs to wait.

- Initialize another variable to store the total waiting time so far.

- Traverse the sorted array. For each query in it,

  - add `nextWaitingTime` - the time that current query have to wait - to the total waiting time;

  - update `nextWaitingTime` by adding the duration of the current query to it.

- Return total waiting time.

### Implementation

```js
function minimumWaitingTime(queries) {
  queries.sort((a, b) => a - b);

  let nextWaitingTime = 0;
  let totalWaitingTime = 0;

  for (const duration of queries) {
    totalWaitingTime += nextWaitingTime;
    nextWaitingTime += duration;
  }

  return totalWaitingTime;
}
```

### Complexity Analysis

- Time Complexity: O(N · log(N)), where N is the number of queries.

- Space Complexity: O(1).

#

### Improved Approach

In fact, we don't need to keep track of the time that next query needs to wait.

Suppose the input array is `[8, 9, 10, 11]`. If the queries are executed in that order, the total waiting time is going to be `0 + 8 + (8 + 9) + (8 + 9 + 10) = 52`.

```
query time:   8   9     10          11
              ^   ^     ^           ^
waiting time: 0   8   (8 + 9)   (8 + 9 + 10)
```

`0 + 8 + (8 + 9) + (8 + 9 + 10) = 0 + 8 * 3 + 9 * 2 + 10 * 1`. It can be noticed that `3` is equal to the number of the queries that are after `8` in the array, so are `2` and `3`. Therefore, if there are `n` queries in the input array `queries`, the total waiting time is going to be:

```
duration of queries[0] * (n - 1) +
duration of queries[1] * (n - 2)
...
+ duration of queries[n - 3] * 2 +
duration of queries[n - 2] * 1
```

Or

```
duration of queries[0] * (n - 1) +
duration of queries[1] * (n - 2)
...
+ duration of queries[n - 2] * 1 +
duration of queries[n - 1] * 0`
```

The reason for this is that all queries after a query need to wait the duration of that query. Therefore to compute the total waiting time, we would initialize a variable that is going to keep track of the total waiting time so far, then iterate through every query in the input array, keeping track of the index each query is at, since we need to know how many queries are remaining in the array. For each query, multiply the duration of the query by the number of queries that are left, and add the result to the total waiting time.

### Implementation

JavaScript:

```js
function minimumWaitingTime(queries) {
  queries.sort((a, b) => a - b);

  let totalWaitingTime = 0;

  for (let i = 0; i < queries.length; i++) {
    const duration = queries[i];
    const queriesLeft = queries.length - (i + 1);
    totalWaitingTime += duration * queriesLeft;
  }

  return totalWaitingTime;
}
```

Go:

```go
package main

import (
	"sort"
)

func MinimumWaitingTime(queries []int) int {
	sort.Ints(queries)

	totalWaitingTime, numOfQueries := 0, len(queries)

	for i, duration := range queries {
		queriesLeft := numOfQueries - (i + 1)
		totalWaitingTime += duration * queriesLeft
	}

	return totalWaitingTime
}
```

### Complexity Analysis

- Time Complexity: O(N · log(N)), where N is the number of queries.

- Space Complexity: O(1).
