---
layout: post
title: "Array: Subarray Problems"
date: 2025-05-12
tags: ["Data Structures"]
---

## Introduction

- Subarray is a contiguous sequence of elements from the array.
- Each subarray can be defined by two indices
    - Start Index i : [0, n-1]
    - End Index j : [i, n-1]

_How many subarrays ?_

- Number of choices for starting index (i) = n
- For each start index i, the number of choices for end index (j) = n-i

Example: 
- A = [10, 5, 14] 
- n = 3

- For i = 0,
    - for j = 0, SA1 = [10]
    - for j = 1, SA2 = [10, 5]
    - for j = 2, SA3 = [10, 5, 14]
- For i = 1,
    - for j = 1, SA4 = [5]
    - for j = 2, SA5 = [5, 14]
- For i = 2,
    - for j = 2, SA6 = [14]

In General,

Total number of subarrays =
    Number of subarrays for i=0
    + Number of subarrays for i=1
    + Number of subarrays for i=2
    ...
    + Number of subarrays for i=n-2
    + Number of subarrays for i=n-1

Total number of subarrays =
    (n)
    + (n-1)
    + (n-2)
    ...
    + 2
    + 1

Total number of subarrays = 

$$\sig_{i=1}^n i = \frac {n(n+1)}{2}$$

---
## Problem #1 - Longest Subarray with Sum k

**Setup**
- Given an array nums of size n and an integer k, find the length of the longest sub-array that sums to k. If no such sub-array exists, return 0.

Example 1:
- Input: nums = [10, 5, 2, 7, 1, 9],  k=15
- Output: 4

Example 2:
- Input: nums = [-1, 1, 1], k=1
- Output: 3

**Brute-Force Approach**

- Generate all sub-arrays
- Calculate the sum of each sub-array and check if it equals k

<img src="{{site.url}}/images/dsa/array/subarray_sum_equals_k.png">


### 
