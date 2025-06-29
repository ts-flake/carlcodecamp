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
recommend: 2
priority: 1
authors:
  - Donghan
dg-publish: false
---
## 题目
[94.城市间货物运输I | 卡码题目](https://programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I.html)

继续 [[11-图论-Bellman-ford算法]], SPFA 就是**队列优化 (queue-improved)** Bellman-ford 算法.

## 初步解答
看解答.

## 参考解答
首先理解 SPFA 优化在哪? 回顾原始的 Bellman-ford 算法里, 每次松弛要遍历一遍所有的边, 而这里面只有部分是成立的 (有效的松弛要与源点相连通). 所以为了避免这些无用功, SPFA 采用队列来跟踪那些要被松弛的节点.

✅什么时候用 SPFA:
- SPFA 优化的精髓在于松弛时不做无用功, 即原本 Bellman-ford `n-1` 次松弛 (`n`个节点) 的节点转成由队列维护
- SPFA 适合**稀疏图**, 即边比节点少; 加入到队列的节点远远小于全部节点, 时间复杂度大概 O(nk), k 为节点的度的均值
- 在稠密图下 SPFA 效率和 Bellman-ford 一样

![[graph_spfa1.png|300]]  ![[graph_spfa2.png|300]]

```python
import collections

def main():
    n, m = map(int, input().strip().split())
    edges = [[] for _ in range(n + 1)]
    for _ in range(m):
        src, dest, weight = map(int, input().strip().split())
        edges[src].append([dest, weight])
    
    minDist = [float("inf")] * (n + 1)
    minDist[1] = 0
    que = collections.deque([1])
    visited = [False] * (n + 1)
    visited[1] = True
    
    while que:
        cur = que.popleft()
        # 从队列里取出的时候, 要取消标记
        # 我们只保证已经在队列里的元素不重复加入, 但允许重复访问回环
        # 🚨如果有负回环 (走一圈权重之和为负) 则会无限循环
        visited[cur] = False
        for dest, weight in edges[cur]:
            if minDist[cur] != float("inf") and minDist[cur] + weight < minDist[dest]:
                minDist[dest] = minDist[cur] + weight
                if visited[dest] == False: # 避免重复加入队列
                    que.append(dest)
                    visited[dest] = True
    
    if minDist[-1] == float("inf"):
        return "unconnected"
    return minDist[-1]

if __name__ == "__main__":
    print(main())
```


## 心得

---
## links


## reference
