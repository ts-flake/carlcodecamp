---
status: ✅完成
start: 2025-06-21
due: 2025-06-21
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
[53.寻宝 | 卡码题目](https://kamacoder.com/problempage.php?pid=1053)

输入:
第一行包含两个整数 V 和 E, V 代表顶点数, E 代表边数. 顶点编号是从 1 到 V. 例如: V=2, 一个有两个顶点, 分别是 1 和 2. 接下来共有 E 行, 每行三个整数 v1, v2 和 val, v1 和 v2 为边的起点和终点, val代表边的权值.

输出:
输出**联通所有岛屿**的**最小路径**总距离.
![[graph_prim.png|400]]
## 初步解答
看解答.

## 参考解答
本题是**最小生成树的模版**. ✅树是一种特殊的图, 特点有**每个节点只有一个父节点** (入度 1, 除根节点外), 且**没有环**. 此外, **树是连通图**.

✅最小生成树就是**最小的连通所有节点的图**, 此处最小含义是**边的权重之和最小**.

在最小生成树里有一个数组 `minDist` 用于记录**节点到生成树的最小距离**. Prim 算法本质是贪心算法, 即每次**将节点以最小 (距离最近) 的方式加入到生成树内**.

🚨Prim 算法 (最小生成树) 的前提是**连通图**, 即最小生成树是原有连通图的子图.

体会一下 `minDist` 的更新过程:
1. 先将 1 号节点加入生成树, `minDist` 更新了相连节点到生成树的距离
2. 选择最近的节点, 例如 2 号加入生成树, `minDist` 更新了相连节点到生成树的距离 (只更新和 2 号节点相关的)
3. 依次类推, 直到所有节点都加入生成树
4. 最后得到的 `minDist` 记录的就是各边的最小权重
![[graph_prim2.png|330]] ![[graph_prim3.png|330]]

时间复杂度 O(n^2).
```python
# 接收输入
v, e = list(map(int, input().strip().split()))
# 按照常规的邻接矩阵存储图信息，不可达的初始化为10001
graph = [[10001] * (v+1) for _ in range(v+1)] # 节点从 1 开始编号
for _ in range(e):
    x, y, w = list(map(int, input().strip().split()))
    graph[x][y] = w # 记录边的权重
    graph[y][x] = w # 无向图, 对称的

# 定义加入生成树的标记数组和未加入生成树的最近距离
visited = [False] * (v + 1)
minDist = [10001] * (v + 1)

# 循环 n - 1 次，建立 n - 1 条边
# 从节点视角来看：每次选中一个节点加入树，更新剩余的节点到树的最短距离，
# 这一步其实蕴含了确定下一条选取的边，计入总路程 ans 的计算
for _ in range(1, v + 1):
    min_val = 10002
    cur = -1
    # 选取下一个节点
    for j in range(1, v + 1):
        if visited[j] == False and minDist[j] < min_val:
            cur = j
            min_val = minDist[j]
    visited[cur] = True # 加入生成树
    # 更新 minDist
    for j in range(1, v + 1):
        if visited[j] == False and minDist[j] > graph[cur][j]:
            minDist[j] = graph[cur][j]

ans = 0
for i in range(2, v + 1): # 从下标 2 开始累加
    ans += minDist[i]
print(ans)
```



## 心得

---
## links


## reference
