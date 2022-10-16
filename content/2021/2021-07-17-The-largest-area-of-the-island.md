---
date: 2021-07-17
title: 695.岛屿的最大面积
author: Jn
categories: 工作
tags: 
- Python
---

题目：https://leetcode-cn.com/problems/max-area-of-island/

图的遍历类似二叉树，分为
* 广度遍历 BFS
* 深度遍历 DFS
两者共同点是<q>遍历的结果不固定</q>（其实只是一般意义上的，如果固定的图和代码，结果是一样的）

一般广度遍历写成双端队列，深度遍历写成递归或栈（元素出栈再访问）。

下面放这题的两种实现。
官方题解 https://leetcode-cn.com/problems/max-area-of-island/solution/dao-yu-de-zui-da-mian-ji-by-leetcode-solution/

### DFS
```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        def dfs(x, y):
            if x < 0 or x >= m or y < 0 or y >= n:
                return 0
            if grid[x][y] == 0:
                return 0
            grid[x][y] = 0
            return 1 + dfs(x-1, y) + dfs(x+1, y) + dfs(x,y-1)+dfs(x, y+1)
        res = 0
        for i in range(m):
            for j in range(n):
                res = max(res, dfs(i,j))
        return res
```

### BFS
```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        ans = 0
        for i, l in enumerate(grid):
            for j, n in enumerate(l):
                cur = 0
                q = collections.deque([(i, j)])
                while q:
                    cur_i, cur_j = q.popleft()
                    if cur_i < 0 or cur_j < 0 or cur_i == len(grid) or cur_j == len(grid[0]) or grid[cur_i][cur_j] != 1:
                        continue
                    cur += 1
                    grid[cur_i][cur_j] = 0
                    for di, dj in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                        next_i, next_j = cur_i + di, cur_j + dj
                        q.append((next_i, next_j))
                ans = max(ans, cur)
        return ans
'''
作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/max-area-of-island/solution/dao-yu-de-zui-da-mian-ji-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
'''
```
