---
layout: post
title: "Array: Subarray Problems"
date: 2025-08-28
tags: ["Data Structures"]
---

Array is very important and widely used data structure
---
## Introduction

- Subarray is a contiguous sequence of elements from the array.
- Each subarray can be defined by two indices
    - Start Index i : [0, n-1]
    - End Index j : [i, n-1]

_How many subarrays are present in a given array of length n ?_

- Number of choices for starting index (i) = n
- For each start index i, the number of choices for end index (j) = n-i

Example: 
- A = [10, 5, 14] n = 3
- For i = 0,
    - for j = 0, SA1 = [10]
    - for j = 1, SA2 = [10, 5]
    - for j = 2, SA3 = [10, 5, 14]
- For i = 1,
    - for j = 1, SA4 = [5]
    - for j = 2, SA5 = [5, 14]
- For i = 2,
    - for j = 2, SA6 = [14]
- Total number of subarrays = 3 + 2 + 1 = 6

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

$$\sigma_{i=1}^n i = \frac {n(n+1)}{2}$$

---
## Problem #1 - Longest Subarray with Sum k

### Setup
- Given an array nums of size n and an integer k, find the length of the longest sub-array that sums to k. If no such sub-array exists, return 0.

Example 1:
- Input: nums = [10, 5, 2, 7, 1, 9],  k=15
- Output: 4

Example 2:
- Input: nums = [-1, 1, 1], k=1
- Output: 3

### Brute-Force Approach

_Logic_
- Generate all sub-arrays
- Calculate the sum of each sub-array and check if it equals k

<img src="{{site.url}}/images/dsa/array/subarray_sum_equals_k.png">

_Code_

```
def longest_subarr(arr, k):
    n = len(arr)
    max_len = 0
    for i in range(n):
        for j in range(i,n):
            sum = 0
            for p in range(i,j+1):
                sum += nums[p]
                if sum == k:
                    max_len = max(max_len, p+1-j)
    return max_len
```

_Time Complexity_
- For each subarray defined by (i,j), we are iterating through this subarray to find its sum

$$
T(n) = (1+2+...+n) + (1+2+...+n-1) + ... + (1+2) + 1
\RightArrow T(n) = O(n^3)
$$

### 
