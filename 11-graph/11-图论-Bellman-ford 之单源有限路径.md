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
[96.城市间货物运输III](https://kamacoder.com/problempage.php?pid=1154)

仍是 [[11-图论-Bellman-ford算法]]的问题, 现在加入一条 **(不等式) 约束**: 请计算在**最多经过 k 个城市**的条件下, 从城市 src 到城市 dst 的最低运输成本.

## 初步解答
看解答.
## 参考解答
✅把题目里的约束条件翻译一下:
- 最多经过`k`个节点, 即路径上的节点数 `<= k+2` (包含起始和终点)
- 最多经过`k`个节点意味着路径的边数 `k+1`, 即**最多松弛`k+1`次**

🚨下面的代码相较于 [[11-图论-Bellman-ford算法]]和[[11-图论-Bellman-ford之判断负权回路]]会发现多了一个`minDist_copy`. 该数组用于记录上一次松弛的值, 因为更新`minDist`是根据**上一次的值**而非最新的 (类似的情况在二维 dp 转到一维 dp 时也出现过).

🚨引入`minDist_copy`主要就是解决如下图中**负回环**的问题, 可以动手推一下, **对比用`minDist`覆盖更新和用`minDist_copy`更新的区别**. 特别关注回环里的值的变化:
- 用`minDist`覆盖更新依赖于**边的排序**
- 用`minDist`覆盖更新造成`k`次松弛的值*不等于*`<=k`内从起点到达所需最短开销
- 用`minDist_copy`更新不依赖于边的排序
- ✅用`minDist_copy`更新保证 **`k`次松弛的值等于`<=k`内从起点到达所需最短开销**

![[graph_bellman_ford2.png|300]]

把`minDist`每次更新 (采用`minDist_copy`) 的值写下来更清楚!!

| 松弛  | 0   | 1   | 2   | 3   | 4   |
| --- | --- | --- | --- | --- | --- |
| 1   | max | max | -1  | max | max |
| 2   | max | max | -1  | 0   | max |
| 3   | max | -1  | -1  | 0   | 1   |
| 4   | max | -1  | -2  | 0   | 1   |
| 5   | max | -1  | -2  | -1  | 1   |
| 6   | max | -2  | -2  | -1  | 0   |
| 7   | max | -2  | -3  | -1  | 0   |

❓为什么之前的代码里不用`minDist_copy`呢? 因为在 [[11-图论-Bellman-ford算法]]里没有负回环, 而 [[11-图论-Bellman-ford之判断负权回路]] 只用判断负回环 (且没有约束条件, 故无需担心结果的准确性).

❓最短路径用 [[11-图论-Dijkstra朴素版]] 可不可以? 之前也说过了, **Dijkstra 不能处理负权重**, 所以本题不能用. 另一个问题是本题有`k`步约束, **Dijkstra 同样无法处理约束, 其得到的是无约束下的最短路径.**

```python
def main():
    # 輸入
    n, m = map(int, input().split())
    edges = list()
    for _ in range(m):
        edges.append(list(map(int, input().split() )))
    
    start, end, k = map(int, input().split())
    min_dist = [float('inf') for _ in range(n + 1)]
    min_dist[start] = 0
    
    # 只能經過k個城市，所以從起始點到中間有(k + 1)個邊連接
    # 需要鬆弛(k + 1)次
    
    for _ in range(k + 1):
        update = False
        min_dist_copy = min_dist.copy()
        for src, desc, w in edges:
            if (min_dist_copy[src] != float('inf') and 
            min_dist_copy[src] + w < min_dist[desc]):
                min_dist[desc] = min_dist_copy[src] + w
                update = True
        if not update:
            break
    # 輸出
    if min_dist[end] == float('inf'):
        print('unreachable')
    else:
        print(min_dist[end])
            
            

if __name__ == "__main__":
    main()
```


## 心得

---
## links


## reference
