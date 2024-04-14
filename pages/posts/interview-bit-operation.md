---
title: Interview bit operation
date: 2024-03-31
description: Interview bitwise operation
tag: interview, bit, bitwise operation, algorithm
author: nicolas2lee
---

# Interview bit operation
## Count bit for n 
### Problem
[Leetcode 191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/description/)

Given an integer n (n< pow(2, 31-1)) , count the number of 1s in the binary representation.
For example:
```
11 -> 1011(binary), has 3 1s
128 -> 10000000(binary), has 1 1s
```
### Solution
#### Solution 1 Loop and Flip
For number n, can be represented with max 32 bits, as n< pow(2, 31-1),
so we can loop 32 times, check if each bit is 1
```java
public int hammingWeight(int n) {
    int bits = 0;
    int mask = 1;
    for (int i = 0; i < 32; i++) {
        if ((n & mask) != 0) {
            bits++; 
        }
        mask <<= 1;
    }
    return bits;
}
```

#### Solution 2 Bit Manipulation
The trick is n & (n-1) flips the least-significant 1-bit in n to 0, 
so count the number of operation n & n-1.

For example binary representation of n is 110100, then
```
n       = 1 1 0 1 0 0
n-1     = 1 1 0 0 1 1
n & n-1 = 1 1 0 0 0 0
```
```java
public int hammingWeight(int n) {
    int sum = 0;
    while (n != 0) {
        sum++;
        n &= (n - 1);
    }
    return sum;
}
```
### Follow up 1 negative number
#### How to represent negative number
The first bit is sign bit:
* 0 -> positive
* 1-> negative

Then calculate the complement
Let's take an example, n = 12
```
in binary it's 00001100
for number -12
flip all bits of 12, it would be 11110011
then +1,          so it would be 11110100
```

## Count bit from 0 to n
### Problem
[Leetcode 338. Counting Bits](https://leetcode.com/problems/counting-bits/description)

Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), 
ans[i] is the number of 1's in the binary representation of i.
```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```
### Solution
In previous problem, we know how to count number of 1s.
This problem we can reuse previous state for each give number n.
For number n, the number of 1s is the same as n/2, except last bit, so the result is
P(x)=P(x/2)+(x mod 2)
```java
public class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n + 1];
        for (int x = 1; x <= n; ++x) {
            // x / 2 is x >> 1 and x % 2 is x & 1
            ans[x] = ans[x >> 1] + (x & 1); 
        }
        return ans;
    }
}
```