---
layout: post
title:  "Interview preparation system design"
date:   2020-11-15 14:00:00 +0100
categories: Interview, system design
---
# Hash
# Cache
## Cache Invalidation
### Write-through cache
Write into the cache & database at the same time.
Advantage: complete data consistency, no data loss even when external system (e.g. database) down
Disadvantage: high latency for write operation 
### Write-around cache
Write into the database directly, when read data from the cache, if not exist then retrieval from the database & put into the cache
Advantage: low latency for write operation (solve the problem of write-through)
Disadvantage: high latency for first read 
### Write-back cache
Write into the cache and confirm back to client, then write into the database.
Advantage: low latency
Disadvantage: data loss, inconsistency
## Cache eviction policies
* LRU (Least recently used)


    private Map<Integer, Integer> map = new LinkedHashMap<Integer, Integer>();
    private int capacity;
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
    }
    
    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        int val = map.get(key);
        set(key, val);
        return val;
    }
    
    public void set(int key, int value) {
        //if not contains and reach the capacity, then remove the 1st inserted element
        if (!map.containsKey(key) && (map.size() == capacity)) 
            map.remove(map.keySet().iterator().next());
        // if contains key, remove element
        else map.remove(key);
        map.put(key, value);
    }}
 
# Database
# System design interview questions with solutions
* [Designing a URL Shortening service like TinyURL](https://www.educative.io/courses/grokking-the-system-design-interview/m2ygV4E81AR)
# Reference
* [The System Design Primer](https://github.com/donnemartin/system-design-primer)