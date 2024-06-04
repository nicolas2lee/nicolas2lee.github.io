---
title: Interview dp
date: 2024-06-04
description: Interview dp
tag: interview, dp, algorithm
author: nicolas2lee
---

# Interview dp
## Paint house
### [256 Paint hourse](https://leetcode.com/problems/paint-house/description/)
There is a row of n houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x 3 cost matrix costs.

For example, costs[0][0] is the cost of painting house 0 with the color red; costs[1][2] is the cost of painting house 1 with color green, and so on...
Return the minimum cost to paint all houses.

Example
```
Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.
```
### Solution
#### Solution dp
For each position of house, need to choose another color of previous position house

State transfer formula 
```
dp[i][0] = Math.min(dp[i-1][1], dp[i-1][2])+costs[i-1][0];
dp[i][1] = Math.min(dp[i-1][0], dp[i-1][2])+costs[i-1][1];
dp[i][2] = Math.min(dp[i-1][0], dp[i-1][1])+costs[i-1][2];
```

<details>
  <summary>Show code</summary>
  
```java
class Solution {
    public int minCost(int[][] costs) {
        int n=costs.length;
        int[][] dp = new int[n+1][3];
        for(int i=1; i<=n; i++){
            dp[i][0] = Math.min(dp[i-1][1], dp[i-1][2])+costs[i-1][0];
            dp[i][1] = Math.min(dp[i-1][0], dp[i-1][2])+costs[i-1][1];
            dp[i][2] = Math.min(dp[i-1][0], dp[i-1][1])+costs[i-1][2];
        }
        return Math.min(dp[n][0], Math.min(dp[n][1], dp[n][2]));
    }
}
```
</details>

#### Solution dp with space optimization
Space optimization, no need to use a 2d arry to store all states, just need to store last 2 rows

<details>
  <summary>Show code</summary>
  
```java
class Solution {
    public int minCost(int[][] costs) {

        if (costs.length == 0) return 0;

        int[] previousRow = costs[costs.length -1];

        for (int n = costs.length - 2; n >= 0; n--) {

            int[] currentRow = costs[n].clone();
            // Total cost of painting the nth house red.
            currentRow[0] += Math.min(previousRow[1], previousRow[2]);
            // Total cost of painting the nth house green.
            currentRow[1] += Math.min(previousRow[0], previousRow[2]);
            // Total cost of painting the nth house blue.
            currentRow[2] += Math.min(previousRow[0], previousRow[1]);
            previousRow = currentRow;
        }  

        return Math.min(Math.min(previousRow[0], previousRow[1]), previousRow[2]);
    }
}
```
</details>

### [265. Paint House II](https://leetcode.com/problems/paint-house-ii/description/)
Same as previous problme, instead of having 3 colors, this problem has k colors
