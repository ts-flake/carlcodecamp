---
status: ✅完成
start: 2025-06-26
due: 2025-06-26
topic:
  - 算法
  - 代码随想录
tags:
  - project/代码随想录算法营
  - 计算机/算法
recommend: 3
priority: 1
authors:
  - Donghan
dg-publish: false
---
## 题目
[97.小明逛公园 | 卡码题目](https://kamacoder.com/problempage.php?pid=1155)

输入:
第一行包含两个整数 N, M, 分别表示景点的数量和道路的数量.
接下来的 M 行, 每行包含三个整数 u, v, w, 表示景点 u 和景点 v 之间有一条**长度**为 w 的**双向**道路.
接下里的一行包含一个整数 Q, 表示观景计划的数量.
接下来的 Q 行, 每行包含两个整数 start, end, 表示一个观景计划的起点和终点.

输出:
对于每个观景计划, 输出一行表示从起点到终点的最短路径长度. 如果两个景点之间不存在路径, 则输出 -1.

## 初步解答
看解答.

## 参考解答
对比之前的题目 ([[11-图论-Dijkstra朴素版]]和 [[11-图论-Bellman-ford算法]]):
- 本题是**多源**最短路径 (关注点在源/起始点, 因为单源问题一次能得到所有终点的最短路径)
- 本题假设非负权重
- 无向图

✅**Floyd 算法**:
- **多源**最短路径
- 边权重**可正可负**
- 核心是**动态规划**
- 适合于**稠密图且源点多**的情况 (源点少的话可以多次调用 Dijkstra 或 Bellman-ford)

✅**动态规划**:
- `dp[i][j][k]` 表示从节点`i`到节点`j`经过`[1...k]`中**某一个**节点的最短路径
- 迭代公式 `dp[i][j][k] = min(dp[i][k][k-1]+dp[k][j][k-1], dp[i][j][k-1])`, 即 `dp[i][j][k]`的最短路径要么经过`k`节点 (`dp[i][k][k-1]+dp[k][j][k-1]`), 要么不经过 (`dp[i][j][k-1]`)
- 遍历顺序, `k` 依赖于`k-1`, 所以最外层遍历`k`, 内层`i`和`j`遍历顺序无所谓

❓为什么递推公式里只考虑`[1...k-1]`而不考虑`[k+1...n]`? 首先, 因为后者在遍历`k`的时候就包含进去了; 再者, 只有这样才能遍历, 需要从已知量得到新的值.

**三维 dp** 版本: 时间复杂度 O(n^3), 空间复杂度 O(n^3), n 是节点数.
```python
if __name__ == '__main__':
    max_int = 10005  # 设置最大路径，因为边最大距离为10^4

    n, m = map(int, input().split())

    grid = [[[max_int] * (n+1) for _ in range(n+1)] for _ in range(n+1)]  # 初始化三维dp数组, 第 0 层

    for _ in range(m):
        p1, p2, w = map(int, input().split())
        grid[p1][p2][0] = w # 双向图
        grid[p2][p1][0] = w

    # 开始floyd
    for k in range(1, n+1): # 一层一层遍历
        for i in range(1, n+1):
            for j in range(1, n+1):
                grid[i][j][k] = min(grid[i][j][k-1], grid[i][k][k-1] + grid[k][j][k-1])

    # 输出结果
    z = int(input())
    for _ in range(z):
        start, end = map(int, input().split())
        if grid[start][end][n] == max_int:
            print(-1)
        else:
            print(grid[start][end][n]) # 最后一层
```

**二维 dp** 版本:  时间复杂度 O(n^3), 空间复杂度 O(n^2), n 是节点数.
```python
if __name__ == '__main__':
    max_int = 10005  # 设置最大路径，因为边最大距离为10^4

    n, m = map(int, input().split())

    grid = [[max_int]*(n+1) for _ in range(n+1)]  # 初始化二维dp数组, 滚动覆盖每一层

    for _ in range(m):
        p1, p2, val = map(int, input().split())
        grid[p1][p2] = val # 双向图
        grid[p2][p1] = val

    # 开始floyd
    for k in range(1, n+1):
        for i in range(1, n+1):
            for j in range(1, n+1):
                grid[i][j] = min(grid[i][j], grid[i][k] + grid[k][j])

    # 输出结果
    z = int(input())
    for _ in range(z):
        start, end = map(int, input().split())
        if grid[start][end] == max_int:
            print(-1)
        else:
            print(grid[start][end])
```

## 心得

---
## links


## reference
