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
LRU (Least recently used)


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

## Cache high availability  
[Design the caching system](https://medium.com/@narengowda/designing-the-caching-system-e42c6938df6a)
### Consistent hashing 
[Consistent hashing](https://www.toptal.com/big-data/consistent-hashing) 
### Count-min sketch
# Database
## Relational database management system (RDBMS)
ACID + transactions
### How to scale
#### Master-slave replication
Clients do read or/and write operations in master node, and do read operation only in slave node.
If the master goes down, one of slaves should replace the master.
![Master-slave-replication](https://github.com/donnemartin/system-design-primer/blob/master/images/C9ioGtn.png)
#### Master-master replication
Clients can do read/write operations in both master nodes, then all nodes are either loosely consistent (violating ACID)
or have increased write latency due to synchronization
#### Sharding
#### Denormalization

[What Happens When You Type A URL In Your Browser And Press Enter](https://medium.com/@salmenzouari/what-happens-when-you-type-a-url-in-your-browser-and-press-enter-fc56f387b5e7)
# System design interview questions with solutions
* [Designing a URL Shortening service like TinyURL](https://www.educative.io/courses/grokking-the-system-design-interview/m2ygV4E81AR)
* [Design Pastebin.com (or Bit.ly)](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/pastebin/README.md)
## Design a chat system
[Chat system design](https://mp.weixin.qq.com/s?__biz=MzI1ODY0NjAwMA==&mid=2247483756&idx=1&sn=a8e3303bc573b1acaf9ef3862ef89bdd&chksm=ea044bf3dd73c2e5dcf2c10202c66d6143ec866205e9230f974fbc0b0be587926699230b6b18&scene=21#wechat_redirect)
[Chat system design plus](https://cloud.tencent.com/developer/article/1525567)


* [Build a real-time gaming leaderboard](https://aws.amazon.com/blogs/database/building-a-real-time-gaming-leaderboard-with-amazon-elasticache-for-redis/)

## Design a Video Streaming Service (To complete)
[Design youtube](https://systeminterview.com/design-youtube.php)
Fun facts of YouTube in 2020 
Total number of monthly active users: 2 billion
Number of videos watched per day: 5 billion
73% of US adults use YouTube.
50 million creators on YouTube
YouTube’s Ad revenue was $15.1 billion for the full year 2019, up 36% from 2018.
YouTube is responsible for 37% of all mobile internet traffic.
YouTube is available in 80 different languages.

## Design instagram
[Design instagram](https://www.educative.io/courses/grokking-the-system-design-interview/m2yDVZnQ8lG)
### Understand the problem and establish design scope (To complete)
like, comment
[Design like system1](https://juejin.cn/post/6844903722066116621)
[Design like system2](https://www.jianshu.com/p/a43c6d2f8bfb)

### Unique id generation
#### MongoDB generator 
The 12-byte ObjectId value consists of:
* a 4-byte timestamp value, representing the ObjectId’s creation, measured in seconds since the Unix epoch
* a 5-byte random value
* a 3-byte incrementing counter, initialized to a random value

#### Twitter's snowflake (generated by web service)
[Twitter's snowflake](https://github.com/twitter-archive/snowflake/)
#### Instagram (generated in db via pl/pgsql)
[Sharding & IDs at Instagram](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c)
Each of our IDs consists of:
41 bits for time in milliseconds (gives us 41 years of IDs with a custom epoch)
13 bits that represent the logical shard ID
10 bits that represent an auto-incrementing sequence, modulus 1024. This means we can generate 1024 IDs, per shard, per millisecond

[UUID vs DB sequence](https://www.zhihu.com/question/28703540)
## Design web crawler
### bloom filter to avoid url redundancy
[Web crawler](https://wizardforcel.gitbooks.io/gainlo-interview-guide/content/sd16.html)
[Web crawler design](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/web_crawler/README.md)

## Design typehead suggestion/auto complete system
* [Typehead suggestion design](https://www.youtube.com/watch?v=us0qySiUsGU&ab_channel=TusharRoy-CodingMadeSimple)


## Design yelp
## Design uber (an advanced version of design yelp)
[Design uber](https://learnsystemdesign.blogspot.com/p/design-uber-backend.html)
To solve
* [Parking system]()
* [Elevator]()
* [Job scheduling]()
* [Meeting scheduling]()
* [Poker or chess gaming]()
* [Taxi/hotel booking]()
* [Recommendation]()

# Reference
* [The System Design Primer](https://github.com/donnemartin/system-design-primer)
* [How to scale websocket](https://hackernoon.com/scaling-websockets-9a31497af051)
* [Top 10 System Design Interview Questions](https://systeminterview.com/top-10-system-design-interview-questions.php)
* [gainlo guide](https://wizardforcel.gitbooks.io/gainlo-interview-guide/content/sd21.html)
* [sharding key chose](https://blog.taozj.org/201811/data-intensive-episode2-sharding.html)