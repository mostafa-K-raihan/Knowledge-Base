Given an integer array `nums`, return _all the different possible non-decreasing subsequences of the given array with at least two elements_. You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [4,6,7,7]
**Output:** [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]

**Example 2:**

**Input:** nums = [4,4,3,2,1]
**Output:** [[4,4]]

**Constraints:**

-   `1 <= nums.length <= 15`
-   `-100 <= nums[i] <= 100`

```python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        res = set()

        def backtrack(index, sequences):
            nonlocal res
  
            if len(sequences) > 1:
                res.add(tuple(sequences))
  

            if index == len(nums):
                return
  

            if not sequences or nums[index] >= sequences[-1]:
                backtrack(index+1, sequences + [nums[index]])

            backtrack(index+1, sequences)
        backtrack(0, [])
        return res
```