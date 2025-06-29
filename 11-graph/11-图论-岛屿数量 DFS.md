---
status: ✅完成
start: 2025-06-15
due: 2025-06-15
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
[99.岛屿数量 | 卡码题目](https://kamacoder.com/problempage.php?pid=1171)

给定一个由 `1` (陆地) 和 `0` (水) 组成的矩阵, 你需要计算**岛屿的数量**. 岛屿由**水平方向或垂直方向**上相邻的陆地连接而成, 并且四周都是水域. 你可以假设矩阵外均被水包围.

![[graph_island.png|300]]
## 初步解答
🚨小心思维惯性, 本题的输入不是邻接矩阵, DFS/BFS 也不是只用在邻接矩阵上的 (别被 [[11-图论-所有可达路径]] 误导)!! 像这种 grid space 里的图搜索问题, 图的节点是显式给出来了 (即格点), 但图的边是隐含的 (事先默认的, 例如四连通或八连通).

```python
def dfs(i, j, graph):
	# 是陆地, 标记为到访过
    if graph[i][j] == 1:
        graph[i][j] = -1
    # 其他情况: 标记过, 不是陆地
    else:
        return
    # DFS 搜索上右下左方向的陆地
    for di,dj in [[-1,0],[0,1],[1,0],[0,-1]]:
        _i,_j = i+di,j+dj
        if _i < 0 or _i >= n or _j < 0 or _j >= m:
            continue
        # 递归
        dfs(_i, _j, graph)
    

if __name__ == '__main__':
	# 读取输入
	# 创建地图
    n,m = map(int, input().split())
    graph = []
    for _ in range(n):
        graph.append(list(map(int, input().split())))

	# 遇到一个陆地就记录一次, 然后使用 DFS 来标记相邻 (上下左右) 的陆地
    count = 0
    for i in range(n):
        for j in range(m):
            if graph[i][j] == 1: # 这里的判断可省略
                count += 1
                dfs(i, j, graph)
    print(count)
```

## 参考解答
参考解答几处注意点:
- 不修改原始地图, 用 `visited` 单独记录到访情况
- `dfs` 函数用了递归

```python
direction = [[0, 1], [1, 0], [0, -1], [-1, 0]]  # 四个方向：上、右、下、左


def dfs(grid, visited, x, y):
    """
    对一块陆地进行深度优先遍历并标记
    """
    for i, j in direction:
        next_x = x + i
        next_y = y + j
        # 下标越界，跳过
        if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
            continue
        # 未访问的陆地，标记并调用深度优先搜索
        if not visited[next_x][next_y] and grid[next_x][next_y] == 1:
            visited[next_x][next_y] = True
            dfs(grid, visited, next_x, next_y)


if __name__ == '__main__':  
    # 版本一
    n, m = map(int, input().split())
    
    # 邻接矩阵
    grid = []
    for i in range(n):
        grid.append(list(map(int, input().split())))
    
    # 访问表
    visited = [[False] * m for _ in range(n)]
    
    res = 0
    for i in range(n):
        for j in range(m):
            # 判断：如果当前节点是陆地，res+1并标记访问该节点，使用深度搜索标记相邻陆地。
            if grid[i][j] == 1 and not visited[i][j]:
                res += 1
                visited[i][j] = True
                dfs(grid, visited, i, j)
    
    print(res)
```

## 心得

---
## links


## reference
