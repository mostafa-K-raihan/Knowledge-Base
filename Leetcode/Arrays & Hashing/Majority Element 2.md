Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

**Example 1:**

**Input:** nums = [3,2,3]
**Output:** [3]

**Example 2:**

**Input:** nums = [1]
**Output:** [1]

**Example 3:**

**Input:** nums = [1,2]
**Output:** [1,2]

**Constraints:**

-   1 <= nums.length <= 5 * 10<sup>4</sup>
-   -10<sup>9</sup> <= nums[i] <= 10<sup>9</sup>

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:
	count, c = defaultdict(int), 0
	        
	target = len(nums) // 3
	        
	for n in nums:
		count[n] += 1
	        
		res = []
		for key, value in count.items():
			if value > target:
		          res.append(key)
	        
	        
	    return res

```
**Follow up:** Could you solve the problem in linear time and in `O(1)` space?

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:
	# first thing to notice here is that
    # there can't possibly be 3 elements in the ans set
    # because if two elements are more than n / 3,
    # for third elements to become more than n / 3 would result
    # number of elements > n e.g.
    # more than n/3 + more than n/3 + more than n/3 = more than > n
            
	# so we need to find first two elements who has the most elements
        num1, num2 = -1, -2 # need to be different, these are sample candidates, can be anything, really
        # we are saying that num1 is the highest
        # num2 is the 2nd highest number

        count1, count2 = 0, 0 # their respective counts/votes

        for n in nums:
            if n == num1:
                # another element matching the highest, more vote
                count1 += 1
            elif n == num2:
                # another element matching 2nd highest, more vote
                count2 += 1
            elif count1 == 0:
                # assign this to highest candidate
                num1 = n
                count1 = 1
            elif count2 == 0:
                # assign this to 2nd highest, since count1 is not 0
                # our bias is towards num1 first
                num2 = n
                count2 = 1

            else:
                # completely different number
                # and the count of 1st and 2nd highest number is not 0, less vote
                # for these 2 candidates
                count1 -= 1
                count2 -= 1


        # we are done with the voting
        # 2nd pass, see how many votes those candidate actually made

        count1, count2 = 0, 0
        for n in nums:
            if n == num1:
                count1 += 1
            elif n == num2:
                count2 += 1

        target = len(nums) // 3
        res = []

        # print (num1, num2)
        if count1 > target:
            res.append(num1)
        if count2 > target:
            res.append(num2)

        return res
```