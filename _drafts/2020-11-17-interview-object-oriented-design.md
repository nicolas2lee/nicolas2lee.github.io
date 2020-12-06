---
layout: post
title:  "Interview preparation object oriented design"
date:   2020-11-17 20:00:00 +0100
categories: Interview, object oriented design
---
# Questions & solutions
* [LRU](https://leetcode.com/problems/lru-cache/)
    
    
    class LRUCache {
        Map<Integer, Integer> map;
        Deque<Integer> deque;
        int capacity;
    
        public LRUCache(int capacity) {
            this.capacity = capacity;
            this.map = new HashMap<>();
            this.deque = new LinkedList<>();
        }
    
        public int get(int key) {
            if (!map.containsKey(key)) return -1;
            final Integer val = map.get(key);
            deque.remove(key);
            deque.offer(key);
            //System.out.println(deque);
            return val;
        }
    
        public void put(int key, int value) {
            if (!map.containsKey(key)){
                if (deque.size()==capacity){
                    int toRemoveKey = deque.poll();
                    map.remove(toRemoveKey);
                    deque.offer(key);
                    map.put(key, value);
                }else {
                    map.put(key, value);
                    deque.offer(key);
                }
            }else{
                deque.remove(key);
                deque.offer(key);
                map.put(key, value);
            }
        }
    }
    
* [Design hit counter](https://medium.com/@thomaspoignant/algorithmic-design-a-hit-counter-4bc6400152a)