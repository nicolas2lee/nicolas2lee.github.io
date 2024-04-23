---
title: Interview binary search
date: 2024-04-23
description: Interview binary search
tag: interview, algorithm, binary search
author: nicolas2lee
---

# Interview binary search
## Binary search template
```java
  public int binarySearch(int[] a, int fromIndex, int toIndex, int key) {
        int low = fromIndex;
        int high = toIndex - 1;
        while (low <= high) {
            int mid = (low + high) >>> 1;
            int midVal = a[mid];

            if (midVal < key)
                low = mid + 1;
            else if (midVal > key)
                high = mid - 1;
            else
                return mid; // key found
        }
        return -(low + 1);  // key not found.
    }
```

### Problem
[Leetcode 719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/description/)

### Solution
The pair distance is within [0, max(nums) - min(nums)]. Binary search value target with left = 0, right = max(nums) - min(nums) with the count of nums
```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int left = 0, right = nums[n-1]-nums[0];
        while(left<=right){
            int mid = (right-left)/2+left;
            int cnt = count(nums, mid);
            System.out.println("["+left+","+right+"] cnt: "+cnt);
            if (cnt>=k){
                right = mid-1;
            } else {
                left = mid+1;
            }
        }
        return left;
    }

    public int count(int[] nums, int target){
        int n = nums.length;
        int cnt = 0;
        
        for(int j=0; j<n; j++){
            int i=0;
            while(nums[j] - nums[i]>target) i++;
            cnt+=j-i;
        }
        return cnt;
    }
}
```
