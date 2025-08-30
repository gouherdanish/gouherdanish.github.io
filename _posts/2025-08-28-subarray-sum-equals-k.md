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
- Let's take an example of an array A having n = 3 elements
    - A = [10, 5, 14] n = 3
- Here are the possible subarrays that we can create from A 
    - [10]
    - [10, 5]
    - [10, 5, 14]
    - [5]
    - [5, 14]
    - [14]
- Total number of subarrays = 3 + 2 + 1 = 6

_How many subarrays are present in a given array of length n ?_

- In short, this is given by $\binom{n+1}{2}$
- Intuitively, each subarray can be defined by two indices
    - Start Index i : [0, n-1]
    - End Index j : [i, n-1]
- Number of choices for starting index (i) = n
- For each start index i, the number of choices for end index (j) = n-i

So, Total number of subarrays = $\sum_{i=0}^{n} (n-i) = \frac {n(n+1)}{2}$

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
- $O(n^3)$

$$ T(n) = (1+2+...+n) + (1+2+...+n-1) + ... + (1+2) + 1 $$

$$ \RightArrow T(n) = O(n^3) $$

_Space Complexity_
- We are using $O(1)$ space

### Running Sum Approach

_Logic_

- Instead of finding each end indices and then iterating start to end, we can save one loop by using running sum

_Code_

```
def longest_subarr(arr, k):
    n = len(arr)
    max_len = 0
    for i in range(n):
        sum = 0
        for j in range(i,n):
            sum += nums[j]
            if sum == k:
                max_len = max(max_len, p+1-j)
    return max_len
```

_Time Complexity_

- For each start index i, the inner loop is running from i to n

$$ T(n) = (n) + (n-1) + ... + (2) + (1) $$

$$ \RightArrow T(n) = O(n^2) $$

### Hashmap Approach

_Logic_

- 
