---
title: Leetcode-121 Best Time to Buy and Sell Stock
date: 2017-11-24 10:49:42
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---

[Best Time to Buy and Sell Stock 最佳倒卖股票](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)
### 题目描述
Say you have an array for which the $i^{th}$  element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction(ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

### Example 1
    Input: [7, 1, 5, 3, 6, 4]
    Output: 5
    
    max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)

### Example 2
    Input: [7, 6, 4, 3, 1]
    Output: 0
    
    In this case, no transaction is done, i.e. max profit = 0.
### 分析
假如说给了一个数组，每一个元素都当天股票交易的价格。
现在，如果只允许你完成至多一次交易(也就是，买入一次股票，卖出一次股票)，设计一个可以利润最大化的算法。
### 解法
只需要遍历一遍数组，用一个变量记录遍历过程中的最小值，然后每次计算当前值和这个最小值之间的差值最利润，然后每次选较大利润来更新。当遍历完成后当前利润就是最大利润。代码如下：
### C++

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0, buy = INT_MAX;
        for (int price: prices) {
            buy = min(buy, price);
            res = max(res, price - buy);
        }
        return res;
    }
};
```
#### Swift

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var res = 0, buy = Int.max
        for price in prices {
            buy = min(buy, price)
            res = max(res, price - buy)
        }
        return res
    }
}
```

#### Java

```java
class Solution {
    public int maxProfit(int[] prices) {
        int res = 0, buy = Integer.MAX_VALUE;
        for (int price : prices) {
            buy = Math.min(buy, price);
            res = Math.max(res, price - buy);
        }
        return res;
    }
}
```

### 相关题目

* [Best Time to Buy and Sell Stock with Cooldown](https://keshiim.github.io/2017/11/24/Leetcode-309-Best-Time-to-Buy-and-Sell-Stock-with-Cooldown/)
* [Best Time to Buy and Sell Stock IV](https://keshiim.github.io/2017/11/24/Leetcode-188-Best-Time-to-Buy-and-Sell-Stock-IV/)
* [Best Time to Buy and Sell Stock III](https://keshiim.github.io/2017/11/24/Leetcode-123-Best-Time-to-Buy-and-Sell-Stock-III/)
* [Best Time to Buy and Sell Stock II](https://keshiim.github.io/2017/11/24/Leetcode-122-Best-Time-to-Buy-and-Sell-Stock-II/)

