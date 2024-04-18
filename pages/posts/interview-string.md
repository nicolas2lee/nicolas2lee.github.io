---
title: Interview string
date: 2024-04-16
description: Interview string
tag: interview, algorithm, string
author: nicolas2lee
---

# Interview string
## String match
### Problem - String wildcard match
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
### Problem - Regular Expression Matching
[Leetcode 10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/description/)

Given string s and pattern p, return true if pattern p matches s, otherwise false. The rules are:
1. '?' matches any single character
2. '*' matches 0 or more of the preceding element.

For example:
```
s = "aa", p = "a*" -> true
s = "ab", p = ".*" -> true
```

### Solution
The diffcult part of the problem is how to handle the case ```.*``` correctly.
In the case ```.*```, we need to check the first char is match. If so then we check recursively:

1. ```.*``` matches 0 in string s, then move the pattern 2 chars

```isMatch(s, p.substring(2))```

2. ```.*``` matches 1 to n chars in string s, then move the string s 1 char step by step

```firstMatch && isMatch(s.substring(1), p)```

3. Other case, not ```*```

So move string s 1 char and pattern 1 char
```firstMatch && isMatch(s.substring(1), p.substring(1))```


```java
class Solution {
    public boolean isMatch(String s, String p) {
        if (p.length()==0) return s.length()==0;
        boolean firstMatch = s.length()>0 && (s.charAt(0) == p.charAt(0) || p.charAt(0)=='.');
        if (p.length()>=2 && p.charAt(1)=='*'){
            return isMatch(s, p.substring(2)) // match 0 pre*
            || (
                firstMatch && isMatch(s.substring(1), p) //match 1..n pre*
            );
        } else {
            // * syntax should be valid, check s[i] matches p[j]
            return  firstMatch && isMatch(s.substring(1), p.substring(1));
        }
        
    }
}
```
