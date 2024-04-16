---
title: Interview string
date: 2024-04-16
description: Interview string
tag: interview, algorithm, string
author: nicolas2lee
---

# Interview string
## String wildcard match
### Problem
[Leetcode 44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/description/)

Given string s and pattern p, return true if pattern p matches s, otherwise false. The rules are:
1. '?' matches any single character
2. '*' matches 0 to n characters

For example:
```
s = "cb", p = "?a" -> false
s = "acdcb", p = "a*c?b" -> false
s = "bbarc", p = "*c" -> true 
```
### Solution
For the below cases are quite easy:
s[i] == p[j] or p[j] = '?'
The diffcult part is when we have '*' in pattern p.
So the solution is we need to match '*nextChar' in pattern p with string s, if we could not find any string in s match pattern '*nextChar',
then we can return false.

[A very interesting video explains the solution](https://www.youtube.com/watch?v=-8QnRMyHo_o&ab_channel=%E7%88%B1%E5%BE%B7%E5%8D%8E%E8%AF%B4-%E7%95%99%E5%AD%A6%E7%94%9F%E6%B1%82%E8%81%8C)

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int si = 0;
        int pi = 0;
        int pLen = p.length();
        int pStar = -1;
        int sMatch = -1;
        while (si<s.length()){
            if (pi< pLen && (s.charAt(si) == p.charAt(pi) || p.charAt(pi)=='?')){
                si++; pi++;
            }else if (pi<pLen && p.charAt(pi)=='*'){
                pStar = pi;
                sMatch = si;
                pi++; 
            } else if (pStar!=-1){
                pi=pStar+1;
                sMatch++;
                si=sMatch;
            } else return false;
        }
        while (pi<pLen && p.charAt(pi)=='*') pi++;
        return pi==p.length();
    }
}
```
