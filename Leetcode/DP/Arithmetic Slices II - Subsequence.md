Given an integer array `nums`, return _the number of all the **arithmetic subsequences** of_ `nums`.

A sequence of numbers is called arithmetic if it consists of **at least three elements** and if the difference between any two consecutive elements is the same.

-   For example, `[1, 3, 5, 7, 9]`, `[7, 7, 7, 7]`, and `[3, -1, -5, -9]` are arithmetic sequences.
-   For example, `[1, 1, 2, 5, 7]` is not an arithmetic sequence.

A **subsequence** of an array is a sequence that can be formed by removing some elements (possibly none) of the array.

-   For example, `[2,5,10]` is a subsequence of `[1,2,1,**2**,4,1,**5**,**10**]`.

The test cases are generated so that the answer fits in **32-bit** integer.

**Example 1:**

**Input:** nums = [2,4,6,8,10]
**Output:** 7
**Explanation:** All arithmetic subsequence slices are:
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]

**Example 2:**

**Input:** nums = [7,7,7,7,7]
**Output:** 16
**Explanation:** Any subsequence of this array is arithmetic.

**Constraints:**

-   `1  <= nums.length <= 1000`
-   -2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1

```python
class Solution:
    def numberOfArithmeticSlices(self, nums: List[int]) -> int:
        # need to find all subsequnce that has arithmetic diff and the length of subseq >= 3
        # dp[i][diff] means the subseq ends with ith element 
        # and the subseq has the difference of diff
        # since diff can be huge, we store the diff for each element as a dict
        dp = [defaultdict(int) for i in range(len(nums))]
        ans = 0
        # iterate thru every possible subseq
        # for each i
        # go thru each possible element j upto i
        for i in range(1, len(nums)):
            for j in range(0, i):
                diff = nums[i] - nums[j]
	            # this diff can be unique, or repeated

				# if its repeated, we will get the count, 
				# if its unique, we will have 0 in dp[j][diff]
                cnt = dp[j].get(diff, 0)
	            # update the count for dp[i][diff], for unique, cnt = 0, so +1
	            # for repeated, cnt = dp[j][diff], so # of sub ending j with diff + 1 (for i)
                dp[i][diff] += cnt + 1
	            # why cnt gets added? cnt means # sub ending with j with diff and a subseq must be of 2 elements, and we now added ith element as well, so >=3
                ans += cnt

#                 dp[i][diff] +=1
    
#                 cntFromJ = dp[j].get(diff, 0) # might be zero as well
                
#                 if (cntFromJ):
#                     ans += cntFromJ # cuz now its >= 3 (j has something, not zero, and i, j)
#                 dp[i][diff] += cntFromJ
                
        return ans
        
```