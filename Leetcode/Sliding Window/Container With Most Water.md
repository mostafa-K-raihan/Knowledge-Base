### Problem Statement
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return _the maximum amount of water a container can store_.

**Notice** that you may not slant the container.

**Example 1**
![[Pasted image 20221029090417.png]]

**Input:** height = [1,8,6,2,5,4,8,3,7]
**Output:** 49
**Explanation:** The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

**Example 2**
**Input:** height = [1,1]
**Output:** 1

**Constraints**
-   `n == height.length`
-   2 <= n <= 10<sup>5</sup>
-   0 <= height[i] <= 10<sup>4</sup>

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        # idea is to maximize the width and height as much we can
        # width will be (r - l)
        # height will be min(height[r], height[l])
        # we will maintain two pointers l and r
        # l will move right if height[l] is smaller than height[r]
        # r will move left if height[r] is smaller than height[l]
        # why??
        # because we want to maximize the height, so we want to change if
        # one of l, r height is smaller
        
        l, r = 0, len(height) -1
        res = 0
        while l < r:
            area = (r - l) * min(height[l], height[r])
            res = max(area, res)
            
            if height[l] < height[r]:
                # now try to move right, cuz this l is smaller
                l += 1
            else:
                # now try to move left, cuz this r is smaller
                r -= 1
        
        return res
```