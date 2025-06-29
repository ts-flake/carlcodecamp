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

见 [[11-图论-岛屿数量 DFS]]

## 初步解答
```python
from collections import deque

def bfs(i, j, graph):
	# 是陆地, 标记为到访过
    if graph[i][j] == 1:
        graph[i][j] = -1
    # 其他情况: 标记过, 不是陆地
    else:
        return
    # BFS 将上右下左方向的格子加入到队列
    for di,dj in [[-1,0],[0,1],[1,0],[0,-1]]:
        _i,_j = i+di,j+dj
        if _i < 0 or _i >= n or _j < 0 or _j >= m:
            continue
        queue.append((_i, _j))
    # 依次从队列里弹出
    while queue:
        bfs(*queue.popleft(), graph)
    

if __name__ == '__main__':
	# 处理输入
	# 创建地图
    n,m = map(int, input().split())
    graph = []
    for _ in range(n):
        graph.append(list(map(int, input().split())))

    count = 0
    for i in range(n):
        for j in range(m):
            if graph[i][j] == 1: # 这里的判断可省略
                count += 1
                queue = deque([])
                bfs(i, j, graph)
    print(count)
    

```

## 参考解答
参考解答几处注意点:
- 不修改原始地图, 用 `visited` 单独记录到访情况
- `bfs` 函数没有用递归, 而是迭代遍历式的 (`while` 循环, 类似的参见[[06-二叉树-二叉树的迭代遍历]])

```python
from collections import deque
directions = [[0, 1], [1, 0], [0, -1], [-1, 0]]
def bfs(grid, visited, x, y):
    que = deque([])
    que.append([x,y])
    visited[x][y] = True
    while que:
        cur_x, cur_y = que.popleft()
        for i, j in directions:
            next_x = cur_x + i
            next_y = cur_y + j
            if next_y < 0 or next_x < 0 or next_x >= len(grid) or next_y >= len(grid[0]):
                continue
            # 只把相邻的陆地加入队列
            if not visited[next_x][next_y] and grid[next_x][next_y] == 1: 
                visited[next_x][next_y] = True
                que.append([next_x, next_y])


def main():
    n, m = map(int, input().split())
    grid = []
    for i in range(n):
        grid.append(list(map(int, input().split())))
    visited = [[False] * m for _ in range(n)]
    res = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1 and not visited[i][j]:
                res += 1
                bfs(grid, visited, i, j)
    print(res)

if __name__ == "__main__":
    main()

```

## 心得

---
## links


## reference
