You are given an integer array `matches` where `matches[i] = [winneri, loseri]` indicates that the player `winneri` defeated player `loseri` in a match.

Return _a list_ `answer` _of size_ `2` _where:_

-   `answer[0]` is a list of all players that have **not** lost any matches.
-   `answer[1]` is a list of all players that have lost exactly **one** match.

The values in the two lists should be returned in **increasing** order.

**Note:**

-   You should only consider the players that have played **at least one** match.
-   The testcases will be generated such that **no** two matches will have the **same** outcome.

**Example 1:**

**Input:** matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
**Output:** [[1,2,10],[4,5,7,8]]
**Explanation:**
Players 1, 2, and 10 have not lost any matches.
Players 4, 5, 7, and 8 each have lost one match.
Players 3, 6, and 9 each have lost two matches.
Thus, answer[0] = [1,2,10] and answer[1] = [4,5,7,8].

**Example 2:**

**Input:** matches = [[2,3],[1,3],[5,4],[6,4]]
**Output:** [[1,2,5,6],[]]
**Explanation:**
Players 1, 2, 5, and 6 have not lost any matches.
Players 3 and 4 each have lost two matches.
Thus, answer[0] = [1,2,5,6] and answer[1] = [].

**Constraints:**

-   1 <= matches.length <= 10<sup>5</sup>
-   `matches[i].length == 2`
-   1 <= winner<sub>i</sub>, loser<sub>i</sub> <= 10<sup>5</sup>
-   winner<sub>i</sub> != loser<sub>i</sub>
-   All `matches[i]` are **unique**.

```python
class Solution:
    def findWinners(self, matches: List[List[int]]) -> List[List[int]]:
        # brute force it
        # keep track of total unique players in a set
        # keep track of players who lost, and keep the count 
        players, lose = set(), defaultdict(int)
        
        for winner, loser in matches:
            players.add(winner)
            players.add(loser)
            lose[loser] += 1
        
        # iterate thru the player set
        # and see if they lost and how many times?
        zero, one = [], []
        
        for player in players:
            count = lose.get(player, 0)
            if count == 0:
                zero.append(player)
            if count == 1:
                one.append(player)
        
        # return sorted of both list
        return [sorted(zero), sorted(one)]
```