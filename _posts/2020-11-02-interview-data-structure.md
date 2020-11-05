---
layout: post
title:  "Interview preparation"
date:   2020-11-02 14:00:00 +0100
categories: Interview, Data structure
---
# Binary search
[Leetcode 704. Binary Search](https://leetcode.com/problems/binary-search/)
  
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
    
Or you can simple call api in jdk
* Array: 

    public int search(int[] nums, int target) {
        int index = Arrays.binarySearch(nums, target);
        return index <0 ? -1: index;
    }
  
* Collection:

    index = Collections.binarySearch(collection, target)
    
when element not found, a negative will be returned, -index-1 is the position to insert
# Sort
## Quick sort    
[Leetcode 912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
  
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
    
Or you can call jdk api
    
    Arrays.sort(array)

## Merge sort

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

# Graph
## BFS & DFS
### BFS     
## Union find set
## Minimum spanning tree
[1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

### Prim
1. Choose any point in the graph as the start point, and add into mstSet which is a set contains all chosen vertex
2. Find the edge with min weight who is reachable based on all vertexes in mstSet, then add to mstSet
3. Repeat 2 till mstSet contains n vertexes 

Example of implementation:

    class Solution {
        public int minCostConnectPoints(int[][] points) {
            int[][] graph = buildGraph(points);
            int n=points.length;
            boolean[] visited = new boolean[n];
            Set<Integer> mstSet = new HashSet<>();
            mstSet.add(0);
            visited[0] = true;
            int sum=0;
            while(mstSet.size()!=n){
                int minDis = Integer.MAX_VALUE;
                int minIndex = -1;
                for(int p: mstSet){ 
                    for (int i=0; i<n;i++){
                        if (visited[i]) continue;
                        int dd = graph[p][i];
                        //System.out.printf("p is %d, i is %d, dd is %d\n", p, i, dd);
                        if (minDis > dd) {
                            minDis = dd;
                            minIndex =i;
                        }
                    }   
                }
                mstSet.add(minIndex);
                visited[minIndex]=true;
                sum+=minDis;
                //System.out.printf("minIndex is %d, minDis is %d\n", minIndex, minDis);
            }
            return sum;            
        }
        
        int[][] buildGraph(int[][] points){
            int n=points.length;
            int[][] graph = new int[n][n];
            for(int i=0; i<n;i++){
                for(int j=0; j<n;j++){
                    graph[i][j] = calculateDistance(points[i], points[j]);
                }
            }
            return graph;
        }
        
        int calculateDistance(int[] p1, int[] p2){
            //System.out.printf("p1[0] %d p1[1] %d p2[0] %d p2[1] %d\n", p1[0], p1[1], p2[0], p2[1]);
            return Math.abs(p1[0]-p2[0])+Math.abs(p1[1]-p2[1]);
        }
    }
    
### Kruskal
1. Sort all edges
2. If 2 vertexes of the edge are not connected (here we use union find set to check if 2 vertexes are connected or not), then add the edge
3. Repeat 2 till there are n-1 (here n is the number of vertex) edges are added

Example of implementation:

    class Solution {
        static class Edge{
            int start;
            int end;
            int weight;
            public Edge(int start, int end, int weight){
                this.start= start;
                this.end = end;
                this.weight = weight;
            }
            
            public String toString(){
                return String.format("start is %d, end is %d, weight is %d", start, end, weight);
            }
        }
        
        public int minCostConnectPoints(int[][] points) {
            if (points.length<=1) return 0;
            List<Edge> edges = buildGraph(points);
            Collections.sort(edges, (o1, o2)-> o1.weight- o2.weight);
            //edges.forEach(System.out::println);
            int n=points.length;
            int sum =0;
            int[] father = new int[n];
            int[] rank = new int[n];
            Arrays.fill(rank, 1);
            for (int i=0; i<n; i++){
                father[i]=i;
            }
            Set<Edge> edgeSet = new HashSet<>();
            for(int i=0; i<edges.size(); i++){
                if (edgeSet.size()!=n-1){
                    Edge e = edges.get(i);
                    if (unions(e.start, e.end, father, rank)){
                        sum+=e.weight;
                        edgeSet.add(e);
                    }
                }else break;
            }
            return sum;
        }
        
        boolean unions(int x,int y, int[] father, int[] rank){
            int fx = find(x, father);
            int fy = find(y, father);
            if(fx==fy) return false;   
            else if(rank[fx] >= rank[fy]){   
                father[fy] = fx;
                rank[fx] += rank[fy];
            } else{
                father[fx] = fy;
                rank[fy] += rank[fx];
            }
            return true;
        }
        
        int find(int x, int[] father){
            if(x!=father[x]) father[x]=find(father[x], father);
            return father[x];
        }
    
        List<Edge> buildGraph(int[][] points){
            int n=points.length;
            List<Edge> edges = new ArrayList<>();
            for(int i=0; i<n;i++){
                for(int j=0; j<n;j++){
                    if (i!=j) edges.add(new Edge(i, j, calculateDistance(points[i], points[j])));
                }
            }
            return edges;
        }
    
        int calculateDistance(int[] p1, int[] p2){
            //System.out.printf("p1[0] %d p1[1] %d p2[0] %d p2[1] %d\n", p1[0], p1[1], p2[0], p2[1]);
            return Math.abs(p1[0]-p2[0])+Math.abs(p1[1]-p2[1]);
        }
    }
    
## 

# String
## Anagrams
* [HackerRank Strings: Making Anagrams](https://www.hackerrank.com/challenges/ctci-making-anagrams/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)
* [Leetcode 1347. Minimum Number of Steps to Make Two Strings Anagram](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
* [Leetcode 438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
   
# Reference
[Princeton lectures](https://algs4.cs.princeton.edu/lectures/)
