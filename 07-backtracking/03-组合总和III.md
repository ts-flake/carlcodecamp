## 题目
[216.组合总和III | 力扣题目](https://leetcode.cn/problems/combination-sum-iii/description/)

只用数字 `1-9`, 找出所有相加之和为 `n` 的 `k` 个数的组合. 数字不能复用.

## 初步解答
本题只需在 [[07-回溯算法-组合]]的基础上修改终止条件. 因为**递归里藏着回溯**, 本题和 [[06-二叉树-路径总和]]是类似的.

还是先给自己的野路子代码:
```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []

        def backtracking(m, n, k, l: list):
            if k == 0: # 递归终止
                if sum(l) == n: res.append(l) # 判断数之和
                return

            for i in range(m, k-1, -1): # 剪枝优化, range: [k, k+1, ..., m]
                backtracking(i-1, n, k-1, l+[i])
        
        backtracking(9, n, k, [])
        return res
```

## 参考解答
来看看参考代码, 注意本题的**剪枝优化**相较于 [[07-回溯算法-组合问题]]可以更进一步.
```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        result = []  # 存放结果集
        self.backtracking(n, k, 0, 1, [], result)
        return result

    def backtracking(self, targetSum, k, currentSum, startIndex, path, result):
        if currentSum > targetSum:  # 剪枝操作 👈
            return  # 如果path的长度等于k但currentSum不等于targetSum，则直接返回
        if len(path) == k:
            if currentSum == targetSum:
                result.append(path[:])
            return
        for i in range(startIndex, 9 - (k - len(path)) + 2):  # 剪枝 👈
            currentSum += i  # 处理
            path.append(i)  # 处理
            self.backtracking(targetSum, k, currentSum, i + 1, path, result)  # 注意i+1调整startIndex
            currentSum -= i  # 回溯
            path.pop()  # 回溯
```

## 心得