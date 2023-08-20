Given a data stream input of non-negative integers `a1, a2, ..., an`, summarize the numbers seen so far as a list of disjoint intervals.

Implement the `SummaryRanges` class:

-   `SummaryRanges()` Initializes the object with an empty stream.
-   `void addNum(int value)` Adds the integer `value` to the stream.
-   `int[][] getIntervals()` Returns a summary of the integers in the stream currently as a list of disjoint intervals `[starti, endi]`. The answer should be sorted by `starti`.

**Example 1:**

**Input**
["SummaryRanges", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals"]
[[], [1], [], [3], [], [7], [], [2], [], [6], []]
**Output**
[null, null, [[1, 1]], null, [[1, 1], [3, 3]], null, [[1, 1], [3, 3], [7, 7]], null, [[1, 3], [7, 7]], null, [[1, 3], [6, 7]]]

**Explanation**
SummaryRanges summaryRanges = new SummaryRanges();
summaryRanges.addNum(1);      // arr = [1]
summaryRanges.getIntervals(); // return [[1, 1]]
summaryRanges.addNum(3);      // arr = [1, 3]
summaryRanges.getIntervals(); // return [[1, 1], [3, 3]]
summaryRanges.addNum(7);      // arr = [1, 3, 7]
summaryRanges.getIntervals(); // return [[1, 1], [3, 3], [7, 7]]
summaryRanges.addNum(2);      // arr = [1, 2, 3, 7]
summaryRanges.getIntervals(); // return [[1, 3], [7, 7]]
summaryRanges.addNum(6);      // arr = [1, 2, 3, 6, 7]
summaryRanges.getIntervals(); // return [[1, 3], [6, 7]]

**Constraints:**

-   `0 <= value <= 104`
-   At most `3 * 104` calls will be made to `addNum` and `getIntervals`.

**Follow up:** What if there are lots of merges and the number of disjoint intervals is small compared to the size of the data stream?


```python
class SummaryRanges:

  

    def __init__(self):

        self.nums = set()

  

    def addNum(self, value: int) -> None:

        self.nums.add(value)

  

    def getIntervals(self) -> List[List[int]]:

        intervals = []

        start = end = None

        for num in sorted(self.nums):

            if start is None:

                start = end = num

            elif num - end == 1:

                end = num

            else:

                intervals.append([start, end])

                start = end = num

        if start is not None:

            intervals.append([start, end])

        return intervals

  
  

# Your SummaryRanges object will be instantiated and called as such:

# obj = SummaryRanges()

# obj.addNum(value)

# param_2 = obj.getIntervals()
```

#shit
#hard 