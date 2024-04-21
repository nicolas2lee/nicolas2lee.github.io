---
title: Interview priority queue
date: 2024-04-21
description: Interview priority queue
tag: interview, algorithm, priority queue
author: nicolas2lee
---

# Interview priority queue
Priority queue is a basic data structure which is very powerful. There are some problems could be solved by using priority queue.

## TopK
### Problem
[Leetcode 347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)
### Solution
The solution is easy and straightforward, so won't discuss here

## Meeting room
### Problem
[Leetcode 253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/description/)

The problme is giving the start and end time of meetings, find the min number of meeting rooms to hold all meetings without any conflicts
Examples:
```
[[0,30],[5,10],[15,20]] -> 2

[[7,10],[2,4]] -> 1
```

### Solution
Sort the meetings by the start time. And then use the priority queue to store the endtime of each meeting room. 
If the new meeting start time >= meeting room end time, then we can reuse this meeting room and update the end time of meeting room
If the new meeting start time < meeting room end time, add the end of meeting time into priority queue as a new meeting room

So the size of priority queue would be the min meeting room.

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        Arrays.sort(intervals, (o1, o2)-> o1[0] - o2[0]);
        var pq = new PriorityQueue<Integer>();
        pq.add(intervals[0][1]);
        for(int i=1; i<intervals.length; i++){
            var cur = intervals[i];
            if (cur[0]>=pq.peek()){
                pq.poll();
                pq.add(cur[1]);
            }else{
                pq.add(cur[1]);
            }
        }
        return pq.size();
    }
}
```
