---
title: Interview permutations
date: 2024-03-04
description: Interview permutations
tag: interview, permutation, backtracking, algorithm
author: nicolas2lee
---

# Interview permutations
## permutations
### Permutations I (without duplicates)

#### Problem
[46. Permutations](https://leetcode.com/problems/permutations/description/)

#### Solution - Backtracking

<details>
  <summary>Show code</summary>

```java
class Solution {
    int[] nums;
    List<List<Integer>> ans;
    public List<List<Integer>> permute(int[] nums) {
        this.nums = nums;
        ans = new ArrayList<>();
        backtracking(new ArrayList<>());
        return ans;
    }

    void backtracking(List<Integer> curList){
        if (curList.size()==nums.length){
            ans.add(new ArrayList<>(curList));
            return;
        }
        for(int i=0; i<nums.length; i++){
            if (!curList.contains(nums[i])){
                curList.add(nums[i]);
                backtracking(new ArrayList<>(curList));
                curList.remove(curList.size()-1);
            }
        }
    }
}
```

</details>




### Permutations II (with duplicates)
#### Problem: [Leetcode 47. Permutations II](https://leetcode.com/problems/permutations-ii/description/)

Solution:
The same base as the Permutations I problem, use backtracking, for each position, you can only pick the unique number, let's take an example of [1, 1, 2]:
At index 0, you can pick 1, 2
At index 1, if you picked 2 in the previous round, then you can only pick 1
if you picked 1, then you can still pick 1, 2

So we need a hashmap to store the unique number with it's count, when the count is 0, then you can not pick the number

### Next Permutations 

## Subsets (without duplicates)
Problem: [Leetcode 78. Subsets](https://leetcode.com/problems/subsets/description/)

Solution:
Iterate from index 0 to n of nums, for each number, we can either pick it, or not pick it, then backtrack on it

## Subsets II (with duplicates)
1. We need to sort all numbers
2. The same as previous problem subsets, backtrakcing for every number, either pick it or not pick it. The difference is when the current number is the same as previous number, after the 1st time it was added, then we just skip it

## Combination sum
### Combination sum III
Problem: [Leetcode 216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

Solution:
Backtracking, for each number, we can either pick or not pick

### Combination sum II
Problem: [Leetcode 40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

Solution:
Backtracking, same as previous problem Combination sum III, but it may contain duplicates, so sort before backtracking, for duplicates only iterate the 1st one

### Combination sum 
Problem: [Leetcode 39. Combination Sum](https://leetcode.com/problems/combination-sum/description/)

Solution:
Backtracking, same as previous problem Combination sum III, but it may contain duplicates, so sort before backtracking, for duplicates only iterate the 1st one

## Related problem
generating subsets
combination sum
suduko solver
