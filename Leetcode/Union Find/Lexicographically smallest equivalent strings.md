You are given two strings of the same length `s1` and `s2` and a string `baseStr`.

We say `s1[i]` and `s2[i]` are equivalent characters.

-   For example, if `s1 = "abc"` and `s2 = "cde"`, then we have `'a' == 'c'`, `'b' == 'd'`, and `'c' == 'e'`.

Equivalent characters follow the usual rules of any equivalence relation:

-   **Reflexivity:** `'a' == 'a'`.
-   **Symmetry:** `'a' == 'b'` implies `'b' == 'a'`.
-   **Transitivity:** `'a' == 'b'` and `'b' == 'c'` implies `'a' == 'c'`.

For example, given the equivalency information from `s1 = "abc"` and `s2 = "cde"`, `"acd"` and `"aab"` are equivalent strings of `baseStr = "eed"`, and `"aab"` is the lexicographically smallest equivalent string of `baseStr`.

Return _the lexicographically smallest equivalent string of_ `baseStr` _by using the equivalency information from_ `s1` _and_ `s2`.

**Example 1:**

**Input:** s1 = "parker", s2 = "morris", baseStr = "parser"
**Output:** "makkek"
**Explanation:** Based on the equivalency information in s1 and s2, we can group their characters as [m,p], [a,o], [k,r,s], [e,i].
The characters in each group are equivalent and sorted in lexicographical order.
So the answer is "makkek".

**Example 2:**

**Input:** s1 = "hello", s2 = "world", baseStr = "hold"
**Output:** "hdld"
**Explanation:** Based on the equivalency information in s1 and s2, we can group their characters as [h,w], [d,e,o], [l,r].
So only the second letter 'o' in baseStr is changed to 'd', the answer is "hdld".

**Example 3:**

**Input:** s1 = "leetcode", s2 = "programs", baseStr = "sourcecode"
**Output:** "aauaaaaada"
**Explanation:** We group the equivalent characters in s1 and s2 as [a,o,e,r,s,c], [l,p], [g,t] and [d,m], thus all letters in baseStr except 'u' and 'd' are transformed to 'a', the answer is "aauaaaaada".

**Constraints:**

-   `1 <= s1.length, s2.length, baseStr <= 1000`
-   `s1.length == s2.length`
-   `s1`, `s2`, and `baseStr` consist of lowercase English letters.


```python
class Solution:

    def smallestEquivalentString(self, s1: str, s2: str, baseStr: str) -> str:
        UF = {}

        def find(x):
            UF.setdefault(x, x)
            if x != UF[x]:
                UF[x] = find(UF[x])
            return UF[x]

        def union(x, y):
            rootX, rootY = find(x), find(y)
            if rootX > rootY:
                UF[rootX] = rootY
            else:
                UF[rootY] = rootX

        for a, b in zip(s1, s2):
            union(a, b)
        res = []
        for c in baseStr:
	             res.append(find(c))
        return ''.join(res)
```

for faster access, use array, but be careful on the conversion
```python
parent = [-1 for x in range(26)]

def find(x):
	if parent[x] == -1:
		return x
	else:
		parent[x] = find(parent[x]) 
		return parent[x]

def union(x, y):
	rootx, rooty = find(x), find(y)
	if rootx != rooty:
		# we want lexicographically smallest string
		# when doing union, we got two root for x, y
		# if they are different, we need to merge the two group
		# now which will be the base group?
		# that one whose root is lexicographically smaller
		# parent of max roots means assigning max root group to min group
		parent[max(rootx, rooty)] = min(rootx, rooty)

for i in range(len(s1)):
	union(ord(s1[i]) - 97, ord(s2[i]) - 97)

res = []
for c in baseStr:
	res.append(chr(find(ord(c) - 97) + 97))
return ''.join(res)
```