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
recommend: 1
priority: 1
authors:
  - Donghan
dg-publish: false
---
## 题目
[95.城市间货物运输II](https://kamacoder.com/problempage.php?pid=1153)

仍然是 [[11-图论-Bellman-ford算法]]的题目, 但本题可能出现**负回环**的情况. 若检测到负回环, 输出 `circle`.

## 初步解答
看解答.

## 参考解答
✅在 Bellman-ford 的框架下检测负回环有如下方式:
- 原始 Bellman-ford 松弛 `>=n` 次 (`n`个节点) 后 `minDist` 仍变化, 说明有负回环 (因为负回环会无限循环)
- SPFA 队列优化下, 每个节点进入队列最多 `n-1` 次 (`n`个节点的全连双向图), 所以如果有节点进入队列`>=n`次则说明有负回环

🚨从上面的方法可以看出, 要检测负回环就必须松弛 `n` 次, 即无法提前结束.

**方法一**: Bellman-ford
```python
import sys

def main():
    input = sys.stdin.read
    data = input().split()
    index = 0
    
    n = int(data[index])
    index += 1
    m = int(data[index])
    index += 1
    
    grid = []
    for i in range(m):
        p1 = int(data[index])
        index += 1
        p2 = int(data[index])
        index += 1
        val = int(data[index])
        index += 1
        # p1 指向 p2，权值为 val
        grid.append([p1, p2, val])

    start = 1  # 起点
    end = n    # 终点

    minDist = [float('inf')] * (n + 1)
    minDist[start] = 0
    flag = False

    for i in range(1, n + 1):  # 这里我们松弛n次，最后一次判断负权回路
        for side in grid:
            from_node = side[0]
            to = side[1]
            price = side[2]
            if i < n: # 松弛 n-1 次
                if minDist[from_node] != float('inf') and minDist[to] > minDist[from_node] + price:
                    minDist[to] = minDist[from_node] + price
            else:  # 多加一次松弛判断负权回路
                if minDist[from_node] != float('inf') and minDist[to] > minDist[from_node] + price:
                    flag = True

    if flag:
        print("circle")
    elif minDist[end] == float('inf'):
        print("unconnected")
    else:
        print(minDist[end])

if __name__ == "__main__":
    main()
```

**方法二**: SPFA
```python
from collections import deque
from math import inf

def main():
    n, m = [int(i) for i in input().split()]
    graph = [[] for _ in range(n+1)]
    min_dist = [inf for _ in range(n+1)]
    count = [0 for _ in range(n+1)]  # 记录节点加入队列的次数
    for _ in range(m):
        s, t, v = [int(i) for i in input().split()]
        graph[s].append([t, v])
        
    min_dist[1] = 0  # 初始化
    count[1] = 1
    d = deque([1])
    flag = False
    
    while d:  # 主循环
        cur_node = d.popleft()
        for next_node, val in graph[cur_node]:
            if min_dist[next_node] > min_dist[cur_node] + val:
                min_dist[next_node] = min_dist[cur_node] + val
                count[next_node] += 1
                if next_node not in d:
                    d.append(next_node)
                if count[next_node] == n:  # 如果某个点松弛了n次，说明有负回路
                    flag = True
        if flag:
            break
            
    if flag:
        print("circle")
    else:
        if min_dist[-1] == inf:
            print("unconnected")
        else:
            print(min_dist[-1])


if __name__ == "__main__":
    main()
```


## 心得

---
## links


## reference
