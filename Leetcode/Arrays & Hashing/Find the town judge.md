n a town, there are `n` people labeled from `1` to `n`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

1.  The town judge trusts nobody.
2.  Everybody (except for the town judge) trusts the town judge.
3.  There is exactly one person that satisfies properties **1** and **2**.

You are given an array `trust` where `trust[i] = [ai, bi]` representing that the person labeled `ai` trusts the person labeled `bi`.

Return _the label of the town judge if the town judge exists and can be identified, or return_ `-1` _otherwise_.

**Example 1:**

**Input:** n = 2, trust = [[1,2]]
**Output:** 2

**Example 2:**

**Input:** n = 3, trust = [[1,3],[2,3]]
**Output:** 3

**Example 3:**

**Input:** n = 3, trust = [[1,3],[2,3],[3,1]]
**Output:** -1

**Constraints:**

-   `1 <= n <= 1000`
-   `0 <= trust.length <= 104`
-   `trust[i].length == 2`
-   All the pairs of `trust` are **unique**.
-   `ai != bi`
-   `1 <= ai, bi <= n`

```python
class Solution:
    def findJudge(self, n: int, trust: List[List[int]]) -> int:
        a = [0 for i in range(n+1)]
        for x, y in trust:
            a[x] -= 1
            a[y] += 1

		# find the index from 1 to n
		# who has in degree - out degree = n - 1
		# everyone except the judge trust the judge
		# so in degree will be n-1
		# judge trusts no one, so out degree of judge will be 0
		# n - 1 - 0 = n - 1
        for i in range(1, n+1):
            if a[i] == n - 1:
                return i

        return -1
```

#easy