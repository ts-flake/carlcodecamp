---
status: ✅完成
start: 2025-06-23
due: 2025-06-23
topic:
  - 算法
  - 代码随想录
tags:
  - project/代码随想录算法营
  - 计算机/算法
recommend: 4
priority: 1
authors:
  - Donghan
dg-publish: false
---
## 题目
[47.参加科学大会 | 卡码题目](https://kamacoder.com/problempage.php?pid=1047)

输入:
第一行包含两个正整数, 第一个正整数 N 表示一共有 N 个公共汽车站, 第二个正整数 M 表示有 M 条公路. 接下来为 M 行, 每行包括三个整数, S, E 和 V, 代表了从 S 车站可以单向直达 E 车站, 并且需要花费 V 单位的时间.

输出:
输出一个整数, 代表小明从起点到终点所花费的最小时间.

下图左黑色是一条最短路径; 下图右的终点无法到达.
![[graph_dijkstra1.png|300]] ![[graph_dijkstra2.png|300]]
## 初步解答
**Dijkstra 算法**是**图路径搜索**的经典算法, 之前已经接触过很多次了. 这次再系统性回顾一下.

✅Dijkstra 算法的输入:
- 有向图, 有权图 (权重非负)
- 起始节点 (单个)
- 终点节点 (可以多个, **`minDist` 更新完后记录的是从单源出发到每一个节点的最短路径**)

✅Dijkstra 算法的输出: 
- 一条连接起始和终点的**最短 (边的权重) 路径**

✅其实 Dijkstra 算法也是**贪心**, 每一步都是在找**离起始点最近的节点**.

✅Dijkstra 和 Prim 的对比:
- (相似) 两者都是贪心, 都有 `minDist`
- (不同) Prim 寻找最小生成树, 每次添加**离生成树最近的节点**, 没有连贯路径; Dijkstra 寻找最短路径, 每次添加**离起始点/路径最近的节点**, 路径连贯

🚨注意 **Dijkstra 不能用于权重有负数**的情况, 因为没一步更新本质是贪心, 所以只有在权重非负的假设下, 一步贪心局部最优才能得到全局最优. 例如下图, `1->2->3` 的那条路线会因为贪心而被忽略 (`3` 已经访问过, 无法更新 `minDist[3]=-200`)

![[graph_dijkstra3.png|300]]

下面的算法里图采用**邻接表**来表示, 适合于稀疏图表示. 外层 while 和内层 for, 加起来时间复杂度是 O(n^2).
```python

def dijkstra(s, e, graph):
    n = len(graph)
    minDist = [float('inf')]*n
    visited = [False]*n
    
    cur = s
    minDist[cur] = 0

    while cur: # O(n)
        visited[cur] = True
        # 更新 minDist, 更新未访问的节点离起始的距离
        vertices = graph[cur]
        for v in vertices:
            if not visited[v[0]]:
                minDist[v[0]] = min(minDist[v[0]], minDist[cur]+v[1])
        # 寻找下一个节点, 在所有非访问节点里找离起始最近的节点
        cost = float('inf')
        cur = None
        for i in range(1, n): # O(n)
            if not visited[i] and minDist[i] < cost:
                cur = i
                cost = minDist[i]
    return minDist[e]

if __name__ == '__main__':
    n,m = map(int, input().split())
    graph = [[] for _ in range(n+1)] # 节点从 1 开始编号
    for _ in range(m):
        s,e,v = map(int, input().split())
        graph[s].append([e,v]) # 邻接表
    cost = dijkstra(1, n, graph)
    print(cost if cost != float('inf') else -1)

```

## 参考解答
✅Dijkstra 三部曲:
1. 选源点到哪个节点近且该节点未被访问过
2. 该最近节点被标记访问过
3. 更新非访问节点到源点的距离 (即更新 `minDist` 数组)

下面的算法里图采用**邻接矩阵**来表示.
```python
import sys

def dijkstra(n, m, edges, start, end):
    # 初始化邻接矩阵
    grid = [[float('inf')] * (n + 1) for _ in range(n + 1)]
    for p1, p2, val in edges:
        grid[p1][p2] = val

    # 初始化距离数组和访问数组
    minDist = [float('inf')] * (n + 1)
    visited = [False] * (n + 1)

    minDist[start] = 0  # 起始点到自身的距离为0

    for _ in range(1, n + 1):  # 遍历所有节点
        minVal = float('inf')
        cur = -1

        # 选择距离源点最近且未访问过的节点
        for v in range(1, n + 1):
            if not visited[v] and minDist[v] < minVal:
                minVal = minDist[v]
                cur = v

        if cur == -1:  # 如果找不到未访问过的节点，提前结束
            break

        visited[cur] = True  # 标记该节点已被访问

        # 更新未访问节点到源点的距离
        for v in range(1, n + 1):
            if not visited[v] and grid[cur][v] != float('inf') and minDist[cur] + grid[cur][v] < minDist[v]:
                minDist[v] = minDist[cur] + grid[cur][v]

    return -1 if minDist[end] == float('inf') else minDist[end]

if __name__ == "__main__":
    input = sys.stdin.read
    data = input().split()
    n, m = int(data[0]), int(data[1])
    edges = []
    index = 2
    for _ in range(m):
        p1 = int(data[index])
        p2 = int(data[index + 1])
        val = int(data[index + 2])
        edges.append((p1, p2, val))
        index += 3
    start = 1  # 起点
    end = n    # 终点

    result = dijkstra(n, m, edges, start, end)
    print(result)
```

## 心得

---
## links


## reference
