---
title: Interview inverted index search
date: 2024-04-20
description: Interview inverted index search
tag: interview, algorithm, search, inverted index
author: nicolas2lee
---

# Interview inverted index search
## Is Subsequence
### Problem - Is Subsequence
[Leetcode 392. Is Subsequence](https://leetcode.com/problems/is-subsequence/description/)

The problem itself is very easy to solve. We can use 2 pointer to check 1 by 1 in each string. Here we want to discuss the follow up:
```
Suppose there are lots of incoming s, 
say s1, s2, ..., sk where k >= 109, and you want to check one by one to see if t has its subsequence. In this scenario, how would you change your code?
```

### Solution
Build a inverted index hashmap with:
k: the character 
v: the sorted list of character position

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        // precomputation, build the hashmap out of the target string
        HashMap<Character, List<Integer>> letterIndicesTable = new HashMap<>();
        for (int i = 0; i < t.length(); ++i) {
            var indices = letterIndicesTable.getOrDefault(t.charAt(i), new ArrayList<>());
            indices.add(i);
            letterIndicesTable.put(t.charAt(i), indices);
        }
        Integer currMatchIndex = -1;
        for (char letter : s.toCharArray()) {
            if (!letterIndicesTable.containsKey(letter)) return false; // no match, early exit
            boolean isMatched = false;
            // greedy match with linear search
            for (Integer matchIndex : letterIndicesTable.get(letter)) {
                if (currMatchIndex < matchIndex) {
                    currMatchIndex = matchIndex;
                    isMatched = true;
                    break;
                }
            }
            if (!isMatched) return false;
        }
        // consume all characters in the source string
        return true;
    }
}
```

## Shortest Way to Form String
### Problem - Shortest Way to Form String
[Leetcode 1055. Shortest Way to Form String](https://leetcode.com/problems/shortest-way-to-form-string/description/)

Given two strings ```source``` and ```target```, return the minimum number of subsequences of source such that their concatenation equals target. 
If the task is impossible, return -1.

Example:
```
source = "abc", target = "abcbc" -> 2
source = "abc", target = "acdbc" -> -1
source = "xyz", target = "xzyxz" -> 3
```

### Solution
The problem itself is also easy to solve, we can use 2 pointer to solve it, so time xomplexity will be O(m*n) m is length of source, n is length of target.
Here we want to discuss the follow up, which is similar as problem ```Is Subsequence```, given multiple sources

Use a 2D array instead of HashMap 

```java
class Solution {
    public int shortestWay(String source, String target) {
        // Next occurrence of a character after a given index
        int[][] nextOccurrence = new int[source.length()][26];
        // Base Case
        for (int c = 0; c < 26; c++) {
            nextOccurrence[source.length() - 1][c] = -1;
        }
        nextOccurrence[source.length() - 1][source.charAt(source.length() - 1) - 'a'] = source.length() - 1;
        // Fill using recurrence relation
        for (int idx = source.length() - 2; idx >= 0; idx--) {
            for (int c = 0; c < 26; c++) {
                nextOccurrence[idx][c] = nextOccurrence[idx + 1][c];
            }
            nextOccurrence[idx][source.charAt(idx) - 'a'] = idx;
        }

        // Pointer to the current index in source
        int sourceIterator = 0;
        // Number of times we need to iterate through source
        int count = 1;
        // Find all characters of target in source
        for (char c : target.toCharArray()) {
            // If the character is not present in source
            if (nextOccurrence[0][c - 'a'] == -1) {
                return -1;
            }
            // If we have reached the end of source, or the character is not in
            // source after source_iterator, loop back to beginning
            if (sourceIterator == source.length() || nextOccurrence[sourceIterator][c - 'a'] == -1) {
                count++;
                sourceIterator = 0;
            }
            // Next occurrence of character in source after source_iterator
            sourceIterator = nextOccurrence[sourceIterator][c - 'a'] + 1;
        }
        // Return the number of times we need to iterate through source
        return count;
    }
}
```
