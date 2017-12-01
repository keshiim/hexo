---
title: Leetcode-88 Merge Sorted Array
date: 2017-11-28 14:23:53
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---

[Merge Sorted Array 合并有序数组](https://leetcode.com/problems/merge-sorted-array/description/)

### 题目描述
Given two sorted integer arrays *nums1* and *nums2*, merge *nums2* into *nums1* as one sorted array.

### Note
You may assume that *nums1* has enough space (size that is greatr or equal to *m + n*) to hold additional elements from *nums2*. The number of elements initialized in *nums1* and *nums2* are *m* and *n* respectively.

### 分析
混合插入两个有序数组，由于俩数组都是后续的，只需要按顺序比较大小即可。
### 解法一
最先想到的方法就是建立一个`m + n`大小的新数组，然后逐个从`num1`和`num2`数组中去除元素比较，把较小的加入新数组中，然后考虑`nums1`数组有剩余和`nums2`数组有剩余的两种情况，最后把新数组的元素重新赋值到`nums1`数组中即可，代码如下：
#### Cpp

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        if (m <= 0 && n <= 0) return;
        int a = 0, b = 0;
        int merged[m + n];
        for (int i = 0; i < m + n; i++) {
            if (a < m && b < n) {
                if (nums1[a] < nums2[b]) {
                    merged[i] = nums1[a];
                    ++a;
                } else {
                    merged[i] = nums2[b];
                    ++b;
                }
            }
            else if (a < m && b >= n) {
                merged[i] = nums1[a];
                ++a;            
            }
            else if (a >= m && b < n) {
                merged[i] = nums2[b];
                ++b;          
            }
            else return;
        }
        for (int i = 0; i < m + n; i++) nums1[i] = merged[i];
    }
};
```
#### Swift

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        if m <= 0 && n <= 0 {
            return
        }
        var a = 0, b = 0
        var merged = [Int](repeatElement(0, count: m + n))
        for i in 0..<m + n {
            if a < m && b < n {
                //都有数据
                if nums1[a] < nums2[b] {
                    merged[i] = nums1[a];
                    a += 1
                } else {
                    merged[i] = nums2[b]
                    b += 1
                }
            } else if a < m && b >= n {
                // nums2 耗尽
                merged[i] = nums1[a]
                a += 1
            } else if a >= m && b < n {
                // nums1 耗尽
                merged[i] = nums2[b]
                b += 1
            } else  {
                return
            }
        }
        for i in 0..<m + n {
            nums1[i] = merged[i]
        }
    }
}
```

### 解法二
上述解法固然没错，但是还有更简洁的办法，而且不用申请新变量。算法思想是：由于合并后的`nums1`数组的大小必定是`m + n`，所以从最后面开始往前赋值，先比较`nums1`和`nums2`中最后一个元素的大小，把较大的那个插入到`m + n - 1`的位置上，再依次向前推。如果`nums1`中所有的元素都比`nums2`中的小，那么前`m`个元素都是`nums1`的内容，没有改变。如果`nums1`中的数组比`nums2`大的，当`nums1`循环完后，`nums2`中还有剩余元素没有加入到`nums1`，直接用个循环把`nums2`中所有的元素覆盖到`nums1`剩下的位置中。代码如下：
#### Cpp

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int count = m + n - 1;
        --m; --n;
        while (m >= 0 && n >= 0) nums1[count--] = nums1[m] > nums2[n] ? nums1[m--] : nums2[n--];
        while (n >= 0) nums1[count--] = nums2[n--];
    }
};
```
#### Swift

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        var count = m + n - 1
        var m = m - 1
        var n = n - 1
        while m >= 0 && n >= 0 {
            if nums1[m] > nums2[n] {
                nums1[count] = nums1[m];
                m -= 1
            } else {
                nums1[count] = nums2[n]
                n -= 1
            }
            count -= 1
        }
        while n >= 0 {
            nums1[count] = nums2[n]
            count -= 1
            n -= 1
        }
    }
}
```


