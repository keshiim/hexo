---
title: Leetcode-485 Max Consecutive Ones
date: 2017-8-14
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---

### 题目描述
[Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/description/)
Given a binary array, find the maximum number of consecutive 1s in this array.

**Example 1:**

``` c
Input: [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s.
    The maximum number of consecutive 1s is 3.
```

**Note:**

* The input array will only contain **0** and **1**.
* The length of input array is a positive integer and will not exceed 10,000

- - -
这道题让我们求最大连续整数1的个数，难易程度为：**简单**。

#### 方法

我们可以遍历一遍数组，用给一个计数器`count`来统计1的个数。如果遍历当前数字为0，那么`count`重置为0，如果不是0，`count`自增1，与全局result做max比较即可，时空复杂度分别为 $O(n)$ 和 $O(1)$ 参照以下代码：

**Swift version :**

```swift
class Solution {
    func findMaxConsecutiveOnes(_ nums: [Int]) -> Int {
        var globalMax: Int = 0, localMax: Int = 0
        
        for num in nums {
            if num == 1 {
                localMax += 1
                globalMax = max(globalMax, localMax)
            } else {
                localMax = 0
            }
        }
        return globalMax
    }
}
```

**C version :**

```c
int findMaxConsecutiveOnes(int* nums, int numsSize) {
    int globalMax = 0, localMax = 0;
    for(int i = 0; i < numsSize; i++) {
        if (nums[i] == 1) {
            globalMax = globalMax > ++localMax ? globalMax : localMax;
        } else {
            localMax = 0;
        }
    }
    return globalMax;
}
```

