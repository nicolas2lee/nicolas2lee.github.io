---
layout: post
title:  "Interview preparation"
date:   2020-11-02 14:00:00 +0100
categories: Interview, Data structure
---
# Binary search
[Leetcode 704. Binary Search](https://leetcode.com/problems/binary-search/)
## Without jdk native lib, just be careful of edge case of mid
<details>
  <summary>show java code</summary>
  
    public int search(int[] nums, int target) {
        int left =0, right = nums.length-1, mid = -1;
        while (left<=right){
            mid = (left+right)/2;
            //System.out.printf("left is %d, mid is %d, right is %d\n", left, mid, right);
            int cur = nums[mid];
            if (cur == target) return mid;
            if (cur< target) left = mid+1;
            else right = mid-1;
        }
        return -1;
    }
    
</details>

## With jdk native lib:
* Array: 

  <details>
    <summary>show java code</summary>
  
    public int search(int[] nums, int target) {
        int index = Arrays.binarySearch(nums, target);
        return index <0 ? -1: index;
    }
    
  </details>
  
* Collection:

    index = Collections.binarySearch(collection, target)
    
when element not found, a negative will be returned, -index-1 is the position to insert
# Sort
## Quick sort    
[Leetcode 912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
## Without jdk native lib:
<details>
  <summary>show java code</summary>
  
    class Solution {
        public int[] sortArray(int[] nums) {
           quickSort(0, nums.length-1, nums);
            return nums;
        }
        
        void quickSort(int start, int end, int[] nums){
            if (start>end) return;
            int i=start, j=end, pivot=(nums[start]+nums[end])/2;
            while (i <= j){
                while(i<=j && nums[i]<pivot) i++;
                while(i<=j && nums[j]>pivot) j--;
                if (i<=j) {
                    int tmp=nums[i];
                    nums[i]=nums[j];
                    nums[j]=tmp;
                    i++; j--;
                } 
            }
            quickSort(start, j, nums);
            quickSort(i, end, nums);
        }
    }
    
</details>

## With jdk native lib:
    
    Arrays.sort(array)

## Merge sort
<details>
  <summary>show java code</summary>
    
    void  mergeSort(int[] nums, int l, int r) {
        if (l >= r) return;
        int m = l + ((r - l)/2);
        mergeSort(nums, l, m);
        mergeSort(nums, m + 1, r);
        merge(nums, l, m, r);
    }
    
    void merge(int[] nums, int l, int m, int r) {
        int i = l, j = m + 1, p = 0;
        int[] temp = new int[r - l + 1];
        while (i <= m && j <= r) {
            if (nums[i] > nums[j]) temp[p++] = nums[j++];
            else temp[p++] = nums[i++]; 
        }
        while (i <= m) temp[p++] = nums[i++];
        while (j <= r) temp[p++] = nums[j++];    
        for (int x = 0 ; x < r - l + 1; x++) {
            nums[x + l] = temp[x];
        }
    }

</details>

# Graph
## BFS & DFS
### BFS        
# Reference
[Princeton lectures](https://algs4.cs.princeton.edu/lectures/)
