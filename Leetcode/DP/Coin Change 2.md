You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**

**Input:** amount = 5, coins = [1,2,5]
**Output:** 4
**Explanation:** there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

**Example 2:**
**Input:** amount = 3, coins = [2]
**Output:** 0
**Explanation:** the amount of 3 cannot be made up just with coins of 2.

**Example 3:**
**Input:** amount = 10, coins = [10]
**Output:** 1

**Constraints:**
-   `1 <= coins.length <= 300`
-   `1 <= coins[i] <= 5000`
-   All the values of `coins` are **unique**.
-   `0 <= amount <= 5000`


```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
	    ### Iterative bottom up DP
        coin_length = len(coins)
        dp = [[0 for _ in range(amount + 1)] for __ in range(coin_length)]
        
        # 0 1 2 3 4 5
        #0
        #1
        #2
        
        for i in range(coin_length):
            for j in range(amount + 1):
                if j == 0: # how many ways we can make j = 0 given 1, 2, 5 coins
                    # another base case
                    dp[i][j] = 1
                    continue
                
                # how many ways we can make amount j, given coins[0]..coins[i]
                used = j - coins[i]
                # dp[i-1][j] means we did not take coins[i] so our amount did not change
                # dp[i][used] means we did take coins[i], so our amount changed j => used
                dp[i][j] = dp[i-1][j] + (dp[i][used] if used >= 0 else 0) # overshoot
        
        return dp[coin_length-1][amount]
                
        
        # Recursive top down DP memoization
		cache = {}
        def dfs(index, _amount):
            # if out of coins or we overshoot the amount,
            # then there is no way we can make this to amount
            # so returning 0
            if index >= len(coins) or _amount > amount:
                return 0
            # if we can make amount, then there is 1 way
            if _amount == amount:
                return 1
            
            # we already computed 
            if (index, _amount) in cache:
                return cache[(index, _amount)]
            
            # either skip the current coin
            # or take the current coin
            cache[(index, _amount)] = dfs(index + 1, _amount) + dfs(index, _amount + coins[index])
            return cache[(index, _amount)]
                
        return dfs(0, 0)
        
```

