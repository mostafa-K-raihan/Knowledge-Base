https://leetcode.com/problems/best-team-with-no-conflicts/description/

You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the **sum** of scores of all the players in the team.

However, the basketball team is not allowed to have **conflicts**. A **conflict** exists if a younger player has a **strictly higher** score than an older player. A conflict does **not** occur between players of the same age.

Given two lists, `scores` and `ages`, where each `scores[i]` and `ages[i]` represents the score and age of the `ith` player, respectively, return _the highest overall score of all possible basketball teams_.

**Example 1:**

**Input:** scores = [1,3,5,10,15], ages = [1,2,3,4,5]
**Output:** 34
**Explanation:** You can choose all the players.

**Example 2:**

**Input:** scores = [4,5,6,5], ages = [2,1,2,1]
**Output:** 16
**Explanation:** It is best to choose the last 3 players. Notice that you are allowed to choose multiple people of the same age.

**Example 3:**

**Input:** scores = [1,2,3,5], ages = [8,9,10,1]
**Output:** 6
**Explanation:** It is best to choose the first 3 players. 

**Constraints:**

-   `1 <= scores.length, ages.length <= 1000`
-   `scores.length == ages.length`
-   `1 <= scores[i] <= 106`
-   `1 <= ages[i] <= 1000`

```python
class Solution:

    def bestTeamScore(self, scores: List[int], ages: List[int]) -> int:
        n = len(scores)
        players = list(zip(ages, scores))
        # sort according to age desc
        players.sort(reverse=True)
        dp = [0] * n

        ans = 0

		# we wanna take i
		# we also wanna take 0 - i-1 th player if they score more (because age sort)
		# but we wanna check if taking both, makes a greater yield
        for i in range(n):

            score = players[i][1]

            dp[i] = score # take this player

            for j in range(i):

                if players[j][1] >= players[i][1]:

                    # we can take the older player

                    # if they score more

                    # but check if collectively they outweigh prev score

                    dp[i] = max(dp[i], dp[j] + score)

            ans = max(ans, dp[i])

        return ans
```

#dp
