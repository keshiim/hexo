---
title: Leedcode-283 Move Zeroes
date: 2017-11-23 15:26:19
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---

[Move Zeroes 移动零](https://leetcode.com/problems/move-zeroes/description/)

### 题目描述
Given an array `nums`, write a function to move all `0`s to the end of it while maintaining the relative order of the non-zero elements. For example, given `nums = [0, 1, 0, 3, 12]`, after calling your function, `nums` should be `[1, 3, 12, 0, 0]`.

### Note
1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

### 分析
这道题让我们将一个给定数组中所有的`0`都移到后面，把非零数前移，要求不能改变非零数的相对应的位置关系，而且不能拷贝额外数组，只能用替换法`in-place`来做。
### 解法一
两指针，一个不停的向后扫，找到非零位置，然后和前面那个指针交换位置即可，代码如下：
#### C++

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        for (int i = 0, j = 0; i < nums.size(); i++) {
            if (nums[i]) {
                swap(nums[i], nums[j++]);
            }
        }
    }
};
```

#### Swift

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        var i = 0, j = 0
        while i < nums.count {
            if nums[i] != 0 {
                nums.swapAt(i, j)
                j += 1
            }
            i += 1
        }
    }
}
```
### 解法二
方法一的改进，如果扫到的数不是0，并且上面代码中`j` 和 `i`数值相同，则不需要交换，代码如下：
#### C++
```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        for (int i = 0, j = 0; i < nums.size(); i++) {
            if (nums[i]) {
                if (i != j) {
                    swap(nums[i], nums[j++]);    
                } else {
                    j++;   
                }
            }
        }
    }
};
```
#### Swift
```swift
func moveZeros(_ nums: inout [Int]) {
    var i = 0, j = 0
    while i < nums.count {
        if nums[i] != 0 {
            if  i != j  {
                nums.swapAt(i, j)
            }
            j += 1
        }
        i += 1
    }
}
moveZeros(&nums)
print(nums)
```


