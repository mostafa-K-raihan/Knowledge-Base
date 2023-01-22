Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane, return _the maximum number of points that lie on the same straight line_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/25/plane1.jpg)

**Input:** points = [[1,1],[2,2],[3,3]]
**Output:** 3

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/25/plane2.jpg)

**Input:** points = [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
**Output:** 4

**Constraints:**

-   `1 <= points.length <= 300`
-   `points[i].length == 2`
-   `-104 <= xi, yi <= 104`
-   All the `points` are **unique**.

```python
class Solution:
    def maxPoints(self, points: List[List[int]]) -> int:
        def getSlopeAndB(p1, p2):
            if (p1[0] == p2[0]):
                return (inf, p1[0])
            slope = (p1[1] - p2[1]) / (p1[0] - p2[0])
            b = p1[1] - (slope * p1[0])

            return (slope, b)


        N, res = len(points), 0
        for i in range(N):
            count = defaultdict(int)
            for j in range(i+1, N):
                s = getSlopeAndB(points[i], points[j])
                count[s] += 1
            if count:
                res = max(res, max(count.values()))
        return res+1
```