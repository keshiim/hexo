---
title: Leetcode-673 number of Longest Increasing Subsequence
date: 2017-12-06 15:39:47
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Dynamic Programming
    - Medium
---

[Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/description/)
### 题目描述
Given an unsorted array of integers, find the number of longest increasing subsequence.

### Example

```cpp
Input: [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].

Input: [2,2,2,2,2]
Output: 5
Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
```
### Note
Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.

### 分析
动态规划求解，建立两个数组 `len`、`cnt`： 

* `len[k]`: 表示以 `nums[k]` 为末尾的最长子序列长度。 
* `cnt[k]`: 表示以 `nums[k]` 为末尾的最长子序列个数。

对于每一个 `nums[i]`，只需要遍历其前面的所有数字 `nums[j] (0 <= j < i) `，找到比它小且长度最长的 `nums[k]`，就可得出以 `nums[i]` 为末尾的子序列的最大长度 len[i] = `len[k] + 1` 。

同时，以 `nums[i]` 为末尾的最长子序列个数应该等于 `nums[j] (0 <= j < i) `中，比它小且长度最长的所有 `nums[k]` 的最长子序列个数之和。

用两条公式来阐述上面一段难以理解的话
 
* `len[i] = max(len[i], len[k] + 1), for all 0 <= k < i and nums[k] < nums[i] and len[j] + 1 > len[i]`
* `cnt[i] = sum(cnt[k]), for all 0 <= k < i and len[i] = len[k] + 1`

--------
举个栗子：
`nums = {1, 3, 5, 4, 7}`

初始状态，`len = {1, 1, 1, 1, 1}`，`cnt = {1, 1, 1, 1, 1}`

开始遍历

* 数字 3，比它小的只有 1 => `len[1] = len[0] + 1 = 2，cnt[1] = cnt[0] = 1`；
* 数字 5，比它小且长度最长的为 3 => `len[2] = len[1] + 1 = 3，cnt[2] = cnt[1] = 1`； 
* 数字 4， 比它小且长度最长的为 3 => `len[3] = len[1] + 1 = 3，cnt[3] = cnt[1] = 1`； 
* 数字 7，比它小且长度最长的为 5 和 4 => `len[4] = len[2] + 1 = 4，cnt[4] = cnt[2] + cnt[3] = 2`

最终状态，`len = {1, 2, 3, 3, 4}`，`cnt = {1, 1, 1, 1, 2}`

### 代码
#### cpp
```cpp
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int maxLen = 1, res = 0, n = nums.size();
        vector<int> len(n, 1);
        vector<int> cnt(n, 1);
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    if (len[j] + 1 > len[i]) {
                        len[i] = len[j] + 1;
                        cnt[i] = cnt[j];
                    } else if (len[j] + 1 == len[i]) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            maxLen = max(maxLen, len[i]);
        }
        for (int i = 0; i < n; i++) {
            if (maxLen == len[i]) res += cnt[i];
        }
        return res;
    }
};
```
#### Swift

```swift
class Solution {
    func findNumberOfLIS(_ nums: [Int]) -> Int {
        if nums.isEmpty {
            return 0
        }
        var maxLen = 1, res = 0
        let n = nums.count
        var len = [Int](repeating: 1, count: n)
        var cnt = [Int](repeating: 1, count: n)
        for i in 1..<n {
            for j in 0..<i {
                if nums[j] < nums[i] {
                    if len[j] + 1 > len[i] {
                        len[i] = len[j] + 1
                        cnt[i] = cnt[j]
                    } else if len[j] + 1 == len[i] {
                        cnt[i] += cnt[j]
                    }
                }
            }
            maxLen = max(maxLen, len[i])
        }
        for i in 0..<n {
            if maxLen == len[i] { res += cnt[i] }
        }
        return res
    }
}
```

### 类似题目
[Longest continuous increasing](https://keshiim.github.io/2017/12/06/Leetcode-674-Longest-Continuous-Increasing/)


