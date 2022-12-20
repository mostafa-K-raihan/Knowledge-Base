Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

**Input:** strs = ["flower","flow","flight"]
**Output:** "fl"

**Example 2:**

**Input:** strs = ["dog","racecar","car"]
**Output:** ""
**Explanation:** There is no common prefix among the input strings.

**Constraints:**

-   `1 <= strs.length <= 200`
-   `0 <= strs[i].length <= 200`
-   `strs[i]` consists of only lowercase English letters.

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        # find min length str
        shortest = min(strs, key=len)
        
        # for each char, check all the given str
        # if the char does not match,
        # then we found the prefix
        # it is upto that char in shortest str
        
        for i, ch in enumerate(shortest):
            for str in strs:
                if str[i] != ch:
                    return shortest[:i]
                
        return shortest
        
```