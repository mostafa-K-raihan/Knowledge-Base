Implement the `RandomizedSet` class:

-   `RandomizedSet()` Initializes the `RandomizedSet` object.
-   `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
-   `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
-   `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

**Example 1:**

**Input**
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
**Output**
[null, true, false, true, 2, true, false, 2]

**Explanation**
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.

**Constraints:**

-   -2<sup>31</sup> <= val <= 2<sup>31</sup> - 1
-   At most 2 * 10<sup>5</sup> calls will be made to `insert`, `remove`, and `getRandom`.
-   There will be **at least one** element in the data structure when `getRandom` is called.
```python
class RandomizedSet:

    # get random and insert is easy
    # delete in O(1) is tricky
    # pop from the end of the list
    # but what happens for the item that is currently on the last
    # just swap ?
    # we need to track the index then
    def __init__(self):
        self.indices = {}
        self.data = []
        

    def insert(self, val: int) -> bool:
        # easy to check with a dict
        if val in self.indices:
            return False
        
        self.data.append(val);
        
        # track the index of current val
        self.indices[val] = len(self.data) - 1
        return True

    def remove(self, val: int) -> bool:
        if val not in self.indices:
            return False
        
        # get the index of the val from the dict
        index = self.indices[val]
        
        # update the index for the last element
        self.indices[self.data[-1]] = index
        
        # swap the two vals, last and val
        self.data[index] =  self.data[-1]
        
        # pop from the end, because the val is now at the end
        self.data.pop()
        
        # we need to pop the val from the dict as well
        # otherwise after this operation, if we call insert with the same 
        # val, it will mistakenly return True, when it should return False
        # because it is no longer in the list
        self.indices.pop(val)
        return True
        

    def getRandom(self) -> int:
        return random.choice(self.data)


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```