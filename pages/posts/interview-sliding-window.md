---
title: Interview sliding window
date: 2024-04-18
description: Interview sliding window
tag: interview, algorithm, sliding window
author: nicolas2lee
---

# Interview sliding window
## Problem
[Leetcode 3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

### Solution
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        int max=0;
        int left=0, right=0;
        while(right<s.length()){
            char c = s.charAt(right);
            if (set.contains(c)){
                max = Math.max(max, right-left);
            }
            while(set.contains(c)){
                set.remove(s.charAt(left));
                left++;
            }
            set.add(c);
            right++;
        }
        return Math.max(max, right-left);
    }
}
```
