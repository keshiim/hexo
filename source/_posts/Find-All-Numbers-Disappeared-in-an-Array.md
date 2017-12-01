---
title: Leetcode-448 Find All Numbers Disappeared in an Array
date: 2017-11-22 10:41:07
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
[Find All Numbers Disappeared in an Array 缺失值查找](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/)

Given an array of integers where $1\leq a[i]\leq n$ (n=size of array), some elements appear twice and tohers appear once. Find all the elements of [1, n] inclusive that do not appear in this array.
Could you do it without extra space and in $O(n)$ runtime? You may assume the returned list does not count as extra space.

### Example
**Input:**
`[4,3,2,7,8,2,3,1]`

**Output:**
`[5,6]`
### 解法一：
这道题让我们找出数组中所有消失的数。乍一看，先拍个序，再遍历整个数组找出未出现数字。但是这要求我们不借助辅助空间且在`O(n)`复杂度内完成。**那这道题的一个最重要的条件就是$1\leq a[i]\leq n$ (n=size of array)**。这里有三种解法。首先先来看第一种解法，这种解法的思路是，对每个数组nums[i]，如果对应的nums[nums[i] - 1]是正数，我们就赋值为其相反数，如果已经是负数不变，那么最后我们只要把留下的证书对应的位置加入结果集`res`中即可，代码如下：
#### C++

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); i++) {
            int idx = abs(nums[i]) - 1
            nums[idx] = (nums[idx] > 0) ? -nums[idx] : nums[idx];
        }
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) {
                res.push_back(i + 1);
            }
        }
        return res;
    }
}
```
#### Swift

```swift
func findDisappearedNumbers(_ nums: [Int]) -> [Int] {
    var res: [Int] = [Int]()
    var nums = nums
    for i in 0..<nums.count {
        let idx = abs(nums[i]) - 1
        nums[idx] = (nums[idx] > 0) ? -nums[idx] : nums[idx]
    }
    for i in 0..<nums.count {
        if nums[i] > 0 {
            res.append(i + 1)
        }
    }
    return res
}
print(findDisappearedNumbers([4,3,2,7,8,2,3,1]))
// Prints [5, 6]
```
### 解法二：
方法二是将`nums[i]`置换到其对应的位置`nums[nums[i] - 1]`上去，比如对于没有缺失项的正确的顺序应该是`[1, 2, 3, 4, 5, 6, 7, 8`，而我们现在确实`[4, 3, 2, 7, 8, 2, 3, 1]`，我们需要把数组移动到正确的位置上去，比如第一个`4`就应该和`7`先交换个位置，以此类推，最后得到的顺序应该是`[1, 2, 3, 4, 3, 2, 7, 8]`，我们最后对应位置检验，如果`nums[i]`和 `i+1`不等，那么我们将`i+1`存入结果`res`中即可，代码如下：
#### C++
```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != nums[nums[i] - 1]) {
                swap(nums[i], nums[nums[i] - 1]);
                --i;
            }
        }
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != i + 1) {
                res.push_back(i + 1);
            }
        }
        return res;
    }
}
```
#### Swift
```swift
func findDisappearedNumbers1(_ nums: [Int]) -> [Int] {
    var res = [Int]()
    var nums = nums
    var i = 0
    while i < nums.count {
        if nums[i] != nums[nums[i] - 1] {
            nums.swapAt(i, nums[i] - 1);
        } else {
            i += 1;
        }
    }
    
    for i in 0..<nums.count {
        if nums[i] != i + 1 {
            res.append(i + 1)
        }
    }
    return res
}
print(findDisappearedNumbers1([4,3,2,7,8,2,3,1]))
```

### 解法三：
和方法一类似，都对`nums[nums[i]-1]`数值做操作，不同的是方法一对值进行负数处理。下面这种方法是对`nums[nums[i]-1]`位置的数值累加数组长度`n`，注意`nums[i] - 1`有可能越界，所以我们需要对`n`取余，最后要找出确实的数只需要看`nums[i]`的值是否小于等于n即可，最后遍历完`nums[i]`数组为`[12, 19, 18, 15, 8, 2, 11, 9]`，我们发现有两个数字8和2小于等于`n`，那么就可以通过`i + 1`来得到正确的结果5、6，代码如下：
#### C++

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); i++) {
            int idx = nums[i] - 1;
            nums[idx % n] += n;
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] <= n) {
                res.push_back(i + 1);
            }
        }
        return res;
    }
}
```
#### Swift

```swift
func findDisappearedNumbers2(_ nums: [Int]) -> [Int] {
    var res = [Int]()
    var nums = nums
    let n = nums.count
    for i in 0..<nums.count {
        let idx = nums[i] - 1
        nums[idx % n] += n
    }
    for i in 0..<n {
        if nums[i] <= n {
            res.append(i + 1)
        }
    }
    return res
}
print(findDisappearedNumbers2([4,3,2,7,8,2,3,1]))
```


