---
title: Leetcode-26 Remove Duplicates from Sorted Array
date: 2017-12-07 17:13:48
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---


[Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
### 题目描述
Given a sorted array, remove the dupicates **in-place** such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array *in-place* with O(1) extra memory.

### Example

```cpp
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the new length.
```

### 解法1
这道题要我们从有序数组中去除重复项，并返回去重后的数组长度。那么这道题的解题思路是，我们使用快慢指针来记录遍历的坐标，最开始时两个指针都指向第一个数字，如果两个指针指的数字相同，则快指针向前走一步，如果不同，则两个指针都向前走一步，这样当快指针走完整个数组后，慢指针当前的坐标加 `1` 就是数组中不同数字的个数，代码如下：
#### Cpp

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        int n = nums.size(), pre = 0, cur = 0;
        while (cur < n) {
            if (nums[pre] == nums[cur]) ++cur;
            else nums[++pre] = nums[cur++];
        }
        return pre + 1;
    }
};
```
#### Swift

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        if nums.isEmpty { return 0 }
        var pre = 0, cur = 0
        let n = nums.count
        while cur < n {
            if nums[pre] == nums[cur] { cur += 1 }
            else {
                pre += 1
                nums[pre] = nums[cur]
                cur += 1
            }
        }
        return pre + 1
    }
}
```
### 解法2
我们也可以用`for`循环来写，这里的`j`就是上面解法中的`pre`，`i`就是`cur`，所以本质上都是一样的，参见代码如下：
#### Cpp

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        int n = nums.size(), j = 0;
        for (int i = 0; i < n; i++) {
            if (nums[j] != nums[i]) {
                nums[++j] = nums[i];
            }
        }    
        return pre + 1;
    }
};
```
#### Swift

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        if nums.isEmpty { return 0 }
        var j = 0
        let n = nums.count
        for i in 0..<n {
            if nums[j] != nums[i] {
                j += 1
                nums[j] = nums[i]
            }
        }
        return pre + 1
    }
}
```


