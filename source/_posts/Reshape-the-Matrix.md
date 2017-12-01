---
title: Leetcode-566 Reshape the Matrix
date: 2017-11-16 14:40:07
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
[Reshape the Matrix（重组矩阵）](https://leetcode.com/problems/reshape-the-matrix/description/)

In MATLAB, there is a very useful function called 'reshape', which can reshape a matrix into a new one with different size but keep its original data.

You're given a matrix represented by a two-dimensional array, and two **positive** integers **r** and **c** representing the **row** number and **column** number of the wanted reshaped matrix, respectively.

The reshaped matrix need to be filled with all the elements of the original matrix in the same **row-traversing** order as they were.

If the 'reshape' operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

### Example1

```
Input: 
nums = 
[[1,2],
 [3,4]]
r = 1, c = 4

Output: 
[[1,2,3,4]]

Explanation:
The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.
```
### Example2

```
Input: 
nums = 
[[1,2],
 [3,4]]
r = 2, c = 4

Output: 
[[1,2],
 [3,4]]
 
Explanation:
There is no way to reshape a 2 * 2 matrix to a 2 * 4 matrix. So output the original matrix.
```
### Note
1. The height and width of the given matrix is in range `[1, 100]`.
2. The given r and c are all positive.

- - - - - -
### 分析
这道题让我们实现矩阵大小的重塑。
*答题思路：* 对于这种二维数组大小重新分配的问题的关键就是**对应位置的坐标转换**，最直接的办法就是把原数组拉直，变成一条直线，然后再组成新数组。
### 解法1
这道题我们先判断给定数组是否能重塑成给定的大小，就是看两者的元素个数是否相同，直接行数乘以列数即可，然后我们重建一个目标大小的数组，并开始遍历，对于每个位置，我们先转为拉之后的一维坐标，然后在算出在原数组中的对应位置复制过来即可，代码如下：
#### C++ 

```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        int m = nums.size(), n = nums[0].size();
        if (m * n != r * c) return nums;
        vector<vector<int>> res(r, vector<int>(c));
        for (int i = 0; i < r; i++) {
            for (int j = 0; j < c; j++) {
                int k = i * c + j;
                res[i][j] = nums[k / n][k % n];
            }
        }
        return res;
    }
};

```
#### Swift

```swift
func matrixReshape(_ nums: [[Int]], _ r: Int, _ c: Int) -> [[Int]] {
    let m = nums.count, n = nums[0].count
    if (m * n != r * c) { return nums }
    var res = [[Int]](repeating: [Int](repeating: 0, count: c), count: r)
    for i in 0..<r {
        for j in 0..<c {
            let k = i * c + j
            res[i][j] = nums[k / n][k % n]
        }
    }
    return res
}
print(matrixReshape([[1, 2], [3, 4]], 1, 4))
```

### 解法2
下面这种方法整体思路和上面没啥区别，但是只使用了一个循环**，直接就是遍历拉直后的一维数组的坐标**，然后分别转换为两个二维数组的坐标进行赋值，参见代码如下：
#### C++

```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        int m = nums.size(), n = nums[0].size();
        if (m * n != r * c) return nums;
        vector<vector<int>> res(r, vector<int>(c));
        for (int i = 0; i < r * c; i++) {
            res[i / c][i % c] = nums[i / n][i % n];
        }
        return res;
    }
};
```
#### Swift

```swift
func matrixReshape(_ nums: [[Int]], _ r: Int, _ c: Int) -> [[Int]] {
    let m = nums.count, n = nums[0].count
    if (m * n != r * c) { return nums }
    var res = [[Int]](repeating: [Int](repeating: 0, count: c), count: r)
    for i in 0..<r*c {
        res[i / c][i % c] = nums[i / n][i % n]
    }
    return res
}
print(matrixReshape([[1, 2], [3, 4]], 1, 4))
```


