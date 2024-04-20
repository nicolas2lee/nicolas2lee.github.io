---
title: Interview sliding window
date: 2024-04-18
description: Interview sliding window
tag: interview, algorithm, sliding window
author: nicolas2lee
---

# Interview sliding window
### Problem
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


### Problem
[Leetcode 2302. Count Subarrays With Score Less Than K](https://leetcode.com/problems/count-subarrays-with-score-less-than-k/description/)

### Solution
It's evident to come up with the brute force solution, check every sub array sum, then it will be O(n*n). So the solution is to use slide window, every sub array in the slide window
will staisfy the condition

```java
class Solution {
    public long countSubarrays(int[] nums, long k) {
        long ans = 0L, sum = 0L;
        for (int left = 0, right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum * (right - left + 1) >= k)
                sum -= nums[left++];
            ans += right - left + 1;
        }
        return ans;
    }
}
```
