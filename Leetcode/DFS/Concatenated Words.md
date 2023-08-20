https://leetcode.com/problems/concatenated-words/description/

Given an array of strings `words` (**without duplicates**), return _all the **concatenated words** in the given list of_ `words`.

A **concatenated word** is defined as a string that is comprised entirely of at least two shorter words in the given array.

**Example 1:**

**Input:** words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]
**Output:** ["catsdogcats","dogcatsdog","ratcatdogcat"]
**Explanation:** "catsdogcats" can be concatenated by "cats", "dog" and "cats"; 
"dogcatsdog" can be concatenated by "dog", "cats" and "dog"; 
"ratcatdogcat" can be concatenated by "rat", "cat", "dog" and "cat".

**Example 2:**

**Input:** words = ["cat","dog","catdog"]
**Output:** ["catdog"]

**Constraints:**

-   `1 <= words.length <= 104`
-   `1 <= words[i].length <= 30`
-   `words[i]` consists of only lowercase English letters.
-   All the strings of `words` are **unique**.
-   `1 <= sum(words[i].length) <= 105`

```python
class Solution:
    def findAllConcatenatedWordsInADict(self, words: List[str]) -> List[str]:
        memo = {}
        d = set(words)

        def dfs(w):
            if w in memo:
                return memo[w]


            memo[w] = False
            for i in range(1, len(w)):
                prefix, suffix = w[:i], w[i:]

                if prefix in d and suffix in d:
                    memo[w] = True
                    break

                if prefix in d and dfs(suffix):
                    memo[w] = True
                    break
            return memo[w]

  
		# for each word
		# we run a dfs
		# dfs will eventually return if it is a concatenated word or not
		# how do we check if it is a concatenated word?
		# in order to be concated, at least two words need to be used to construct that
		# word
		# so we start from there
		# we go thru each position, and create a split
		# prefix to i, and suffix after i
		# we check if prefix and suffix is both in the word list (by leveragin a set)
		# if not we can run dfs on the suffix 
		# once we know the result we can cache the result to speed things a bit
        return [w for w in words if dfs(w)]
```

#hard
#dfs 
#string
