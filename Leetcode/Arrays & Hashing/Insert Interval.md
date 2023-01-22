You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` _after the insertion_.

**Example 1:**

**Input:** intervals = [[1,3],[6,9]], newInterval = [2,5]
**Output:** [[1,5],[6,9]]

**Example 2:**

**Input:** intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
**Output:** [[1,2],[3,10],[12,16]]
**Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

**Constraints:**

-   `0 <= intervals.length <= 104`
-   `intervals[i].length == 2`
-   `0 <= starti <= endi <= 105`
-   `intervals` is sorted by `starti` in **ascending** order.
-   `newInterval.length == 2`
-   `0 <= start <= end <= 105`

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        res = []
        for i, (s, e) in enumerate(intervals):
            # if the new interval ending is smaller than current interval's start
            # we can just add the new interval
            # and add the rest of the intervals and return
            # since it wont collide with any of them anyways

            if newInterval[1] < s:
                res.append(newInterval)
                return res + intervals[i:]
            # if the new interval starting is greater than the current interval's end
            # we can safely append current interval
            # but the new interval we cannot
            # cuz it might collide with the next one
            elif newInterval[0] > e:
                res.append([s, e])
            else:
                # this means the newInterval is colliding with the current
                # so form the new Interval by taking min of the start pos
                # and max of the end pos
                newInterval = [min(s, newInterval[0]), max(e, newInterval[1])]

        # this means newInterval/merged interval never added,
        res.append(newInterval)

        return res
```