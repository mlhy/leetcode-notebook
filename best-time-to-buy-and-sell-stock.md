---
description: LeetCode | Easy | Best Time to Buy and Sell Stock
---

# Best Time to Buy and Sell Stock

前置知识：数组、动态规划

**买卖股票的最佳时机**这一系列有 5 道题目，目前我完成了 121 和 122。

| 编号 | 题目 | 难度 |
| :---: | :--- | :--- |
|  121 | 买卖股票的最佳时机 | Easy |
| 122 | 买卖股票的最佳时机 II | Easy |
| 123 | 买卖股票的最佳时机 III | Hard |
| 188 | 买卖股票的最佳时机 IV | Hard |
| 309 | 买卖股票的最佳时机含冷冻期 | Medium |
| 714 | 买卖股票的最佳时机含手续费 | Medium |

### 题目：买卖股票的最佳时机

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

**Example 1：**

**Input:** \[7,1,5,3,6,4\] 

**Output:** 5 

**Explanation:** Buy on day 2 \(price = 1\) and sell on day 5 \(price = 6\), profit = 6-1 = 5.

**Note：**

你不能在买入股票前卖出股票。

### 算法思路

如果简单的使用循环来计算所有的可能性，那么算法的时间复杂度是 $$O(n^2)$$ 。一旦数据量增大会非常耗时，类似的算法问题的算法复杂度应当被优化到 $$O(n)$$ 。那么就需要使用**动态规划**（Dynamic Programming），避免每一次的计算都是从头开始，能够借助已有的计算结果来进行更新。

题目的假设比较简单，一天只能进行一次买或卖，且只有买了才能卖。那么我们只需要计算每一天可以得到的**最低的买入价**以及可操作的**最高卖出价**，计算它们的差值就可以得到每天可知的最高收入，然后取最大值即可。

以 **Example 1** 为例，输入是每一天的股票价格`[7,1,5,3,6,4]`，那么第一天只能以 7 买入，第二天可以以 1 买入，第三天的价格5高于第二天价格 1，所以我们是在第二天以 1 的价格买入，以此类推，**每一天的最优买入价是**`[7,1,1,1,1,1]`；买入价需要向第一天向后推算，那么卖出价需要从最后一天向前推算：在第六天只能以 4 卖出，在第五天可以以 6 卖出，第四天的价格为 3，但是可以等到第五天再卖，所以第四天的可操作的最高卖出价6，以此类推得到**每一天可操作的最高卖出价是** `[7,6,6,6,6,4]`。那么，**每一天可知的最大收益用最高卖出价减去最低买入价即可**：`[0,5,5,5,5,4]`，对每一天的最大收益取最大值，可以得到只进行一次买卖的整体最大收益。

| Day | 1 | 2 | 3 | 4 | 5 | 6 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **price** | 7 | 1 | 5 | 3 | 6 | 4 |
| **buying\_price** | **7** | 1 | 1 | 1 | 1 | 1 |
| **selling\_price** | 7 | 6 | 6 | 6 | 6 | 4 |
| **profit** | 0 | 5 | 5 | 5 | 5 | 3 |

```text
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        
        if n==0:
            return 0

        # 初始化 buying_prices、selling_prices
        buying_prices = prices.copy()
        selling_prices = prices.copy()
        
        # 计算每一天的最优买入价
        buying_price = prices[0]
        for i in range(n):
            if buying_price > prices[i]:
                buying_price = prices[i]
            buying_prices[i] = buying_price
            
        # 计算每一天可操作的最高卖出价
        selling_price = prices[n-1]
        for i in range(n-1,-1,-1):
            if selling_price < prices[i]:
                selling_price = prices[i]
            selling_prices[i] = selling_price
            
        max_profit = max([y - x for x, y in zip(buying_prices,selling_prices)])
        
        return max(max_profit, 0)
```



