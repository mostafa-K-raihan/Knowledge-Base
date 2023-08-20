Design and implement a data structure for a [Least Frequently Used (LFU)](https://en.wikipedia.org/wiki/Least_frequently_used) cache.

Implement the `LFUCache` class:

-   `LFUCache(int capacity)` Initializes the object with the `capacity` of the data structure.
-   `int get(int key)` Gets the value of the `key` if the `key` exists in the cache. Otherwise, returns `-1`.
-   `void put(int key, int value)` Update the value of the `key` if present, or inserts the `key` if not already present. When the cache reaches its `capacity`, it should invalidate and remove the **least frequently used** key before inserting a new item. For this problem, when there is a **tie** (i.e., two or more keys with the same frequency), the **least recently used** `key` would be invalidated.

To determine the least frequently used key, a **use counter** is maintained for each key in the cache. The key with the smallest **use counter** is the least frequently used key.

When a key is first inserted into the cache, its **use counter** is set to `1` (due to the `put` operation). The **use counter** for a key in the cache is incremented either a `get` or `put` operation is called on it.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input**
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
**Output**
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

**Explanation**
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3

**Constraints:**

-   `0 <= capacity <= 104`
-   `0 <= key <= 105`
-   `0 <= value <= 109`
-   At most `2 * 105` calls will be made to `get` and `put`.

```python
class Node:
    def __init__(self, key, val):
        self.key = key
        self.value = val
        self.count = 1


class LFUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}
        self.cacheToCount = defaultdict(OrderedDict)
        self.minCount = None
  

    def get(self, key: int) -> int:
        # if we dont find the key in the cache
        if key not in self.cache:
            return -1
        # we do find the key in the cache
        node = self.cache[key]
        # clean the memory, if 5 was used 10 times before, it will be now 11 times
        # so we need to clear the entry for cacheToCount[10][node.key]
        # and add a new entry for cacheToCount[11][node.key]
        del self.cacheToCount[node.count][key]

		# but what if cacheToCount[10][node.key] was the last element 
        # there are no nodes at this count

        if not self.cacheToCount[node.count]:
            del self.cacheToCount[node.count]


        # inc the node usage
        node.count += 1
        self.cacheToCount[node.count][key] = node

        # we might have deleted the minCount key
        if not self.cacheToCount[self.minCount]:
            self.minCount += 1
        # return the value
        return node.value
  

    def put(self, key: int, value: int) -> None:
        if not self.capacity:
            return

        # if the key is present in the cache
        # just update the key map
        if key in self.cache:
            self.cache[key].value = value
            # since putting new value also update the usage
            # we can reuse get, without returning anything
            # get fn should update the freq

            self.get(key)
            return

  

        # key is not present in the cache
        # this is the first time we are seeing this key
        # but if the capacity is full we need to clear the cache
        # least frequently used key will be removed
        if len(self.cache) == self.capacity:
            # find the least frequently used
            # using the minCount
            # popitem returns the orderedDict at minCount
            # we just need the key to delete

            k, _ = self.cacheToCount[self.minCount].popitem(last=False)
            del self.cache[k]
  
        # we can finally put item
        newNode = Node(key, value)
        self.cache[key] = newNode
        # this node has been used only one time, hence the 1

        self.cacheToCount[1][key] = newNode
        self.minCount = 1


# Your LFUCache object will be instantiated and called as such:

# obj = LFUCache(capacity)

# param_1 = obj.get(key)

# obj.put(key,value)
```

#hard 
#system_design 