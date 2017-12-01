---
title: Leetcode-695 Max Area of Island
date: 2017-11-20 13:50:38
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
Given a non-empty 2D array **grid** of 0's and 1's, an **island** is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

### Example 1:

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```
Given the above grid, return **6**. Note the answer is not `11`, because the island must be **connected 4-directionally**.
### Example 2:

```
[[0,0,0,0,0,0,0,0]]
```
Given the above grid, return **0**.

### Note
The length of each dimension in the given grid does not exceed 50.



### 解法一：
这道题跟[Number of Islands]() 和 [Number of Distinct Islands]() 是同一种类型的，不过这道题要统计岛的大小，再来更新结果`res`。先用递归来做，遍历`grid`，当遇到为`1`的点，我们调用递归函数，在递归函数中，我们首先判断`i`和`j`是否越界，还有`grid[i][j]`是否为`1`，我们没有用`visited`数组，而是直接修改了`grid`数组，遍历过的标记为`-1`。如果合法，那么`cnt`自增`1`，并且更新结果`res`，然后对其周围四个相邻位置分别调用递归函数即可，代码如下：
#### C++
```c++
class Solution {
public:
    vector<vector<int>> dirs{{0, -1}, {-1, 0}, {0, 1}, {1, 0}};
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size(), res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != 1) continue;
                int cnt = 0;
                helper(grid, i, j, cnt, res);
            }
        }
        return res;
    }
    void helper(vector<vector<int>>& grid, int i, int j, int& cnt, int& res) {
        int m = grid.size(), n = grid[0].size();
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] <= 0) return;
        res = max(res, ++cnt);
        grid[i][j] *= -1;
        for (auto dir : dirs) {
            helper(grid, i + dir[0], j + dir[1], cnt, res);
        }
    }
};
```
#### swift
```swift
class Solution {
    let dirs = [[0, -1], [-1, 0], [0, 1], [1, 0]]
    func maxAreaOfIsland(_ grid: [[Int]]) -> Int {
        let m = grid.count, n = grid[0].count
        var grid = grid
        var res = 0
        for i in 0..<m {
            for j in 0..<n {
                if grid[i][j] != 1 { continue }
                var cnt = 0
                helper(&grid, i: i, j: j, cnt: &cnt, res: &res)
            }
        }
        return res
    }
    func helper(_ grid: inout [[Int]], i: Int, j: Int, cnt: inout Int, res: inout Int) {
        let m = grid.count, n = grid[0].count
        if i < 0 || i >= m || j < 0 || j >= n || grid[i][j] <= 0 { return }
        cnt+=1
        res = max(res, cnt)
        grid[i][j] *= -1
        for dir in dirs {
            helper(&grid, i: i + dir[0], j: j + dir[1], cnt: &cnt, res: &res)
        }
    }
}
```


### 解法二：
下面是迭代的写法，**BFS**遍历，使用**queue**来辅助运算，思路没啥太大区别，都是套路，都是模版，往里套就行了，参见代码如下：

#### C++

```c++
class Solution {
public:
    vector<vector<int>> dirs{{0, -1}, {-1, 0}, {0, 1}, {1, 0}};
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size(), res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != 1) continue;
                int cnt = 0;
                queue<pair<int, int>> q{{{i, j}}};
                grid[i][j] *= -1;
                while (!q.empty()) {
                    auto t = q.front(); q.pop();
                    res = max(res, ++cnt);
                    for  (auto dir : dirs) {
                        int x = t.first + dir[0], y = t.second + dir[1];
                        if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] <= 0) continue;
                        grid[x][y] *= -1;
                        q.push({x, y});
                    }
                }
            }
        }
        return res;
    }
};
```

