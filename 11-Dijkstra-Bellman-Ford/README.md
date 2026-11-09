
# 🛣️ Shortest Path Algorithms - Dijkstra & Bellman-Ford

<div align="center">

![Shortest Path Banner](images/shortest-path-banner.png)

**Complete Guide to Path Finding Algorithms**

[![Concepts](https://img.shields.io/badge/Concepts-2_Algorithms-green?style=for-the-badge)](.)
[![Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)](.)

[📖 Overview](#-overview) • [🔄 Relaxation](#-relaxation-concept) • [🎯 Dijkstra](#-dijkstra-algorithm) • [⚡ Bellman-Ford](#-bellman-ford-algorithm) • [📊 Comparison](#-comparison)

</div>

---

## 📖 Overview

**Shortest Path Algorithms** are used to find the optimal path between nodes in a network.

### Key Algorithms:

| Algorithm | Best For | Negative Weights | Speed |
|-----------|----------|------------------|-------|
| **Dijkstra** | Positive weights only | ❌ No | Fast |
| **Bellman-Ford** | Any weights, negative cycle detection | ✅ Yes | Slow |

---

## 📚 Table of Contents

- [🔄 Relaxation Concept](#-relaxation-concept)
- [🎯 Dijkstra Algorithm](#-dijkstra-algorithm)
  - [How It Works](#how-it-works)
  - [Example](#example)
  - [Why It Fails on Negative Weights](#why-dijkstra-fails-on-negative-weights)
- [⚡ Bellman-Ford Algorithm](#-bellman-ford-algorithm)
  - [How It Works](#how-it-works-1)
  - [Example](#example-1)
  - [Why V-1 Iterations](#why-v-1-iterations)
- [📊 Comparison](#-comparison)

---

## 🔄 Relaxation Concept

### Main Crux

**"Did I find a better/shorter path?"**

If yes, then relax the old route (abandon it) and adopt the new one!

---

### Real Life Example

**Going to Office Scenario:**

```
Initially: 
You think office is 30 minutes away (Route A)

Later:
Friend tells you about a shortcut - only 20 minutes (Route B)

Relaxation = Abandoning old route and taking the new one! ✅
```

---

### Technical Definition

**Formula:**
```
If: distance[source → u] + weight(u → v) < distance[source → v]

Then: distance[v] = distance[source → u] + weight(u → v)
      (Relax - update it!)
```

**Simple Explanation:**
```
u = Where you're coming from
v = Where you're going  
weight(u→v) = Direct distance from u to v

Check: If going through u to reach v is a shorter path, update it!
```

---

### Visual Example

**Before Relaxation:**
```
A → C (direct) = 10

Current distance to C = 10
```

**After Relaxation:**
```
A → B → C = 3 + 4 = 7

7 < 10, so relax!
New distance to C = 7 ✅
```

---

### How Different Algorithms Use Relaxation

#### 1. Dijkstra:
```
- Relaxes each node once
- Greedy approach - selects minimum distance node
```

#### 2. Bellman-Ford:
```
- Relaxes all edges (V-1) times
- Repetition for guarantee
```

#### 3. DUAL (EIGRP):
```
- Relaxes for feasible successors
- With loop prevention
```

---

## 🎯 Dijkstra Algorithm

### What is Dijkstra?

This algorithm is used to find the **shortest path**.

**Key Feature:** Visits each node **once** and finalizes the shortest distance.

---

### How It Works

**Algorithm Process:**

```
Step 1: Select start node (source)

Step 2: Create visited and unvisited sets

Step 3: Assign infinite distance to all nodes, source gets 0

Step 4: Check neighbors from current node and calculate their distances

Step 5: Update if shorter distance found (relaxation)

Step 6: Mark current node as visited

Step 7: Select next minimum distance unvisited node

Step 8: Repeat until all nodes are visited
```

---

### Visited vs Unvisited

**Purpose:**

```
1. To avoid duplicate work:
   - If a node is visited, its shortest distance is FINAL
   - No need to check again

2. Efficiency:
   - Focus only on unvisited nodes

3. Termination:
   - When all nodes are visited, algorithm ends
```

**Easy Way:**
```
Unvisited = Where we still need to go ✗
Visited = Where we've already been ✓
```

---

### Example

**Network:**
```
A ---5--- B
|         |
2         3
|         |
C ---1--- D
```

**Shortest path from A to D:**

**Step-by-Step:**

```
Initialize:
A = 0, B = ∞, C = ∞, D = ∞
Unvisited = {A, B, C, D}

Step 1: Visit A (minimum = 0)
- Relax A→B: B = 5
- Relax A→C: C = 2
- Visited = {A}

Step 2: Visit C (minimum = 2)
- Relax C→D: D = 2+1 = 3
- Visited = {A, C}

Step 3: Visit D (minimum = 3)
- Relax D→B: 3+3 = 6 (not better than 5)
- Visited = {A, C, D}

Step 4: Visit B (minimum = 5)
- Done!
- Visited = {A, C, D, B}
```

**Result:**
```
A → C → D
Distance = 2 + 1 = 3 ✅ (Shortest path!)
```

---

### Benefits and Drawbacks

**Benefits:**
```
✅ Guaranteed shortest path
✅ Loop-free routing
✅ Fast convergence
✅ Efficient for positive weights
```

**Drawbacks:**
```
❌ Requires more memory (must store entire network map)
❌ CPU intensive calculations
❌ Fails on negative weights
```

---

## Why Dijkstra Fails on Negative Weights

### Core Problem

**Dijkstra's Assumption:**

```
"Once we find the shortest distance to a node, it's FINAL"
```

**Negative weights break this assumption!** ❌

---

### Simple Example (Works)

**Network:**
```
A --2--> B --(-5)--> C
A --6--------------> C
```

**Dijkstra's Process:**

```
Step 1: Initialize
A = 0, B = ∞, C = ∞

Step 2: Process node A (current=A)
- Relax A→B: B = 2 ✓
- Relax A→C: C = 6 ✓
- Mark A as visited

Step 3: Minimum unvisited = B (distance=2)
- Relax B→C: 2+(-5) = -3
- Update C: C = -3 (better path!)
- Mark B as visited

Step 4: Process C
- C = -3 (Final answer by Dijkstra)
```

**Result:** A → B → C = 2 + (-5) = **-3** ✅

Wait... Dijkstra gave the correct answer!

**Yes, in this example it's correct.** The problem occurs in a different situation:

---

### Problem Example (Fails)

**Network:**
```
A --1--> B
A --4--> C
B --2--> C
C --(-5)--> B
```

**Dijkstra's Process:**

```
Step 1: Start A
A = 0, B = ∞, C = ∞

Step 2: Process A
- B = 1
- C = 4
- A visited ✓

Step 3: Minimum = B (distance=1)
- Relax B→C: 1+2=3 < 4
- C = 3
- Mark B as visited ✓ (PROBLEM HERE!)

Step 4: Process C (distance=3)
- Relax C→B: 3+(-5) = -2
- But B is already visited! ❌
- Dijkstra won't update
```

**Dijkstra's Answer:** B = 1 ❌

**Correct Answer:** 
```
A → C → B
= 4 + (-5) 
= -1 ✅ (Better than direct A→B = 1)
```

**Dijkstra missed it because B was already marked as visited!**

---

### Real Problem Explained

**Dijkstra's Logic:**
```
"Whichever node is found at minimum distance,
 its distance is FINAL"
```

**This is wrong with negative weights because:**
```
- Later, a negative edge might be found that:
  - Further reduces already visited node's distance
  - But Dijkstra won't check again ❌
```

---

### Technical Reason - Greedy Choice Property Fails

**Dijkstra is Greedy:**
```
Select what seems best at the current moment
```

**Positive Weights: This works ✅**
```
- Going forward will only increase distance
- No need to look back
- Once visited = FINAL
```

**Negative Weights: Greedy fails ❌**
```
- Going forward can DECREASE distance
- Already visited nodes may need revision
- Once visited ≠ FINAL
```

---

## ⚡ Bellman-Ford Algorithm

### What is Bellman-Ford?

**Core Idea:** Relax all edges **(V-1) times** - guarantees correct answer, **even with negative weights!** ✅

---

### How It Works

**Algorithm Process:**

```
Step 1: Initialize
- Source = 0
- All others = ∞

Step 2: Relax (V-1) Times
- Check all edges
- Update if better path found
- Repeat this process (V-1) times

Step 3: Negative Cycle Check
- Relax one more time
- If any edge still relaxes, negative cycle exists!
```

---

### Example

**Network:**
```
A --1--> B --(-3)--> C
A --5--------------> C
```

**Nodes = 3, so V-1 = 2 iterations**

**Process:**

```
Initialize:
A = 0, B = ∞, C = ∞

Iteration 1:
- Edge A→B: B = 0+1 = 1
- Edge A→C: C = 0+5 = 5
- Edge B→C: C = 1+(-3) = -2 (better than 5!)
  Update: C = -2

Iteration 2:
- Edge A→B: 0+1 = 1 (no change)
- Edge A→C: 0+5 = 5 (worse than -2)
- Edge B→C: 1+(-3) = -2 (no change)
  
No changes in iteration 2 = Done!

Negative Cycle Check (Iteration 3):
- No edge relaxed
- No negative cycle ✅
```

**Answer:** A to C = **-2** ✅

---

### Why V-1 Times?

**Mathematical Reason:**

```
Longest simple path (without loop) = V-1 edges

Example:
4 nodes graph
A → B → C → D
Maximum edges = 3 = (4-1) = V-1
```

**Logic:**

```
Iteration 1: At least 1 node gets correct distance
Iteration 2: At least 1 more node correct
...
Iteration V-1: All nodes correct! 🎯

Each iteration finalizes at least one node
V-1 iterations = guarantee all nodes correct
```

---

### Negative Cycle Detection

**What is a Negative Cycle?**

```
A cycle whose total weight is negative

Example:
A → B → C → A
Weights: 5 + (-3) + (-4) = -2 (negative!)

Problem: Infinite loop - distance keeps decreasing
```

**How Bellman-Ford Detects:**

```
After V-1 iterations:
- All distances should be final

If in V-th iteration any edge still relaxes:
- Negative cycle exists! ⚠️
- Distance can decrease to infinity
```

---

## 📊 Comparison

### Dijkstra vs Bellman-Ford

<div align="center">

| Feature | Dijkstra | Bellman-Ford |
|---------|----------|--------------|
| **Iterations** | Once per node | V-1 times all edges |
| **Negative Weights** | ❌ Fails | ✅ Works |
| **Negative Cycle Detection** | ❌ No | ✅ Yes |
| **Speed** | Fast (O(V²) or O(E log V)) | Slow (O(V×E)) |
| **Memory** | More (priority queue) | Less (simple arrays) |
| **Approach** | Greedy | Dynamic Programming |
| **Visited/Unvisited** | ✅ Yes | ❌ No |
| **Use Case** | Positive weights only | Any weights |
| **Real Protocol** | OSPF, IS-IS | RIP, BGP |

</div>

---

### When to Use What?

**Use Dijkstra When:**
```
✅ All edge weights are positive
✅ Need fastest algorithm
✅ OSPF routing protocol
✅ GPS navigation (distances always positive)
```

**Use Bellman-Ford When:**
```
✅ Negative weights present
✅ Need to detect negative cycles
✅ RIP routing protocol
✅ Network with varying costs (profit/loss)
```

---

### Visual Comparison

**Dijkstra Approach:**
```
Start → Pick minimum → Mark visited → Never revisit
  ↓
Fast but can't handle negative weights
```

**Bellman-Ford Approach:**
```
Repeatedly relax ALL edges V-1 times
  ↓
Slow but handles everything correctly
```

---

## 📝 Quick Reference

### Algorithm Summaries

#### Dijkstra Template:
```
1. Initialize: source=0, others=∞
2. Create unvisited set
3. While unvisited not empty:
   a. Pick minimum distance node
   b. Mark as visited
   c. Relax all neighbors
   d. Update distances
4. Done - shortest paths found!
```

#### Bellman-Ford Template:
```
1. Initialize: source=0, others=∞
2. Repeat V-1 times:
   a. For each edge (u,v):
      If distance[u] + weight(u,v) < distance[v]:
         distance[v] = distance[u] + weight(u,v)
3. Check V-th iteration:
   - If any edge relaxes → Negative cycle!
4. Done - shortest paths found!
```

---

### Key Takeaways

**Relaxation:**
```
✅ Core concept for both algorithms
✅ "Did I find a better path?" - check it
✅ Update if better, ignore if worse
```

**Dijkstra:**
```
✅ Fast and efficient
✅ Positive weights only
✅ Greedy approach
✅ One-time visit per node
```

**Bellman-Ford:**
```
✅ Handles negative weights
✅ Detects negative cycles
✅ Slower but more robust
✅ Repeatedly relaxes all edges
```

---


---

## 🎓 What I Learned

✅ **Relaxation Concept** - Core of shortest path algorithms  
✅ **Dijkstra Algorithm** - Fast greedy approach for positive weights  
✅ **Visited/Unvisited Sets** - Efficiency in Dijkstra  
✅ **Negative Weight Problem** - Why Dijkstra fails  
✅ **Bellman-Ford Algorithm** - Handles negative weights correctly  
✅ **V-1 Iterations** - Mathematical guarantee of correctness  
✅ **Negative Cycle Detection** - Important network property  
✅ **Algorithm Selection** - When to use which algorithm  

---

## 🚀 How to Use This Repository

1. **Clone the repository:**
   ```bash
   git clone https://github.com/abdul-wahid022/Shortest-Path-Algorithms.git
   ```

2. **Study the concepts:**
   - Understand relaxation first
   - Learn Dijkstra for positive weights
   - Master Bellman-Ford for all cases

3. **Practice examples:**
   - Work through provided examples
   - Create your own test cases
   - Compare algorithm outputs

4. **Apply to routing:**
   - Understand OSPF (uses Dijkstra)
   - Understand RIP (uses Bellman-Ford)
   - Know when to use which protocol

---

## 📞 Connect With Me

<div align="center">

[![Email](https://img.shields.io/badge/Email-a.wahid7860668%40gmail.com-red?style=for-the-badge&logo=gmail)](mailto:abdulwahid.infosec@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Abdul_Wahid-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/abdul-wahid022)
[![GitHub](https://img.shields.io/badge/GitHub-abdul--wahid022-black?style=for-the-badge&logo=github)](https://github.com/abdul-wahid022)

**💬 Questions? Need clarification? Feel free to reach out!**

</div>

---

## 📄 License

This project is created for **educational purposes** and is open-source.

---

<div align="center">

### ⭐ If you found this helpful, please give it a star!

**Made with 💙 by ABDUL WAHID**

*Last Updated: January 2026*

</div>
