---
status: ✅完成
start: 2025-06-24
due: 2025-06-24
topic:
  - 算法
  - 代码随想录
tags:
  - project/代码随想录算法营
  - 计算机/算法
recommend: 2
priority: 1
authors:
  - Donghan
dg-publish: false
---
## 题目
[47.参加科学大会 | 卡码题目](https://kamacoder.com/problempage.php?pid=1047)

在 [[11-图论-Dijkstra朴素版]]的基础上做优化.
## 初步解答
看解答.

## 参考解答
这里对 Dijkstra 的优化类似于 [[11-图论-最小生成树之Prim]] 和 [[11-图论-最小生成树之Kruskal]] 的讨论, 都是依据问题里的图是稀疏的 (边比节点少), 还是稠密的 (边比节点多).

[[11-图论-Dijkstra朴素版]]是在节点上遍历, 时间复杂度大概是 O(n^2), 这里 n 是节点数.

我们同样可以从边上遍历. 因为算法每次要添加一条到路径最近的边, 我们可以用一个**小顶堆**来维护边, 这样每次从堆顶弹出的就是最小边.

此外, 对于稀疏图, 采用邻接表会比邻接矩阵更高效.

✅Dijkstra 在稀疏图上的优化:
- 采用**邻接表**, 空间复杂度是 O(n+m), 这里 n 是节点数, m 是边的个数
- 用**小顶堆**遍历边, 时间复杂度 O(mlog m), 这里 m 是边的个数

```python
import heapq

class Edge:
    def __init__(self, to, val):
        self.to = to
        self.val = val

def dijkstra(n, m, edges, start, end):
    grid = [[] for _ in range(n + 1)]

    for p1, p2, val in edges:
        grid[p1].append(Edge(p2, val))

    minDist = [float('inf')] * (n + 1)
    visited = [False] * (n + 1)

    pq = []
    heapq.heappush(pq, (0, start))
    minDist[start] = 0

    while pq:
        cur_dist, cur_node = heapq.heappop(pq)

        if visited[cur_node]:
            continue

        visited[cur_node] = True

        for edge in grid[cur_node]:
            if not visited[edge.to] and cur_dist + edge.val < minDist[edge.to]:
                minDist[edge.to] = cur_dist + edge.val
                heapq.heappush(pq, (minDist[edge.to], edge.to))

    return -1 if minDist[end] == float('inf') else minDist[end]

# 输入
n, m = map(int, input().split())
edges = [tuple(map(int, input().split())) for _ in range(m)]
start = 1  # 起点
end = n    # 终点

# 运行算法并输出结果
result = dijkstra(n, m, edges, start, end)
print(result)
```


## 心得

---
## links


## reference
