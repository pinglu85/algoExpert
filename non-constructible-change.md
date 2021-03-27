### Brute-force approach - O (m \* n) time | O(1) space, where n is the number of coins and m is the sum that all of the coins add up to.

### solution

```js
function nonConstructibleChange(coins) {
  coins.sort((a, b) => a - b);
  const maximumSum = coins.reduce((sum, value) => sum + value, 0);

  for (let sum = 1; sum < maximumSum; sum++) {
    let currentSum = sum;
    for (let idx = coins.length - 1; idx >= 0; idx--) {
      const currentValue = coins[idx];
      if (currentValue <= currentSum) {
        currentSum -= currentValue;
      }
      if (currentSum === 0) {
        break;
      }
    }

    if (currentSum > 0) {
      return sum;
    }
  }

  return maximumSum + 1;
}
```
