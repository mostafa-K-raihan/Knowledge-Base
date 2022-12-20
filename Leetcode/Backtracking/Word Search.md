Given an `m x n` grid of characters `board` and a string `word`, return `true` _if_ `word` _exists in the grid_.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
**Output:** true

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
**Output:** false

**Constraints:**

-   `m == board.length`
-   `n = board[i].length`
-   `1 <= m, n <= 6`
-   `1 <= word.length <= 15`
-   `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        ROW, COL = len(board), len(board[0])
        cnt = Counter(word)
        
        # search from the back if freq is less there
        # the reason it works
        # say we have 3 A from the start, 
        # we need to match these 3 As in the board
        # this requires extensive search thru the board
        
        word = word[::-1] if cnt[word[0]] > cnt[word[-1]] else word
        
        def dfs(r, c, i):
            
            if i == len(word):
                return True

            if (r not in range(ROW) or
                c not in range(COL) or
                board[r][c] != word[i]
            ):
                return False
            
            
            #found a matching char
            temp, board[r][c] = board[r][c], '#'
            # now branch out, bf = 4
            res = (dfs(r+1, c, i+1) or 
                   dfs(r-1, c, i+1) or
                   dfs(r, c+1, i+1) or
                   dfs(r, c-1, i+1)
                  )
            
            # we explored all possible ways from r,c
            # we no longer need r, c to be on the path
            board[r][c] = temp
            return res
        
        for r in range(ROW):
            for c in range(COL):
                if dfs(r, c, 0):
                    return True
                
        return False
```