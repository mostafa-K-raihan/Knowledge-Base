There are `n` rooms labeled from `0` to `n - 1` and all the rooms are locked except for room `0`. Your goal is to visit all the rooms. However, you cannot enter a locked room without having its key.

When you visit a room, you may find a set of **distinct keys** in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms.

Given an array `rooms` where `rooms[i]` is the set of keys that you can obtain if you visited room `i`, return `true` _if you can visit **all** the rooms, or_ `false` _otherwise_.

**Example 1:**

**Input:** rooms = [[1],[2],[3],[]]
**Output:** true
**Explanation:** 
We visit room 0 and pick up key 1.
We then visit room 1 and pick up key 2.
We then visit room 2 and pick up key 3.
We then visit room 3.
Since we were able to visit every room, we return true.

**Example 2:**

**Input:** rooms = [[1,3],[3,0,1],[2],[0]]
**Output:** false
**Explanation:** We can not enter room number 2 since the only key that unlocks it is in that room.

**Constraints:**

-   `n == rooms.length`
-   `2 <= n <= 1000`
-   `0 <= rooms[i].length <= 1000`
-   `1 <= sum(rooms[i].length) <= 3000`
-   `0 <= rooms[i][j] < n`
-   All the values of `rooms[i]` are **unique**.

# Intuition

Since the key to unlock a particular room can be found in another room, there is a direct co-relation between this two rooms. This prompted me to think like this problem might be modeled as a graph problem. So the two rooms, have an directed edge between them.

The problem only wants to find if it is possible to go to all the rooms. We can go to the starting room (since it is the only one which is unlocked to begin with), and will find all the keys that can be unlocked from the first room. This prompted me to have a BFS solution.

# Complexity

-   Time complexity:  
    O(n)

-   Space complexity:  
    O(n)

# Code

```python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        # start with the first room
        q = deque([0])

        # keep track of visited rooms
        visited = set()

        while q:
            front = q.popleft()
            visited.add(front)

            for next_room in rooms[front]:
                if next_room not in visited:
                    # this next_room has not been visited yet
                    # so we add this room visit in our to do list
                    q.append(next_room)

        # finally check if we visited all the rooms
        # since visited holds only unique room id
        # comparing length would suffice
        return len(visited) == len(rooms)
```