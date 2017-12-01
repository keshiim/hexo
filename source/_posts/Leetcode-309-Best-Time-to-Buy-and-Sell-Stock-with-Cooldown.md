---
title: Leetcode-309 Best Time to Buy and Sell Stock with Cooldown
date: 2017-11-24 14:14:04
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Dynamic Programming
    - Medium
---
### 题目描述
Say you have an array for which the $i^{th}$ element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

* You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
* After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

### Example

```c++
prices = [1, 2, 3, 0, 2]
maxProfit = 3
transactions = [buy, sell, cooldown, buy, sell]
```

### 分析
而这道题与上面这些不同之处在于加入了一个冷冻期`Cooldown`之说，就是如果某天卖了股票，那么第二天不能买股票，有一天的冷冻期。此题需要维护三个一维数组`buy`, `sell`，和`rest`。其中：

* `buy[i]`表示在第`i`天之前最后一个操作是买，此时的最大收益。
* `sell[i]`表示在第`i`天之前最后一个操作是卖，此时的最大收益。
* `rest[i]`表示在第`i`天之前最后一个操作是冷冻期，此时的最大收益。

我们写出递推式为：

```c++
buy[i]  = max(rest[i-1] - price, buy[i-1]) 
sell[i] = max(buy[i-1] + price, sell[i-1])
rest[i] = max(sell[i-1], buy[i-1], rest[i-1])
```
上述递推式很好的表示了在买之前有冷冻期，买之前要卖掉之前的股票。**一个小技巧是如何保证`[buy, rest, buy]`的情况不会出现**，这是由于`buy[i] <= rest[i]`， 即`rest[i] = max(sell[i-1], rest[i-1])`，这保证了`[buy, rest, buy]`不会出现。

所以递推公式可以变成：

```c++
buy[i]  = max(rest[i-1] - price, buy[i-1]) 
sell[i] = max(buy[i-1] + price, sell[i-1])
rest[i] = max(sell[i-1], rest[i-1])
```
另外，由于冷冻期的存在，我们可以得出`rest[i] = sell[i-1]`，这样，我们可以将上面三个递推式精简到两个：

```c++
buy[i]  = max(sell[i-2] - price, buy[i-1]) 
sell[i] = max(buy[i-1] + price, sell[i-1])
```
我们还可以做进一步优化，由于`i`只依赖于`i-1`和`i-2`，所以我们可以在`O(1)`的空间复杂度完成算法，参见代码如下：
#### C++

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy = INT_MIN, pre_buy = 0, sell = 0, pre_sell = 0;
        for (int price : prices) {
            pre_buy = buy;
            buy = max(pre_sell - price, pre_buy);
            pre_sell = sell;
            sell = max(pre_buy + price, pre_sell);
        }
        return sell;
    }
};
```

#### Swift

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var pre_buy = 0, buy = Int.min, pre_sell = 0, sell = 0
        for price in prices {
            pre_buy = buy
            buy = max(pre_sell - price, pre_buy)
            pre_sell = sell
            sell = max(pre_buy + price, pre_sell)
        }
        return sell
    }
}
```
###另一种解法
[参考链接](http://blog.csdn.net/qingyujean/article/details/51482041)
### 相关题目：
* [Best Time to Buy and Sell Stock IV](https://keshiim.github.io/2017/11/24/Leetcode-188-Best-Time-to-Buy-and-Sell-Stock-IV/)
* [Best Time to Buy and Sell Stock III](https://keshiim.github.io/2017/11/24/Leetcode-123-Best-Time-to-Buy-and-Sell-Stock-III/)
* [Best Time to Buy and Sell Stock II](https://keshiim.github.io/2017/11/24/Leetcode-122-Best-Time-to-Buy-and-Sell-Stock-II/)
* [Best Time to Buy and Sell Stock](https://keshiim.github.io/2017/11/24/Leetcode-121-Best-Time-to-Buy-and-Sell-Stock/)


