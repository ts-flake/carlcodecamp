---
status: ✅完成
start: 2025-06-10
due: 2025-06-10
topic:
  - 算法
  - 代码随想录
tags:
  - project/代码随想录算法营
  - 计算机/算法
recommend: 1
priority: 1
authors:
  - Donghan
dg-publish: false
---
## 题目
[123.买卖股票的最佳时机III | 力扣题目](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

给定一个数组, 它的第 `i` 个元素是一支给定的股票在第 `i` 天的价格. 设计一个算法来计算你所能获取的最大利润. 你最多可以完成**两笔**交易.

**注意**: 你不能同时参与多笔交易 (你必须在再次购买前出售掉之前的股票).
## 初步解答
看解答.

## 参考解答
`dp[i][j]` 的含义类似 [[09-动态规划-买卖股票的最佳时机]], 现在每天有多个状态. 其中"不操作"状态其实可以省略.

递推公式其实看做是 [[09-动态规划-买卖股票的最佳时机]] (只交易一次) 和 [[09-动态规划-买卖股票的最佳时机II]] (交易无限次) 的融合体. ❓怎么理解呢?
- 忽视第 0 列, 第 1,2 列其实就是只交易一次的写法, 即没有累积收入, 基准从零开始
- 第 3,4 列其实是交易无限多次的写法, 因为需要**累积第一次交易收入**

![[动态规划股票问题.excalidraw.svg|400]]
%%[[动态规划股票问题.excalidraw.md|🖋 Edit in Excalidraw]]%%

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [[0] * 5 for _ in range(len(prices))]
        dp[0][1] = -prices[0]
        dp[0][3] = -prices[0]
        for i in range(1, len(prices)):
            dp[i][0] = dp[i-1][0]
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]) # 第一次买入
            dp[i][2] = max(dp[i-1][2], dp[i-1][1] + prices[i]) # 第一次卖出
            dp[i][3] = max(dp[i-1][3], dp[i-1][2] - prices[i]) # 第二次买入
            dp[i][4] = max(dp[i-1][4], dp[i-1][3] + prices[i]) # 第二次卖出
        return dp[-1][4]
```
## 心得


---
## links


## reference
