# Distributed Cache

## What is a Distributed Cache?

A distributed cache is a cache that is spread across multiple machines (nodes) and acts as a single logical cache.

Instead of storing cached data on a single server, data is partitioned and distributed across multiple cache nodes.

Examples:
- Redis Cluster
- Memcached Cluster

---

## Why Use a Distributed Cache?

### Scalability
- A single machine has limited memory.
- Multiple nodes provide larger aggregate memory.

### High Availability
- Replicas can serve requests if a node fails.

### Reduced Database Load
- Frequently accessed data is served from memory.

### Lower Latency
- Accessing memory is much faster than querying a database.

---

## Architecture

                    Applications
                          |
            -----------------------------
            |            |             |
         Service A   Service B    Service C
                          |
                    Cache Cluster
            --------------------------------
            |              |              |
          Node 1         Node 2         Node 3

---

## Core Concepts

### 1. Partitioning (Sharding)

Distribute keys across cache nodes.

Example:

user:1 -> Node A
user:2 -> Node B
user:3 -> Node C

Common approach:
- Consistent Hashing

Benefits:
- Easy horizontal scaling
- Minimal data movement when nodes are added/removed

---

### 2. Replication

Store copies of data on multiple nodes.

Example:

Primary Node
     |
     +---- Replica

Benefits:
- Fault tolerance
- High availability
- Faster failover

---

### 3. Cache Eviction

Memory is limited.

Common eviction policies:

#### LRU (Least Recently Used)
Remove least recently accessed item.

#### LFU (Least Frequently Used)
Remove least frequently accessed item.

#### TTL (Time To Live)
Automatically remove expired entries.

---

### 4. Cache Consistency

Keep cache synchronized with database.

#### Cache Aside (Most Common)

Read:
1. Check cache
2. Cache miss
3. Read DB
4. Update cache

Write:
1. Update DB
2. Invalidate cache

Pros:
- Simple
- Widely used

---

#### Write Through

1. Write cache
2. Cache writes DB

Pros:
- Strong consistency

Cons:
- Higher write latency

---

#### Write Behind

1. Write cache
2. Async write to DB

Pros:
- Fast writes

Cons:
- Risk of data loss

---

### 5. Cache Hit & Cache Miss

#### Cache Hit
Data found in cache.

#### Cache Miss
Data not found in cache, requiring DB access.

Goal:
- Maximize Cache Hit Ratio

Formula:

Hit Ratio = Cache Hits / Total Requests

---

### 6. Hot Keys

Problem:
A single key receives huge traffic.

Example:

celebrity:profile

Issues:
- One node becomes overloaded.

Solutions:
- Replication
- Local cache
- Key sharding

---

### 7. Failover

If a cache node crashes:

1. Detect failure
2. Promote replica
3. Redirect traffic

Goal:
- Minimize downtime

---

## Challenges

### Cache Invalidation
Keeping cache synchronized with DB.

### Cache Stampede
Many requests hit DB simultaneously after cache expiration.

Solutions:
- Request coalescing
- Staggered TTLs
- Distributed locking

### Data Skew
Some nodes receive much more traffic than others.

Solutions:
- Better hashing
- Rebalancing

---

## Interview Talking Points

When asked to design a distributed cache:

1. Data partitioning (Consistent Hashing)
2. Replication strategy
3. Cache eviction policy
4. Consistency model
5. High availability
6. Failover handling
7. Hot key mitigation
8. Scaling strategy
9. Monitoring and metrics

---

## Key Terms

- Cache Hit
- Cache Miss
- TTL
- LRU
- LFU
- Sharding
- Consistent Hashing
- Replication
- Failover
- Cache Aside
- Write Through
- Write Behind
- Cache Stampede
- Hot Key

---

## One-Line Definition

A distributed cache is a cluster of cache nodes that stores data in memory across multiple machines to provide low-latency, scalable, and highly available access to frequently used data.