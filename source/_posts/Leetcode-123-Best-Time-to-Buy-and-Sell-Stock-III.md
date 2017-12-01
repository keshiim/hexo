---
title: Leetcode-123 Best Time to Buy and Sell Stock III
date: 2017-11-24 14:06:46
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Dynamic Programming
    - Hard
---

[Best Time to Buy and Sell Stock III 买股票的最佳时间之三](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)
### 题目描述
Say you have an array for which the $i^{th}$ element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most *two Transactions*.

### Note
You may not engage in multiple transactions at the same time(ie, you must sell the stock before you buy again).

### 分析
这道题是买股票的最佳时间系列问题中最难最复杂的一道，前面两道[Best Time to Buy and Sell Stock II](https://keshiim.github.io/2017/11/24/Leetcode-122-Best-Time-to-Buy-and-Sell-Stock-II/)
和 [Best Time to Buy and Sell Stock](https://keshiim.github.io/2017/11/24/Leetcode-121-Best-Time-to-Buy-and-Sell-Stock/)的思路都非常的简明，算法也很简单。

而这道题是要求最多交易两次，找到最大利润，还是需要用动态规划（Dynamic Programming）来解。
### 解法一
这里我们需要两个地推公式来分别更新两个变量local和global，我们其实可以求至少`k`次交易的最大利润，找到通项公式后可以设定`k = 2`，即为本题的解答。我们定义`local[i][j]`为在到达第`i`天时最多可进行`j`次交易并且最后一次交易在最后一天卖出的最大利润，此为局部最优。然后我们定义`global[i][j]`为在到达第`i`天时最多可进行`j`次交易的最大利润，为此全局最优。他们的递推公式为：

```c
local[i][j] = max(global[i - 1][j - 1] + max(diff, 0), local[i - 1)[j] + diff);
global[i][j] = max(local[i][j], global[i - 1][j])
```
其中局部最优值是比较前一天并少交易一次的全局最优加上大于`0`的差值，和前一天的局部最优加上差值中取较大值，而全局最优值是比较局部最优和前一天的全局最优取较大值。

全局:
**到达第i天进行j次交易的最大收益 = max{局部（在第i天交易后，恰好满足j次交易），全局（到达第i-1天时已经满足j次交易）}**

局部：
**在第i天交易后，总共交易了j次 =  max{情况2，情况1}
情况1：在第i-1天时，恰好已经交易了j次（local[i-1][j]），那么如果i-1天到i天再交易一次：即在第i-1天买入，第i天卖出（diff），则这不并不会增加交易次数！【例如我在第一天买入，第二天卖出；然后第二天又买入，第三天再卖出的行为  和   第一天买入，第三天卖出  的效果是一样的，其实只进行了一次交易！因为有连续性】
情况2：第i-1天后，共交易了j-1次（global[i-1][j-1]），因此为了满足“第i天过后共进行了j次交易，且第i天必须进行交易”的条件：我们可以选择：在第i-1天买入，然后再第i天卖出（diff），或者选择在第i天买入，然后同样在第i天卖出（收益为0）。**

代码如下：
#### C++

```c++
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        if (prices.empty()) return 0;
        int n = prices.size(), g[n][3] = {0}, l[n][3] = {0};
        for (int i = 1; i < prices.size(); i++) {
            int diff = prices[i] - prices[i - 1];
            for (int j = 1; j <= 2; j++) {
                l[i][j] = max(g[i - 1][j - 1] + max(diff, 0), l[i - 1][j] + diff);
                g[i][j] = max(l[i][j], g[i - 1][j]);
            }
        }
        return g[n - 1][2];
    }
}
```
#### Swift

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        if prices.count == 0 {
            return 0
        }
        let n = prices.count
        var g = [[Int]](repeating: [Int](repeatElement(0, count: 3)), count: n)
        var l = [[Int]](repeating: [Int](repeatElement(0, count: 3)), count: n)
        for i in 1..<n {
            let diff = prices[i] - prices[i - 1]
            for j in 1...2 {
                l[i][j] = max(g[i - 1][j - 1] + max(diff, 0), l[i - 1][j] + diff)
                g[i][j] = max(l[i][j], g[i - 1][j])
            }
        }
        return g[n - 1][2]
    }
}
```
#### 提升
```c++
/*节省空间版本：
下面这种解法用一维数组来代替二维数组，
可以极大的节省了空间，由于覆盖的顺序关
系，我们需要j从2到1，这样可以取到正确
的g[j-1]值，而非已经被覆盖过的值，参见代码如下：
*/
class Solution {
public:
    int maxProfit(vector<int> &prices) {
        if (prices.empty()) return 0;
        int g[3] = {0};
        int l[3] = {0};
        for (int i = 0; i < prices.size() - 1; ++i) {
            int diff = prices[i + 1] - prices[i];
            for (int j = 2; j >= 1; --j) {
                l[j] = max(g[j - 1] + max(diff, 0), l[j] + diff);
                g[j] = max(l[j], g[j]);
            }
        }
        return g[2];
    }
};
```
### 解法二
也是用DP，设置四个变量，`buy1、buy2、sell1、sell2`分别记录第一次购买，第二次购买和第一次卖出，第二次卖出。
因为题目要求只能在这个循序下`（buy1 —> sell1 -> buy2 -> sell2）`完成交易。所以遍历一遍数组，每一次买入的时候尽可能花钱代价最小，每次卖出时尽可能高价卖出，代码如下。
#### C++

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy1 = INT_MIN, buy2 = INT_MIN;
        int sell1 = 0, sell2 = 0;
        for (int price: prices) {
            sell2 = max(sell2, buy2 + price);
            buy2 = max(buy2, sell1 - price);
            sell1 = max(sell1, buy1 + price);
            buy1 = max(buy1, -price);
        }
        return sell2;
    }
};
```
#### Swift

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        if prices.count == 0 {
            return 0
        }
        var buy1 = Int.min, buy2 = Int.min
        var  sell1 = 0, sell2 = 0
        for price in prices {
            sell2 = max(sell2, buy2 + price)
            buy2  = max(buy2, sell1 - price)
            sell1 = max(sell1, buy1 + price)
            buy1  = max(buy1, -price)
        }
        return sell2;
    }
}
```
### 参考链接
[http://blog.csdn.net/fightforyourdream/article/details/14503469](http://blog.csdn.net/fightforyourdream/article/details/14503469)
### 相关题目

* [Best Time to Buy and Sell Stock with Cooldown](https://keshiim.github.io/2017/11/24/Leetcode-309-Best-Time-to-Buy-and-Sell-Stock-with-Cooldown/)
* [Best Time to Buy and Sell Stock IV](https://keshiim.github.io/2017/11/24/Leetcode-188-Best-Time-to-Buy-and-Sell-Stock-IV/)
* [Best Time to Buy and Sell Stock II](https://keshiim.github.io/2017/11/24/Leetcode-122-Best-Time-to-Buy-and-Sell-Stock-II/)
* [Best Time to Buy and Sell Stock](https://keshiim.github.io/2017/11/24/Leetcode-121-Best-Time-to-Buy-and-Sell-Stock/)

