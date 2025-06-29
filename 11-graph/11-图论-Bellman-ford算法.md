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
recommend: 4
priority: 1
authors:
  - Donghan
dg-publish: false
---
## 题目
[94.城市间货物运输I | 卡码题目](https://programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I.html)

本题其实就是 [[11-图论-Dijkstra朴素版]]边权重允许有负数的情况.

## 初步解答
看解答.
## 参考解答
✅Bellman-ford 算法解决的问题:
- 有向图, 权重有负数
- 单个起源到多个终点的最短路径/距离

✅Bellman-ford 算法关键:
- **edge relaxation**, 即遍历一次所有的边, 更新 `minDist[t] = min(minDist[t], minDist[s]+val)`, 其中 `s->|val|t`
- **松弛一次** (也就是一次局部最优) 得到的是由**单源通过一条边连通**的最短距离; 松弛 n-1 次得到的就是由单源通过 n-1 条边连通的最短距离
- 其实 Bellman-ford 算法就是 [[09-动态规划]]思想, 这里的 `minDist + 多次松弛过程` 就是把二维 dp 遍历的一维化的结果 (类似的技巧我们在 [[09-动态规划]]里见过很多次了)

![[graph_bellman_ford.png|400]]


```python
def main():
    n, m = map(int, input().strip().split())
    edges = []
    for _ in range(m):
        src, dest, weight = map(int, input().strip().split())
        edges.append([src, dest, weight])
    
    minDist = [float("inf")] * (n + 1)
    minDist[1] = 0  # 起点处距离为0
    
    for i in range(1, n):
        updated = False
        for src, dest, weight in edges: # ✅这样写的一次松弛没有更新所有边, 这不影响算法
            if minDist[src] != float("inf") and minDist[src] + weight < minDist[dest]:
                minDist[dest] = minDist[src] + weight
                updated = True
        if not updated:  # ✅若边不再更新，即停止回圈
            break
    
    if minDist[-1] == float("inf"):  # 返还终点权重
        return "unconnected"
    return minDist[-1]
    
if __name__ == "__main__":
    print(main())
```


## 心得

---
## links


## reference
