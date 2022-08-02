---
title: 用java spring boot快速开发后端api
date: 2023-08-14
description: Interview jump game series, using back tracking, dp 
tag: interview, jump game, backtracking, dp, algorithm
author: nicolas2lee
---

# Jump game 

## Reachable the destination 
### Problem description
You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```
### Solution
#### Backtracking
* Time complexity: O(2^n)
* Space complexity

#### Backtracking with memorization
* Time complexity: O(n^2)
* Space complexity

#### DP
* Time complexity: O(n^2)
* Space complexity

#### Greedy
* Time complexity: O(n)
* Space complexity

## Min step to reach the destination 