---
title: Leedcode-697 Degree of an Array
date: 2017-12-04 14:47:00
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---
[Degree of an Array](https://leetcode.com/problems/degree-of-an-array/description/)

### 题目描述
Given a non-empty array of non-negative integers `nums`, the **degree** of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of `nums`, that has the same degree as `nums`.
### Example 

```cpp
Input: [1, 2, 2, 3, 1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.
------------------------
Input: [1,2,2,3,1,4,2]
Output: 6
```
### Note
* `nums.length` will be between 1 and 50,000.
* `nums[i]` will be an integer between 0 and 49,999.

### 分析
这道题给我一个数组，定义数组的度为某个或某些数字出现**最多**的次数，现让我们找到最短的子数组使其与原数组拥有相同的度。

那么我们需要统计每个数字出现的次数，**用哈希表`map<数字, 次数> m`来建立起数字和出现次数的映射**。由于我们要求包含最大度的最小长度子数组，那么最好的情况就是子数组首位数字就是统计度的数字，即出现最多的数字。那么我们肯定要知道该数字第一次出现的位置和最后一次出现的位置，由于我们开始的时候并不知道那个数字出现次数最多，所以我们统计所有数字的首位出现位置，那么我用再用一个哈希表`map<数字, pair<首, 尾>> pos`来建立每个数字与其首位出现位置的关系。我们用变量`degree`来表示数组的度。

### 解法1
现在我们遍历原数组，累加当前数字出现的次数，当某个数字是第一次出现，那么我们用当前位置来更新改数字出现的首位置，否则更新尾位置。没遍历一个数，我们就更新一个degree。当前遍历完成后，我们已经有了数组的度，还有每个数字首位出现的位置，下面就来找出出现次数等于`degree`的数字，然后计算其首位位置差加上`1`，由于出现次数为`degree`的数字不一定只有一个，我们遍历所有的，找出其中最小的即可。代码如下
#### cpp

```cpp
class Sulotion {
public:
    int findShortestSubArray(vector<int> &nums) {
        int n = nums.size(), res = INT_MAX, degree = 0;
        unordered_map<int, int> m;
        unrodered_map<int, pair<int, int>> pos;
        for (int i = 0; i < n; i++) {
            if (++m[nums[i]] == 1) {
                pos[nums[i]] = {i, i};
            } else {
                pos[nums[i]].second = i;
            }
            degree = max(degree, m[nums[i]]);
        }
        for (auto a : m) {
            if (degree == a.second) {   
                res = min(res, pos[a.first].second - pos[a.first].first + 1);
            }
        }
    return res;
    }
};
```
#### Swift

```swift
class Solution {
    func findShortestSubArray(_ nums: [Int]) -> Int {
        let n = nums.count
        var degree = 0, res = Int.max
        var m = [Int: Int]();
        var pos = [Int: [Int]]()
        for i in 0..<n {
            let num = nums[i]
            let count = (m[num] ?? 0) + 1 //次数累加
            m[num] = count
            if count == 1 {
                //说明第一次记录
                pos[num] = [i, i]
            } else {
                let position = pos[num]
                pos[num] = [(position?.first)!, i];
            }
            degree = max(degree, count)
        }
        for (num, count) in m {
            if count == degree {
                res = min(res, (pos[num]?.last)! - (pos[num]?.first)! + 1)
            }
        }
        return res
    }
}
```
### 解法2
下面这种方法只用了一次遍历，思路上跟上面的解法类似，还是要建立数字出现次数的哈希表，还有就是建立每个数字和其**第一次**出现位置之间的映射，那么我们当前遍历的位置其实可以看作是尾位置，还是可以计算子数组的长度的。我们遍历数组，累加当前数字出现的次数，如果某个数字是第一次出现，建立该数字和当前位置的映射，如果当前数字的出现次数等于`degree`时，当前位置为尾位置，首位置在`startIdx`中取的，二者做差加1来更新结果`res`；如果当前数字的出现次数大于`degree`，说明之前的结果代表的数字不是出现最多的，直接将结果`res`更新为当前数字的首尾差加`1`的长度，然后`degree`也更新为当前数字出现的次数。
#### cpp

```cpp
class Solution {
public:
    int findShortestSubArray(vector<int, int> &nums) {
        int n = nums.size(), res = INT_MAX, degree = 0
        unordered_map<int, int> m, startIdx;
        for (int i = 0; i < n; i++) {
            ++m[nums[i]];
            if (!startIdx.cout(nums[i])) {
                startIdx[i] = i;
            }
            if (m[nums[i]] == degree) {
                res = min(res, i - startIdx[nums[i]] + 1);
            } else if (m[nums[i] > degree) {
                res = i - startIdx[nums[i]] + 1;
                degree = m[nums[i]];
            }
        }
        return res;
    }
}
```
#### Swift

```swift
class Solution {
    func findShortestSubArray(_ nums: [Int]) -> Int {
        let n = nums.count
        var res = Int.max, degree = 0
        var m = [Int: Int](), startIdx = [Int: Int]()
        for i in 0..<n {
            m[nums[i]] = (m[nums[i]] ?? 0) + 1 //先设置出现次数
            if startIdx[nums[i]] == nil {
                startIdx[nums[i]] = i  //设置初始位置
            }
            if m[nums[i]] == degree {
                res = min(res, i - startIdx[nums[i]]! + 1)
            } else if m[nums[i]]! > degree {
                res = i - startIdx[nums[i]]! + 1
                degree = m[nums[i]]!
            }
        }
        return res
    }
}
```

