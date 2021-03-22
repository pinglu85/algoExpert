### Understanding the problem

Given two arrays, one called `competitions`, which is a 2-dimensional array containing arrays of two teams that have competed against each other in an tournament, the other called `results` which contains the result of each competition, I need to write a function that finds the winner of the tournament. The left item in the array of two teams is the home team, and the right one is the away team, each item is a string representing the name of the team. The `results` is an array of integer, each integer denotes the winner of each competition, where a `1` means that the home team won and a `0` means the away team won. A team receives 3 points when it wins and 0 points when it loses. There are no ties. Each team will compete against all other teams exactly once and there is only one winner. It's guaranteed that the tournament always has at least two teams.

### Approach - O(n) time | O(k) space, where n is the number of competitions and k is the number of teams.

I can use a hash table to keep track of each team's points. Iterate through the `competitions` array. At each iteration, get both teams' current points from the hash table, set the points to 0, if either of them is `undefined`, look up the winner of current competition in the `results` array, update both teams' points, store the team as key and its points as value in the hash table. After the loop, iterate through the hash table and find the team that has the most amount of points.

### Solution

```js
function tournamentWinner(competitions, results) {
  const pointsMap = new Map();

  for (let i = 0; i < competitions.length; i++) {
    const [homeTeam, awayTeam] = competitions[i];
    let homeTeamPoints = pointsMap.get(homeTeam) || 0;
    let awayTeamPoints = pointsMap.get(awayTeam) || 0;
    if (results[i] === 1) {
      homeTeamPoints += 3;
    } else {
      awayTeamPoints += 3;
    }

    pointsMap.set(homeTeam, homeTeamPoints);
    pointsMap.set(awayTeam, awayTeamPoints);
  }

  let maxPoints = 0;
  let winner = '';
  pointsMap.forEach((value, key) => {
    if (value > maxPoints) {
      maxPoints = value;
      winner = key;
    }
  });

  return winner;
}
```
