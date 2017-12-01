---
title: Leetcode-1 Two Sum
date: 2017-11-16 16:00:42
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
[Two Sum（两数之和）](https://leetcode.com/problems/two-sum/description/)

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.
You may assume that each input would have **exactly** one solution, and you may not use the same element twice.

### Example

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
- - - 

### 分析
咋一看这道题就知道用暴力迭代肯定能解决问题，但是LeetCode肯定不会接受这种暴力搜索这么简单的方法（其时间复杂度$O(n^2)$）。

### 解法1
先遍历一遍数组，建立`map<key=(数组值), vale=(索引)>`数据。然后再遍历一遍，开始超找，找到则记录index。代码如下：
#### C++

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int>m;
        vector<int> res;
        for(int i = 0; i < nums.size(); i++) {
            m[nums[i]] = i;
        }
        for(int i = 0; i < nums.size(); i++) {
            int t = target - nums[i];
            if (m.count(t) && m[t] != i) {
                res.push_back(i);
                res.push_back(m[t]);
                break;
            }
        }
        return res;
    }
};
```
#### Swift

```swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var map: [Int: Int] = [:]
        for i in 0..<nums.count {
            map[nums[i]] = i
        }
        var res = [Int]()
        for i in 0..<nums.count {
            let t = target - nums[i]
            if let index = map[t], map[t] != i {
                res.append(i)
                res.append(index)
                break;
            }
        }
        return res
    }
}
```
### 解法2
或者我们可以写的更加简洁一些，把两个for循环合并成一个：
#### C++

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> m;
        for (int i = 0; i < nums.size(); ++i) {
            if (m.count(target - nums[i])) {
                return {i, m[target - nums[i]]};
            }
            m[nums[i]] = i;
        }
        return {};
    }
};
```
#### Swift

```
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var map: [Int: Int] = [:]
        for i in 0..<nums.count {
            if let index = map[target - nums[i]] {
                return [index, i]
            }
            map[nums[i]] = i
        }
        return []
    }
}
```

