### Understanding the problem

I am given an array of positive integers, where each integer represents the amount of time that a query needs to take to execute. The queries are going to be executed one by one, but the execution order of the queries can be changed. I am asked to write a function that returns the minimum amount of total waiting time for all of the queries. I am allowed to mutate the input array.

Suppose the input array is going to be `[1, 3, 2]`. The first query can be executed immediately, and it takes 1 second to execute. Since the second query have to wait until the first one finishes before it can execute, the waiting time of the second query is going to be 1 second. The second query takes 3 seconds to execute, so the third query have to wait for 1 + 3 seconds. So if the queries are executed in that order, than the total awaiting time for all of the queries is going to be `(0) + (1) + (1 + 3) = 5`

### Approach - O(nlog(n)) time | O(1) space

Since I am asked to compute the minimum amount of total waiting time, I am going to sort the input array in ascending order at the very beginning. Suppose the input array is going to be `[2, 1]`, the total waiting time is going to be `(0) + (2) = 2`. However, if they are executed in the order of `[1, 2]`, then the waiting time is going to be `(0) + (1) = 1`.

Initialize a variable to keep track of the time that current query needs to wait; initially, set it to 0. Initialize another variable to store the amount of total waiting time so far. Traverse the sorted array element by element. At each integer, add the time that current query have to wait to the amount of total waiting time; then update the time that current query needs to wait by adding the integer to it. When I get out of the loop, return the amount of total waiting time.

### Solution

```js
function minimumWaitingTime(queries) {
  queries.sort((a, b) => a - b);

  let waitingTime = 0;
  let totalWaitingTime = 0;
  for (const query of queries) {
    totalWaitingTime += waitingTime;
    waitingTime += query;
  }

  return totalWaitingTime;
}
```
