## Coding Challenge
- given an array of number, compute the min absolute diff of the consecutive numbers
	- strategy sort the array, that way the diff would be minimum (solved it, pretty easy actually)
- given 2 strings, compute min edits to make them anagram
	- medium 
		- first check length, if mismatch return -1
		- if match build freq set, add for first item, delete for 2nd
		- iterate over the values of set if we find positive int add them up
- given k coins, compute how many ways we can make a given amount module 10^9 + 7
	- classic coin change but unbounded, [[Coin Change 2]] Only gotcha here is in the actual question, the ans might not fit in the 32 bit integer, so we need to mod the result
	
- given a grid of 1/0, compute maximum square length full of 1
	- [Maximal Square](https://leetcode.com/problems/maximal-square/)
	- Couldn't solve it, was thinking of dfs


### Telephone Round
- find middle of the linked list [here](obsidian://open?vault=knowledge-base&file=Leetcode%2FDecember%202022%2FMiddle%20of%20the%20Linked%20List)
- given a city and countries table, output country with number of cities from highest to lowest
- given an array, find the starting of the maximum sum of the subarray