---
title: Leetcode-122 Best Time to Buy and Sell Stock II
date: 2017-11-24 11:32:51
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---

[Best Time to Buy and Sell Stock II 买股票的最佳时间之二](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

### 题目描述
Say you have an array for which the $i^{th}$  element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transaction as you like(ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time(ie, you must sell the stock before you buy again).
### Example
    input [3, 2, 3, 5, 3, 6]
    output 6
    不是 6-2=4
    而是(3-2)+(5-3)+(6-3)=6
### 分析
这道题跟之前[Best Time to Buy and Sell Stock](https://keshiim.github.io/2017/11/24/Leetcode-121-Best-Time-to-Buy-and-Sell-Stock/)类似，这道题由于可以无限次数的买入和卖出，并且要求每次买入之前，必须先卖掉上次买入的股票。要想利润最大化当然是低价买入高价抛出。
### 解法
这里我们只需要从第二天开始，如果当天价格比之前价格高，则把插值加入利润中，因为我们可以昨天买入，今天卖出，若明日价格更高的话，可以今日买入，明日卖出。一次类推，遍历整个数组后即可求得最大利润。代码如下：
#### C++

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0, n = prices.size();
        for (int i = 0; i < n - 1; i++) {
            if (prices[i] < prices[i + 1]) {
                res += prices[i + 1] - prices[i];
            }
        }
        return res;
    }
};
```

#### Java

```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxprofit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1])
                maxprofit += prices[i] - prices[i - 1];
        }
        return maxprofit;
    }
}
```
#### Swift

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var res = 0, i = 0
        let n = prices.count
        while i < n - 1 {
            if prices[i] < prices[i + 1] {
               res += prices[i + 1] - prices[i]
            }
            i += 1
        }
        return res
    }
}
```

### 相关题目：

* [Best Time to Buy and Sell Stock with Cooldown](https://keshiim.github.io/2017/11/24/Leetcode-309-Best-Time-to-Buy-and-Sell-Stock-with-Cooldown/)
* [Best Time to Buy and Sell Stock IV](https://keshiim.github.io/2017/11/24/Leetcode-188-Best-Time-to-Buy-and-Sell-Stock-IV/)
* [Best Time to Buy and Sell Stock III](https://keshiim.github.io/2017/11/24/Leetcode-123-Best-Time-to-Buy-and-Sell-Stock-III/)
* [Best Time to Buy and Sell Stock](https://keshiim.github.io/2017/11/24/Leetcode-121-Best-Time-to-Buy-and-Sell-Stock/)


