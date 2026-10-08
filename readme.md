# 8-Puzzle Solver — BFS & DFS

An AI-based **8-Puzzle Solver** that solves the classic 3×3 sliding puzzle using **Breadth-First Search (BFS)** and **Depth-First Search (DFS)**.

The project compares both uninformed search algorithms based on **execution time, nodes expanded, and solution path length**, and uses BFS with a **Visited Set** as the main solving strategy.

---

## 📌 Project Overview

The 8-Puzzle consists of a 3×3 board containing numbers **1–8** and one blank space.

The objective is to move the tiles from a given **initial state** to a fixed **goal state**.

Example:

```text
Initial State        Goal State

1  2  3              1  2  3
4  0  6      →       4  5  6
7  5  8              7  8  0
```

Here, `0` represents the blank space.

The solver explores possible board configurations until it reaches the goal state.

---

## 🎯 Objectives

* Implement an 8-Puzzle solver.
* Implement **BFS** and **DFS** search algorithms.
* Compare the performance of BFS and DFS.
* Measure:

  * Execution time
  * Number of nodes expanded
  * Solution path length
* Use a **Visited Set** to prevent repeated states.
* Select the most suitable algorithm based on experimental results.
* Apply software architecture concepts using the **C4 Model**.

---

## 🧠 Algorithms Used

### 1. Breadth-First Search (BFS)

BFS explores the puzzle level by level.

It uses:

```text
FIFO Queue
+
Visited Set
```

Because every puzzle move has equal cost, BFS guarantees the **shortest solution path**.

In this project, BFS is the **main solving algorithm**.

---

### 2. Depth-First Search (DFS)

DFS explores one branch as deeply as possible before backtracking.

It uses:

```text
LIFO Stack
+
Visited Set
```

DFS is implemented mainly as a **baseline for comparison**.

Unlike BFS, DFS does not guarantee the shortest solution.

---

## 🔄 BFS Working

The BFS process is:

```text
Start State
     ↓
Add state to Queue
     ↓
Remove first state
     ↓
Is it the Goal?
   ↙       ↘
 Yes        No
 ↓           ↓
Solution   Generate
           Next States
               ↓
        Check Visited Set
               ↓
        Add New States
          to Queue
               ↓
           Repeat
```

The **Visited Set** ensures that the same puzzle configuration is not expanded repeatedly.

---

## 📊 Performance Results

The algorithms were tested on three cases: **Easy, Medium, and Hard**.

| Test Case   | BFS Time (ms) | BFS Nodes | BFS Path | DFS Time (ms) |    DFS Nodes | DFS Path |
| ----------- | ------------: | --------: | -------: | ------------: | -----------: | -------: |
| Easy        |        0.1383 |        17 |        4 |      496.9879 |      120,489 |   59,124 |
| Medium      |        0.4977 |       244 |        8 |      262.5609 |      105,311 |   65,110 |
| Hard        |        3.9239 |     2,149 |       12 |      127.8745 |       51,549 |   46,516 |
| **Average** |    **1.5200** | **803.3** |        — |  **295.8078** | **92,449.7** |        — |

Based on the SLE-2 profiling results, BFS averaged **1.5200 ms** and expanded **803.3 nodes**, while DFS averaged **295.8078 ms** and expanded **92,449.7 nodes**.

---

## 🏆 Why BFS Was Selected

BFS was selected as the main algorithm because:

### ✅ Optimal Solution

For unit-cost moves, BFS guarantees the shortest path.

The tested solutions were:

```text
Easy   → 4 moves
Medium → 8 moves
Hard   → 12 moves
```

### ✅ Faster in the Tested Cases

BFS was approximately **195× faster on average** than DFS.

### ✅ Fewer Nodes

BFS expanded approximately **115× fewer nodes** than DFS.

### ✅ Simple Architecture

BFS and DFS use the same puzzle model, visited set, and profiler.

The main difference is the **Frontier**:

```text
BFS → Queue
DFS → Stack
```

This keeps the architecture simple and makes the comparison fair.

---

## 🏗️ System Architecture

The project follows the **C4 Model** architecture developed during SLE-3.

High-level flow:

```text
             ┌─────────────┐
             │    Input    │
             └──────┬──────┘
                    ↓
          ┌──────────────────┐
          │   Puzzle Model   │
          └────────┬─────────┘
                   ↕
          ┌──────────────────┐
          │   Search Engine  │
          │                  │
          │   BFS / DFS      │
          └────────┬─────────┘
                   ↓
          ┌──────────────────┐
          │     Output       │
          └──────────────────┘

        ┌─────────────────────┐
        │     Visited Set     │
        └─────────────────────┘

        ┌─────────────────────┐
        │      Profiler       │
        └─────────────────────┘
```

The main containers are:

* **Puzzle Model** — represents the puzzle state and generates valid moves.
* **Search Engine** — performs BFS or DFS.
* **Visited Set** — stores already explored states.
* **Profiler** — measures execution time and nodes expanded.
* **Input/Output** — handles puzzle input and displays the solution.

---

## 🧩 Why Use a Visited Set?

Without a Visited Set, the blank tile can move back and forth:

```text
State A
   ↓
State B
   ↓
State A
   ↓
State B
   ↓
...
```

This creates unnecessary repeated exploration.

The Visited Set prevents this:

```text
New State
    ↓
Already Visited?
   ↙       ↘
 Yes        No
 ↓           ↓
Skip      Explore
             ↓
       Add to Visited Set
```

Therefore, each state can be expanded only once.

---

## 📁 Project Structure

A suggested structure for the project is:

```text
8-Puzzle-Solver/
│
├── README.md
├── agent.py / main.py
│
├── puzzle/
│   └── puzzle_model.py
│
├── search/
│   ├── bfs.py
│   └── dfs.py
│
├── profiler/
│   └── profiler.py
│
├── tests/
│   └── test_cases.py
│
└── docs/
    └── architecture/
```

> Update the filenames above if your actual repository uses different filenames.

---

## 🧪 Test Cases

The solver was evaluated using three difficulty levels:

### Easy

* Short solution path
* Very few states expanded
* BFS solved in **4 moves**

### Medium

* Larger search space
* BFS solved in **8 moves**

### Hard

* More states need to be explored
* BFS solved in **12 moves**

The profiling results show that BFS effort increased from **17 → 244 → 2,149 nodes** as the test cases became harder.

---

## ⚖️ BFS vs DFS

| Feature         | BFS                       | DFS                       |
| --------------- | ------------------------- | ------------------------- |
| Data Structure  | Queue                     | Stack                     |
| Complete        | Yes, for finite branching | Depends on implementation |
| Shortest Path   | **Yes** for unit costs    | No                        |
| Memory Usage    | High                      | Generally lower           |
| Main Use        | Optimal solution          | Baseline/deep exploration |
| Used in Project | **Main Algorithm**        | Comparison                |

---

## 🔮 Future Improvements

Although BFS works very well for the 3×3 8-Puzzle, it has limitations.

BFS stores a large number of states in memory as the solution depth increases. It also does not use heuristic information.

Possible future improvements include:

### A* Search

Use **Manhattan Distance** as a heuristic:

```text
f(n) = g(n) + h(n)
```

where:

* `g(n)` = cost from the start state
* `h(n)` = estimated cost to the goal

A* could explore fewer states than BFS.

### Larger Puzzles

The project could be extended to:

```text
8-Puzzle  →  15-Puzzle  →  Larger Boards
```

For larger puzzles, algorithms such as **A*** or **IDA*** would be more suitable because BFS memory requirements grow rapidly.

---

## 🤖 AI Contribution

AI tools were used during the project for:

* Organizing the ADR format
* Drafting wording
* Preparing viva notes

The algorithmic decision and experimental numbers were checked against the project's own SLE-2 and SLE-3 work.

---

## 📚 SLE Journey

### SLE-1

Developed a **rule-based Python chatbot** and learned about:

* Git/GitHub
* Version control
* AI-assisted development

### SLE-2

Implemented and profiled:

* BFS
* DFS
* 8-Puzzle test cases
* Execution time measurement
* Nodes expanded
* Solution path length

### SLE-3

Designed the **C4 architecture**:

* Context Diagram
* Container Diagram
* Component Diagram
* Code-level design

### SLE-4

Combined the previous work into an **Architecture Decision Record (ADR)** and selected:

> **BFS + Visited Set**

as the main solving strategy.

---

## 👨‍💻 Author

**Sanchit Sachin Magdum**

* PRN: 25UAM133
* Division: B
* Department: CSE (AI & ML)
* Course: 02AML204 — Introduction to Artificial Intelligence

### GitHub

https://github.com/sanchitmagdum17

---

## 📄 Conclusion

The 8-Puzzle Solver demonstrates how uninformed search algorithms can be applied to an AI problem.

Based on the experimental results, **BFS with a Visited Set** was selected because it provides:

* Shortest solutions
* Low execution time for the tested 8-Puzzle cases
* Fewer expanded nodes
* Simple implementation
* Simple integration with the C4 architecture

The main limitation is memory usage, making **A*** or **IDA*** potential choices for larger puzzle problems.
