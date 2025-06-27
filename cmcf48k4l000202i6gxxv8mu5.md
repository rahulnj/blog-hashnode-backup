---
title: "Range-Based Sharding: Ordered But Uneven"
seoTitle: "Range-Based Sharding: Simple Scaling for Time-Series Data"
seoDescription: "Sharding by range for fast queries and ordered workloads. Learn pros, cons, and real use cases."
datePublished: Fri Jun 27 2025 17:58:28 GMT+0000 (Coordinated Universal Time)
cuid: cmcf48k4l000202i6gxxv8mu5
slug: range-based-sharding
tags: sharding, databases, system-design, sharding-techniques

---

**Scaling Smart With Sorted Keys (and Hidden Pitfalls)**

## 📌 Overview

Range-based sharding is one of the simplest and most intuitive ways to split data across servers — especially when your data has a natural order like timestamps, IDs, or numerical values. It’s a go-to strategy for systems that rely heavily on time-based queries or sorted ranges, such as logs, audit trails, or reporting systems.

But like all good things, it comes with trade-offs.

---

## 🧠 What Is Range-Based Sharding?

In range-based sharding, you:

1. Choose a **sharding key** (like `user_id`, `created_at`, or `order_total`)
    
2. Define **value ranges**
    
3. Route each record to a shard based on where its value falls in those ranges
    

**Example:**

* IDs 1–10,000 → Shard 1
    
* IDs 10,001–20,000 → Shard 2
    
* IDs 20,001–30,000 → Shard 3
    

Each insert checks which range it belongs to and saves the record in that shard.

---

## 🔁 Real-Life Analogy

Imagine a school splitting students into exam halls by last name:

* A–F → Hall 1
    
* G–L → Hall 2
    
* M–Z → Hall 3
    

Everything works well — unless 70% of students have the same surname. Suddenly, one hall becomes overcrowded while the others are mostly empty.

That’s the biggest risk in range-based sharding: **data skew**.

## ✅ Benefits of Range-Based Sharding

### 1\. Great for Range Queries

Range-based sharding is excellent for queries like:

```javascript
SELECT * FROM logs
WHERE timestamp BETWEEN '2024-01-01' AND '2024-01-31'
```

Since data is stored in sorted order, the system knows exactly which shard(s) to check.

---

### 2\. Predictable Distribution

You always know where to look for data. It’s clean and organized — ideal for analytical or time-based systems.

---

### 3\. Simple to Implement

No hash functions or modulo logic. Just define ranges and match values.

---

## 🚫 Limitations of Range-Based Sharding

### 1\. Hotspot Risk

If new data always falls into the highest range (e.g., latest timestamp), that shard becomes a **write hotspot**. It receives more load, while other shards sit idle.

### 2\. Manual Range Management

Without automation, you’ll need to:

* Monitor usage patterns
    
* Add new ranges
    
* Migrate old data  
    This can lead to operational overhead.
    

### 3\. Skewed Traffic

If one user or customer contributes most of the data (e.g., a top e-commerce seller), and you’re sharding by `customer_id`, their shard can become overwhelmed.

---

## 🏗 Use Case: Logging Systems

Time-series systems like **Prometheus**, **ELK**, and **InfluxDB** often use range-based sharding. Data is naturally ordered by time, and queries often request ranges.

However, they also use:

* **Shard rotation**
    
* **Retention policies (TTL)**
    
* **Cold storage**  
    …to prevent write hotspots and overgrowth.
    

---

## When to Use Range-Based Sharding

Use it when:

* You mostly run **range-based queries**
    
* You’re working with **time-series or ordered data**
    
* Your load is predictable or split by time/customer/location
    

Avoid it if:

* Your data input is bursty or skewed
    
* You expect **unpredictable growth**
    
* You want **auto-scaling** or elastic architecture
    

---

## 📊 Summary

**Pros:**

* Great for sorted or time-based data
    
* Easy range queries
    
* Simple logic
    

**Cons:**

* High chance of imbalance
    
* Manual scaling required
    
* Not ideal for bursty or random workloads
    

---

## 🧭 Coming Up Next

In the next post, we’ll dive into **Consistent Hashing** — the smarter, scalable way to avoid rebalancing chaos and evenly distribute load, even as you grow.