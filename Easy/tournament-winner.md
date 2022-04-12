# Tournament Winner

### Understanding the problem

We're asked to imagine there is an algorithms tournament in which multiple teams compete against each other. In each competition there will be two teams that compete. There will be one winner and one loser out of all of these competitions and there are no ties. Each team will compete against all other teams exactly once. A team gets `3` points for each competition it wins and `0` points for each competition it loses. It's guaranteed that the tournament always has at least two teams and there will be only one tournament winner.

We are given two inputs, the `competitions` array and the `results` array. We need to write a function that returns the winner of the tournament, or more specifically, the name of the team that has the most number of points. The `competitions` array is an array of pairs, representing all of the competitions in the tournament. Inside of these pairs, we have two strings: the first one is the name of the home team and the second one is the name of the away team. The `results` array represents the winner of each of these competitions. Inside the `results` array, a `1` means that the home team won and a `0` means the away team won. The `results` array is the same length as the `competitions` array, and the indices in the `results` array correspond with the indices in the `competitions` array.

#

### Approach 1

We can use a hash table to keep track of each team's points, because a hash table is used to store key/value pairs, here each key is a team's name and the values are each team's points. Given a team name, we can retrieve its points. Moreover, hash tables provide constant-time lookup and insertion on average. Once we know all of the points for all the teams, we can go through the hash table and figure out which team has the most amount of points and then return the name of that team.

### Solution

```js
// Store `1` in a constant variable, so we can use the constant variable later
// rather than having to write 1 in our program. This will make our code more
// readable.
const HOME_TEAM_WON = 1;
const POINTS = 3;

function tournamentWinner(competitions, results) {
  const scores = new Map();

  for (let i = 0; i < competitions.length; i++) {
    const [homeTeam, awayTeam] = competitions[i];
    const result = results[i];
    const winningTeam = result === HOME_TEAM_WON ? homeTeam : awayTeam;
    const currentScore = scores.get(winningTeam) || 0;
    scores.set(winningTeam, currentScore + POINTS);
  }

  let currBestScore = 0;
  let currBestTeam = '';
  scores.forEach((score, team) => {
    if (score > currBestScore) {
      currBestScore = score;
      currBestTeam = team;
    }
  });

  return currBestTeam;
}
```

### Complexity Analysis

- Time Complexity: O(N), where N is the number of competitions.

- Space Complexity: O(K), where K is the number of teams.

#

### Approach 2: One Pass

Instead of iterating through the hash table to find the team that has the most number of points, we can keep track of the current best team as we build the hash table for team points. More specifically, we only need to remember the name of the current best team. To retrieve the current best team's score, we can simply look it up in the hash table.

### Implementation

JavaScript:

```js
const HOME_TEAM_WON = 1;
const POINTS = 3;

function tournamentWinner(competitions, results) {
  let currBestTeam = '';
  const scores = { [currBestTeam]: 0 };

  for (let i = 0; i < competitions.length; i++) {
    const [homeTeam, awayTeam] = competitions[i];
    const result = results[i];

    const winningTeam = result === HOME_TEAM_WON ? homeTeam : awayTeam;

    updateScores(scores, winningTeam);

    if (scores[winningTeam] > scores[currBestTeam]) {
      currBestTeam = winningTeam;
    }
  }

  return currBestTeam;
}

function updateScores(scores, team) {
  const prevScore = scores[team] || 0;
  scores[team] = prevScore + POINTS;
}
```

Go:

```go
package main

const HOME_TEAM_WON = 1
const POINTS = 3

func TournamentWinner(competitions [][]string, results []int) string {
	currBestTeam := ""
	scores := map[string]int{currBestTeam: 0}

	for i, competition := range competitions {
		homeTeam, awayTeam := competition[0], competition[1]
		result := results[i]

		winningTeam := awayTeam
		if result == HOME_TEAM_WON {
			winningTeam = homeTeam
		}

		scores[winningTeam] += POINTS

		if scores[winningTeam] > scores[currBestTeam] {
			currBestTeam = winningTeam
		}
	}

	return currBestTeam
}
```

### Complexity Analysis

- Time Complexity: O(N), where N is the number of competitions.

- Space Complexity: O(K), where K is the number of teams.
