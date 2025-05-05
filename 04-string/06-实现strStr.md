## 题目
[28.实现strStr() | 力扣题目](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

检测 `needle` 是否包含在 `haystack` 字符串中, 如果包含返回开始的索引值, 否则返回 `-1`.

## 初步解答
本题即实现 Python string 内置的 `find` 函数. 具体原理是 KMP 算法, 放在下一节来看, 👋先跳过.
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```

## 参考解答
- [ ]待补 KMP 算法

## 心得
