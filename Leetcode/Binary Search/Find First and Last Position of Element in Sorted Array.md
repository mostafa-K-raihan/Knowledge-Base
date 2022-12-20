Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [5,7,7,8,8,10], target = 8
**Output:** [3,4]

**Example 2:**

**Input:** nums = [5,7,7,8,8,10], target = 6
**Output:** [-1,-1]

**Example 3:**

**Input:** nums = [], target = 0
**Output:** [-1,-1]

**Constraints:**

-   0 <= nums.length <= 10<sup>5</sup>
-   -10<sup>9</sup> <= nums[i] <= 10<sup>9</sup>
-   `nums` is a non-decreasing array.
-   -10<sup>9</sup> <= target <= 10<sup>9</sup>


```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        # we have to run two binary search
        # one for finding the left most index that has target
        # the other is for the right most index that has target
        
        # from the regular bin search we need to modify once we hit
        # target == nums[mid]
        # regular binary search would end here
        # but for this particular problem
        # we need to go left/right again, but this time linearly
        # for left most index, we need to decrease the right by mid - 1
        # for right most index we need to increase the left by mid + 1
        # but keep track of the index, because it is the potential index
        def binSearch(goLeft):
            l, r = 0, len(nums) - 1
            i = -1
            while l <= r:
                mid = (l + r) >> 1
                
                if target > nums[mid]:
                    l = mid + 1
                elif target < nums[mid]:
                    r = mid - 1
                else:
                    i = mid
                    if goLeft:
                        r = mid - 1
                    else:
                        l = mid + 1
                    
            return i
        
        return [binSearch(True), binSearch(False)]
        
                
        
```