Given a string `s`, sort it in **decreasing order** based on the **frequency** of the characters. The **frequency** of a character is the number of times it appears in the string.

Return _the sorted string_. If there are multiple answers, return _any of them_.

**Example 1:**

**Input:** s = "tree"
**Output:** "eert"
**Explanation:** 'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.

**Example 2:**

**Input:** s = "cccaaa"
**Output:** "aaaccc"
**Explanation:** Both 'c' and 'a' appear three times, so both "cccaaa" and "aaaccc" are valid answers.
Note that "cacaca" is incorrect, as the same characters must be together.

**Example 3:**

**Input:** s = "Aabb"
**Output:** "bbAa"
**Explanation:** "bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.

**Constraints:**

-   `1 <= s.length <= 5 * 105`
-   `s` consists of uppercase and lowercase English letters and digits.

```js
/**

 * @param {string} s

 * @return {string}

 */
var frequencySort = function(s) {
    const Map = s.split('')
        .reduce((acc, next) => {
            acc[next] = (acc[next] || 0) + 1;
            return acc;
        }, {});

    return Object.entries(Map)
        .sort(([_, a], [__, b]) => b - a)
        .reduce((acc, [char, freq]) => {
            return acc + char.repeat(freq);
        }, '');
};
```