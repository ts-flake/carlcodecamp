---
status: ✅完成
start: 2025-06-10
due: 2025-06-10
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
[337.打家劫舍III | 力扣题目](https://leetcode.cn/problems/house-robber-iii/)

还是 [[09-动态规划-打家劫舍]] 问题, 这回房屋排列类似一颗 [[06-二叉树]]. 如果**两个直接相连的房子在同一天晚上被打劫**, 房屋将自动报警.
## 初步解答
一看到二叉树就想着递归遍历, 然后用 [[09-动态规划-打家劫舍]] 的套路做. ❌这就太着急了, 没有把握住本题的**相邻关系**是二叉树, 而非之前的一维数组 (两者的拓扑不一样)!!

所以二叉树→数组的法子不太行.
## 参考解答
**暴力法**: 递归判断, **偷与不偷父节点**. 注意这里要用**后序遍历**, 因为判断时要用到左右子树的结果.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: TreeNode) -> int:
        if root is None:
            return 0
        if root.left is None and root.right  is None:
            return root.val
        # 偷父节点
        val1 = root.val
        if root.left:
            val1 += self.rob(root.left.left) + self.rob(root.left.right)
        if root.right:
            val1 += self.rob(root.right.left) + self.rob(root.right.right)
        # 不偷父节点
        val2 = self.rob(root.left) + self.rob(root.right)
        return max(val1, val2)
```

**记忆化递归**:暴力法会超时, 原因是递归时有大量的重复计算 (这在递归法里是常常有的问题), 把中间结果记录下来, 避免重复计算. 例如, 用一个 [[03-哈希表]] (dict).
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    memory = {}
    def rob(self, root: TreeNode) -> int:
        if root is None:
            return 0
        if root.left is None and root.right  is None:
            return root.val
        if self.memory.get(root) is not None:
            return self.memory[root]
        # 偷父节点
        val1 = root.val
        if root.left:
            val1 += self.rob(root.left.left) + self.rob(root.left.right)
        if root.right:
            val1 += self.rob(root.right.left) + self.rob(root.right.right)
        # 不偷父节点
        val2 = self.rob(root.left) + self.rob(root.right)
        self.memory[root] = max(val1, val2)
        return max(val1, val2)
```

✅**树形动态规划**: 就是在树上运用**递推公式**. 时间复杂度 O(n).
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        # dp数组（dp table）以及下标的含义：
        # 1. 下标为 0 记录 **不偷该节点** 所得到的的最大金钱
        # 2. 下标为 1 记录 **偷该节点** 所得到的的最大金钱
        dp = self.traversal(root)
        return max(dp)

    # 要用后序遍历, 因为要通过递归函数的返回值来做下一步计算
    def traversal(self, node):
        
        # 递归终止条件，就是遇到了空节点，那肯定是不偷的
        if not node:
            return (0, 0)

        left = self.traversal(node.left)
        right = self.traversal(node.right)

        # 不偷当前节点, 偷子节点
        val_0 = max(left[0], left[1]) + max(right[0], right[1])

        # 偷当前节点, 不偷子节点
        val_1 = node.val + left[0] + right[0]

        return (val_0, val_1)
```


## 心得

---
## links


## reference
