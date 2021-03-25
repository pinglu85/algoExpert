### Understanding the problem

Given two arrays, one called `competitions`, which is a 2-dimensional array containing arrays of two teams that have competed against each other in an tournament, the other called `results` which contains the result of each competition, I need to write a function that finds the winner of the tournament. The left item in the array of two teams is the home team, and the right one is the away team, each item is a string representing the name of the team. The `results` is an array of integer, each integer denotes the winner of each competition, where a `1` means that the home team won and a `0` means the away team won. A team receives 3 points when it wins and 0 points when it loses. There are no ties. Each team will compete against all other teams exactly once and there is only one winner. It's guaranteed that the tournament always has at least two teams.

### Approach - O(n) time | O(k) space, where n is the number of competitions and k is the number of teams.

I can use a hash table to store each team's score. Iterate through the `competitions` array. At each iteration, find the winning team of current competition by looking up the result in the `results` array, update the score of the winning team in the hash table. After we get all teams' scores, iterate through the hash table to find the team that has the best score.

### Solution

```js
function tournamentWinner(competitions, results) {
  const scores = new Map();

  for (let idx = 0; idx < competitions.length; idx++) {
    const [homeTeam, awayTeam] = competitions[idx];
    const result = results[idx];
    const winningTeam = result === 1 ? homeTeam : awayTeam;
    const currentScore = scores.get(winningTeam) || 0;
    scores.set(winningTeam, currentScore + 3);
  }

  let maxPoints = 0;
  let winner = '';
  scores.forEach((score, team) => {
    if (score > maxPoints) {
      maxPoints = score;
      winner = team;
    }
  });

  return winner;
}
```

### Improved Approach - O(n) time | O(k) space, where n is the number of competitions and k is the number of teams.

Instead of iterating through the hash table to find the winning team, we can keep track of the winning team when traversing the `competitions` array. Use a variable to keep track of the best score and another variable to keep track of the best team. At each iteration, find the winning team of the current competition and update its score, then compare it with current best score, if we get a better score, then update the best score and change the best team to the winning team.

### Improved Solution

```js
function tournamentWinner(competitions, results) {
  let currentBestTeam = '';
  let currentBestScore = 0;
  const scores = new Map();

  for (let idx = 0; idx < competitions.length; idx++) {
    const [homeTeam, awayTeam] = competitions[idx];
    const result = results[idx];
    const winningTeam = result === 1 ? homeTeam : awayTeam;
    const updatedScore = updateScores(winningTeam, 3, scores);

    if (updatedScore > currentBestScore) {
      currentBestScore = updatedScore;
      currentBestTeam = winningTeam;
    }
  }

  return currentBestTeam;
}

function updateScores(team, points, scores) {
  const prevScore = scores.get(team) || 0;
  const newScore = prevScore + points;
  scores.set(team, newScore);
  return newScore;
}
```
