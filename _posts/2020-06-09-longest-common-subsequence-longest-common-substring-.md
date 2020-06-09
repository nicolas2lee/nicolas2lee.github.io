---
layout: post
title:  "Longest common subsequence & Longest common substring "
date:   2020-06-09 14:00:00 +0100
categories: lcs, dp, algorithm
---
# Longest common subsequence
## Problem description
[Leetcode lcs practice problem](https://leetcode.com/problems/longest-common-subsequence/)

The problem is given two string: text1, text2, should find the longest common subsequence.
For example: text1 = "abcde", text2 = "ace" 
The longest length is 3, "ace"
## Solution
It is a classical dynamic planning problem, the key is to find the optimal substructure.

### State transition equation
dp[i][j] represents the longest commons subsequence in the i-1 th postion of text1, j-1 th position of text2.
Here the index of each text begins at 0. 

    dp[i][j] = dp[i-1][j-1]+1           <= if text1[i-1] == text2[j-1]
    
    dp[i][j] = dp[i-1][j], dp[i][j-1]   <= if text1[i-1] != text2[j-1]
    
### Initialisation
if one of string is empty, then the longest commons subsequence should be 0, so

    dp[i][0] = 0
    dp[0][j] = 0
    
### Example of code
    public int longestCommonSubsequence(String text1, String text2) {
        int length1 = text1.length();
        int length2 = text2.length();
        int[][] dp = new int[length1+1][length2+1];
        //with int array, the default value will be 0
        for (int i=1; i<=length1; i++){
            for (int j=1; j<=length2;j++){
                if (text1.charAt(i-1) == text2.charAt(j-1)) dp[i][j] = dp[i-1][j-1]+1;
                else dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return dp[length1][length2];
    }

# Longest common substring
Longest common substring is quite like longest common subsequence problem, the solution is nearly the same.
State transition equation is slightly different:

    dp[i][j] = dp[i-1][j-1]+1           <= if text1[i-1] == text2[j-1]
    
    dp[i][j] = 0                        <= if text1[i-1] != text2[j-1]
    
And to get the result, you should find the max value of dp array
## Problem description
[Lintcode lcs practice problem](https://www.lintcode.com/problem/longest-common-substring/description)

## Solution
### Example of code
    public int longestCommonSubstring(String A, String B) {
        int lengtha = A.length(); 
        int lengthb = B.length(); 
        if (lengtha == 0 || lengthb ==0) return 0;
        int[][] dp = new int[lengtha+1][lengthb+1];
        int max =-1;
        for (int i=1; i<= lengtha; i++){
            for (int j=1; j<= lengthb;j++){
                dp[i][j] = A.charAt(i-1)==B.charAt(j-1) ? dp[i-1][j-1] +1 : 0;
                max=Math.max(max, dp[i][j]);
            }
        }
        return max;
    }
