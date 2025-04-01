---
title: LeetCode 287 - 寻找重复数
date: 2025-03-30 20:00:00
tags: 
- floyd
- 双指针
- 环检测
categories:
- algorithms
---

## Floyd 判圈算法（Floyd's Cycle-Finding Algorithm）

Floyd 判圈算法，也被称为"龟兔赛跑算法"（Tortoise and Hare Algorithm），是一个用于在链表中检测环的算法。该算法由 Robert W. Floyd 发明，其核心思想非常优雅。

### 算法原理

1. 设置两个指针：
   - 慢指针（龟）：每次移动一步
   - 快指针（兔）：每次移动两步

2. 如果链表中存在环：
   - 快慢指针最终一定会在环内某点相遇
   - 相遇时，快指针一定比慢指针多走了若干圈

3. 寻找环的入口：
   - 相遇后，将其中一个指针重置到起点
   - 两个指针每次都走一步
   - 再次相遇的点就是环的入口

### 为什么算法有效？

假设：
- 链表起点到环入口的距离为 F
- 环入口到相遇点的距离为 a
- 环的长度为 C

当两指针第一次相遇时：
- 慢指针走了 F + a 步
- 快指针走了 F + a + nC 步（n 为某个整数）
- 由于快指针速度是慢指针的两倍，所以：2(F + a) = F + a + nC
- 化简得：F = nC - a

这就证明了为什么将一个指针重置到起点后，两指针同速前进最终会在环入口相遇。

## LeetCode 287 题解

LeetCode 287 可以巧妙地转换为判圈问题。给定一个包含 n + 1 个整数的数组 nums，其中每个整数都在 1 到 n 之间，证明存在至少一个重复的数字。

### 如何转换为判圈问题？

我们可以构建一个隐式的链表：
- 将数组值作为指向下一个位置的指针
- 即：从位置 i 转到位置 nums[i]

由于存在重复数字，这个隐式链表必然存在环，而环的入口就是重复的数字。

### 代码实现：

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # 初始化快慢指针
        slow = nums[0]
        fast = nums[nums[0]]
        
        # 第一次相遇
        while slow != fast:
            slow = nums[slow]
            fast = nums[nums[fast]]
        
        # 重置一个指针到起点
        slow = 0
        
        # 寻找环的入口
        while slow != fast:
            slow = nums[slow]
            fast = nums[fast]
        
        return slow

```

### 复杂度分析

- 时间复杂度：O(n)
- 空间复杂度：O(1)
- 不需要修改原数组
- 不需要额外空间

Floyd 判圈算法在这个问题中展现出了优雅的数学美感，它不仅解决了问题，而且完美满足了所有限制条件。
