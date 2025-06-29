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

和 [[11-图论-最小生成树之Prim]] 题目一样, 本篇介绍 最小生成树的 Kruskal 算法.

## 初步解答
看解答.

## 参考解答
✅Prim 算法是**维护节点的集合**, 而 Kruskal 是**维护边的集合**. Kruskal 同样是贪心的, 我们先根据权重将边排序 (复杂度为 O(nlog n)), 然后不断取最小的边, 如果边的两个节点不在生成树内则加入, 否则不加入. 换言之, 若加入该边生成树有环则不加. 判断有无环就用到之前的 [[11-图论#并查集]] (复杂度为 O(log n), 这里 n 是边的个数).

🚨整体复杂度 O(n log n), 这相对 [[11-图论-最小生成树之Prim]] 有提升? 注意, **Kruskal 是对边遍历**, 而 **Prim 是对节点遍历**. 两个算法的输入都是一副连通图, 假设图有 N 个节点, 则生成树有 N-1 条边. 又因为生成树是原图的子图, 原始的边个数 M 大于 N-1.

✅所以选 Prim 还是 Kruskal 得看原图是节点多还是边多. **稠密图 (边远大于节点个数) 推荐 Prim; 稀疏图推荐用 Kruskal.**
```python
class Edge:
    def __init__(self, l, r, val):
        self.l = l
        self.r = r
        self.val = val

# 并查集
n = 10001
father = list(range(n))

def init():
    global father
    father = list(range(n))

def find(u):
    if u != father[u]:
        father[u] = find(father[u])
    return father[u]

def join(u, v):
    u = find(u)
    v = find(v)
    if u != v:
        father[v] = u

def kruskal(v, edges):
    edges.sort(key=lambda edge: edge.val)
    init()
    result_val = 0

    for edge in edges:
        x = find(edge.l)
        y = find(edge.r)
        if x != y:
            result_val += edge.val
            join(x, y)

    return result_val

if __name__ == "__main__":
    import sys
    input = sys.stdin.read
    data = input().split() # 一次性读入全部

    v = int(data[0])
    e = int(data[1])

    edges = []
    index = 2
    for _ in range(e):
        v1 = int(data[index])
        v2 = int(data[index + 1])
        val = int(data[index + 2])
        edges.append(Edge(v1, v2, val))
        index += 3

    result_val = kruskal(v, edges)
    print(result_val)
```



## 心得

---
## links


## reference
