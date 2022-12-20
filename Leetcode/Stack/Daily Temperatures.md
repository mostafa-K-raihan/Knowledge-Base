Given an array of integers `temperatures` represents the daily temperatures, return _an array_ `answer` _such that_ `answer[i]` _is the number of days you have to wait after the_ `ith` _day to get a warmer temperature_. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

**Example 1:**

**Input:** temperatures = [73,74,75,71,69,72,76,73]
**Output:** [1,1,4,2,1,1,0,0]

**Example 2:**

**Input:** temperatures = [30,40,50,60]
**Output:** [1,1,1,0]

**Example 3:**

**Input:** temperatures = [30,60,90]
**Output:** [1,1,0]

**Constraints:**

-   1 <= temperatures.length <= 10<sup>5</sup>
-   `30 <= temperatures[i] <= 100`


```python
class Solution:
	def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
		stack = [] # store the index of monotonic decreasing temps
		ans = [0] * len(temperatures)

		for index, temp in enumerate(temperatures):
			# keep poping from the stack
			# until we get a high temp from the stack index
			while stack and temperatures[stack[-1]] < temp:
				previousIndex = stack.pop()
				ans[previousIndex] = index - previousIndex
			stack.append(index)

		return ans
```

```typescript
function dailyTemperatures(temperatures: number[]): number[] {
    const ans = Array.from(temperatures, x => 0);
    const stack = []

    for (let i=0; i<temperatures.length; i++) {
        
        while (stack.length && temperatures[stack[stack.length - 1]] < temperatures[i]) {
            const prevIndex = stack.pop();
            ans[prevIndex] = i - prevIndex
        }
        stack.push(i);
    }

    return ans;
};
```