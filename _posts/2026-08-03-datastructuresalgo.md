---
layout: distill
title: Data Structures & Algorithms 
description: >-
  DSA Topics and Notes <br>
tags:
  - notes
published: true
giscus_comments: false
hide_date: true
featured: false

_styles: >
  .post.distill p,
  .post.distill li {
    line-height: 1.5;
    margin-bottom: 0.45rem;
  }

  .post.distill h2,
  .post.distill h3 {
    margin-top: 1rem;
    margin-bottom: 0.35rem;
  }

  .post.distill ul {
    margin-top: 0.25rem;
    margin-bottom: 0.5rem;
  }

toc:
  - name: Introduction
    subsections:
        - name: Big O Notation

---

## Introduction

### Big O Notation 
Big O notation is an important concept in computer science. It is used to help us describe the time complexity and space complexity for a particular algorithm. 
- **Time Complexity**: how the run time changes as the input size (```n```) changes 
- **Space Complexity**: how much memory the algorithm requires when input size (```n```)  changes
- Big O notation describes the asymptotic upper bound on an algorithm's growth rate as the input size (n) becomes very large. It is commonly used to describe worst case time or space complexity, although it can also describe best- ase or average case complexity when specified
- We ignore constants and lower order terms because they become insignificant as n grows larger 
- Algorithms can be analyzed using three cases: **Best Case**, **Average Case**, and **Worst Case**. These cases may differ depending on the algorithm and input conditions 
- **Worst-case scenario** is typically chosen to represent an algorithm because it provides a reliable upper bound on performance.

#### Growth Rates 

Fastest → Slowest:

`O(1)` → `O(log n)` → `O(n)` → `O(n log n)` → `O(n²)` → `O(2ⁿ)` → `O(n!)`

As n becomes larger, algorithms with slower growth rates generally scale better 

#### Properties of Big O Notation
- **Reflexivity:** A function is always Big O of itself (For function `f(n)`, `f(n) = O(f(n))`)
- **Transitivity:** If `f(n) = O(g(n))` and `g(n) = O(h(n))`, then `f(n) = O(h(n))`
- **Constant Factor Rule:** Constants can be ignored because they do not affect growth rate (`O(2n)` → `O(n)`)
- **Sum Rule:** When adding complexities, keep only the dominant term (`O(n² + n)` → `O(n²)`)
- **Product Rule:** When multiplying operations, multiply their complexities (`O(n) * O(n)` → `O(n²)`)
- **Composition Rule:** When one operation is performed inside another (such as nested loops or a function called repeatedly inside a loop), multiply their complexities (If `f(n) = O(g(n))`, then `f(h(n)) = O(g(h(n)))`)

#### Analyzing Time Complexity 
Time complexity is calculated by counting how the number of operations executed by the algorithm changes as input size (```n```) changes 

**General Steps**:
1. Identify the basic operations performed by the algorithm
2. Determine how many times each operation executes relative to n
3. Analyze loops, nested loops, conditionals, and recursive calls
4. Combine the resulting complexities
5. Remove constant factors and lower order terms
6. Express the result using Big O notation


**Common Patterns**:
- Single operation → `O(1)`
- Single loop → `O(n)`
- Two consecutive loops → `O(n)`
- Nested loops → `O(n²)`
- Three nested loops → `O(n³)`
- Divide the input in half each iteration → `O(log n)`
- Loop with a logarithmic operation → `O(n log n)`
- Recursive branching into two calls → Often `O(2ⁿ)`
- Generate every permutation → `O(n!)`


#### Analyzing Space Complexity

Space complexity measures how the amount of memory an algorithm requires changes as the input size (`n`) changes

When analyzing space complexity, we focus on the additional memory required by the algorithm like variables, arrays, lists, hash tables, and recursive call stacks.

**General Steps**:
1. Identify the additional memory allocated by the algorithm
2. Determine whether that memory grows with the input size (n)
3. Analyze data structures such as arrays, lists, and hash tables
4. Analyze recursive algorithms because each recursive call consumes stack space
5. Ignore constant factors and lower order terms
6. Express the result using Big O notation


#### Common Big O Complexities

- Constant Time (`O(1)`)
- Linear Time (`O(n)`)
- Logarithmic Time (`O(log n)`)
- Linearithmic Time (`O(n log n)`)
- Quadratic Time (`O(n²)`)
- Cubic Time (`O(n³)`)
- Polynomial Time (`O(nᵏ)`)
- Exponential Time (`O(2ⁿ)`)
- Factorial Time (`O(n!)`)