---
title: Leetcode-561 Array Partition I
date: 2017-11-14 14:25:41
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
[Array Partintion I (数组分割I)](https://leetcode.com/problems/array-partition-i/description/)

Given an array of **2n** integers, your task is to group these integers into **n** pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

### Example

```c
Input: [1,4,3,2]

Output: 4
Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
```
### Note
1. **n** is a positive integer, which is in the range of `[1, 10000]`.
2. All the integers in the array will be in the range of `[-10000, 10000]`.

------
### 分析
这道题，让我们分隔数组形成两两一对，让每对中较小数的和最大。

若要最大化每对中的较小数之和，那么肯定是没对中两个数字大小**越相近也好**。因为如果差距过大，而我们只取每对中最小数字，那么较大的那个数就被浪费掉了。因此，我们只需要给数组排序，然后按照顺序的每两个就是一对。取出每对中第一个数字(较小值)累加起来即可。
### 代码
#### Swift

```swift
func arrayPairSum(_ nums: [Int]) -> Int {
    var res = 0
    let n = nums.count
    let numsSored = nums.sorted{ $0 < $1 } //排序
    for i in stride(from: 0, to: n, by: 2) {
        res += numsSored[i]
    }
    return res
}
/*
这里使用到了Swift标准库里 Collection定义的函数做迭代步长

/// Returns the sequence of values (`self`, `self + stride`, `self +
/// 2 * stride`, ... *last*) where *last* is the last value in the
/// progression that is less than `end`.
public func stride<T>(from start: T, to end: T, by stride: T.Stride) -> StrideTo<T> where T : Strideable
*/
```
#### C++ 

```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        int res = 0, n = nums.size();
        sort(nums.begin(), nums.end());
        for(int i = 0; i < n; i+=2) {
            res += nums[i];
        }
        return res;
    }
};
```
#### Java

```java
class Solution {
    public int arrayPairSum(int[] nums) {
        int res = 0, n = nums.length;
        Arrays.sort(nums);
        for(int i = 0; i < n; i+=2) {
            res += nums[i];
        }
        return res;
    }
}
```

