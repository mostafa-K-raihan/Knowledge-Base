### Problem Statement
Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1**
**Input:** nums = [100,4,200,1,3,2]
**Output:** 4
**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

**Example2**
**Input:** nums = [0,3,7,2,5,8,4,6,0,1]
**Output:** 9

**Constraints**
-   0 <= nums.length <= 10<sup>5</sup>`
-   -10<sup>9</sup> <= nums[i] <= 10<sup>9</sup>


```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        # convert the nums to a set for faster access
        # for each element, check if the element - 1 is present in the set
        # if its not, then element is the start of the sequence
        # then search for consecutive elements from that element and keep track of the length
        
        s, l = set(nums), len(nums)
        res = 0
        for i in range(l):
            num = nums[i]
            if num - 1 not in s:
                # then num is the start of a new sequence
                # keep track of the length
                longest = 0
                while (num + longest) in s:
                    longest += 1
                
                res = max(res, longest)
                
        return res
```