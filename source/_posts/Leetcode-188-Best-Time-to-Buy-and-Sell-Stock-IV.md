---
title: Leetcode-188 Best Time to Buy and Sell Stock IV
date: 2017-11-24 14:10:23
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Dynamic Programming
    - Hard
---

[Best Time to Buy and Sell Stock IV 面股票的最佳时间之四](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

### 题目描述
Say you have an array for which the $i^{th}$ element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most **K** transactions.
### Note
You may not engage in multiple transactions at the same time (ie, you must sell the stock befor you buy again).
### 分析
这道题实际上是之前那道[Best Time to Buy and Sell Stock III 买股票的最佳时间之三](https://keshiim.github.io/2017/11/24/Leetcode-123-Best-Time-to-Buy-and-Sell-Stock-III/)的一半情况的推广，还是需要用动态规划（Dynamic programming）来解决，其思路如下：

这里我们需要两个递推公式来分别更新两个变量`local`和`global`，我们其实可以求至少`k`次交易的最大利润。
我们定义`local[i][j]`为在到达第`i`天时最多可进行j次交易并且最后一次交易在最后一天卖出的最大利润，此为局部最优。
然后我们定义`global[i][j]`为在到达第i天时最多可进行j次交易的最大利润，此为全局最优。它们的递推式为：

```c++
local[i][j] = max(global[i - 1][j - 1] + max(diff, 0), local[i - 1][j] + diff)

global[i][j] = max(local[i][j], global[i - 1][j])，
```
其中局部最优值是比较前一天并少交易一次的全局最优加上**大于0的差值**，和前一天的局部最优加上差值后相比，两者之中取较大值，而全局最优比较局部最优和前一天的全局最优。

但这道题还有个坑，就是如果k的值远大于`prices`的天数，比如`k`是好几百万，而`prices`的天数就为若干天的话，上面的**DP**解法就非常的没有效率，应该直接用[Best Time to Buy and Sell Stock II](https://keshiim.github.io/2017/11/24/Leetcode-122-Best-Time-to-Buy-and-Sell-Stock-II/) 买股票的最佳时间之二的方法来求解，所以实际上这道题是之前的二和三的综合体，代码如下：
### 解题
#### C++

```c++
class Solution {
public:
    int maxProfit(int k, vector<int> &prices) {
        if (prices.empty()) return 0;
        if (k >= prices.size()) return solveMaxProfit(prices);
        int g[k + 1] = {0};
        int l[k + 1] = {0};
        for (int i = 0; i < prices.size() - 1; i++) {
            int diff = prices[i + 1] - prices[i];
            for (int j = k; j >= 1; j--) {
                l[j] = max(g[j - 1] + max(diff, 0), l[j] + diff);
                g[j] = max(g[j], l[j]);
            }
        }
        return g[k];
    }
    int solveMaxProfit(vector<int> &prices) {
        int res = 0;
        for (int i = 1; i < prices.size(); i++) {
            if (prices[i] - prices[i - 1] > 0) {
                res += prices[i] - prices[i - 1];
            }
        }
        return res;
    }
};
```
#### Swift

```swift
class Solution {
    func maxProfit(_ k: Int, _ prices: [Int]) -> Int {
        if prices.isEmpty {
            return 0
        }
        if prices.count <= k {
            return solveMaxProfit(prices)
        }
        var g = [Int](repeatElement(0, count: k + 1))
        var l = [Int](repeatElement(0, count: k + 1))
        for i in 0..<prices.count - 1 {
            let diff = prices[i + 1] - prices[i]
            var j = k
            while (j >= 1) {
                l[j] = max(g[j - 1] + max(0, diff), l[j] + diff)
                g[j] = max(l[j], g[j])
                j -= 1
            }
        }
        return g[k]
    }
    func solveMaxProfit(_ prices: [Int]) -> Int {
        var res = 0
        for i in 1..<prices.count {
            if prices[i] - prices[i - 1] > 0 {
                res += prices[i] - prices[i - 1]
            }
        }
        return res
    }
}
```
### 相关题目
* [Best Time to Buy and Sell Stock with Cooldown](https://keshiim.github.io/2017/11/24/Leetcode-309-Best-Time-to-Buy-and-Sell-Stock-with-Cooldown/)
* [Best Time to Buy and Sell Stock](https://keshiim.github.io/2017/11/24/Leetcode-121-Best-Time-to-Buy-and-Sell-Stock/)
* [Best Time to Buy and Sell Stock III](https://keshiim.github.io/2017/11/24/Leetcode-123-Best-Time-to-Buy-and-Sell-Stock-III/)
* [Best Time to Buy and Sell Stock II](https://keshiim.github.io/2017/11/24/Leetcode-122-Best-Time-to-Buy-and-Sell-Stock-II/)


