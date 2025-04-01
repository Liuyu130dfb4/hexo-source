---
title: LeetCode 75 - 颜色分类
date: 2025-03-31 10:00:00
tags: 
- 双指针
- 排序
categories:
- algorithms
---

## 问题描述

给定一个包含红色、白色和蓝色三种颜色的数组 `nums`，将数组中的元素进行排序，使得相同颜色的元素相邻，并按照红、白、蓝的顺序排列。我们使用整数 0、1 和 2 分别表示红色、白色和蓝色。

要求：**使用常数空间的一趟扫描算法**。

---

## 算法思路

我们可以使用双指针的方式来解决这个问题。通过维护两个指针 `low` 和 `high`，分别表示红色区域的右边界和蓝色区域的左边界。

### 算法步骤

1. 初始化三个指针：
   - `low` 指向红色区域的右边界
   - `high` 指向蓝色区域的左边界
   - `i` 为当前扫描的指针

2. 遍历数组：
   - 如果 `nums[i] == 0`，将其与 `nums[low]` 交换，`low` 和 `i` 向右移动。
   - 如果 `nums[i] == 2`，将其与 `nums[high]` 交换，`high` 向左移动。
   - 如果 `nums[i] == 1`，仅 `i` 向右移动。

3. 遍历结束后，数组即为排序后的结果。

---

### 分析

这个算法乍看很难懂，但是实际上是为了达到以下效果：

- `all in [0, low) == 0`
- `all in [low, i) == 1`
- `all in (high, len - 1] == 2`

因此当 `i` 迭代到大于 `high` 的时候，就代表整个数组处理完毕。

## 代码实现

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        low, i, high = 0, 0, len(nums) - 1
        
        while i <= high:
            if nums[i] == 0:
                nums[i], nums[low] = nums[low], nums[i]
                low += 1
                i += 1
            elif nums[i] == 2:
                nums[i], nums[high] = nums[high], nums[i]
                high -= 1
            else:
                i += 1
```

---

## 复杂度分析

- **时间复杂度**：O(n)，只需遍历数组一次。
- **空间复杂度**：O(1)，只使用了常数空间。

---

## 总结

双指针法是一种非常高效的解决方案，能够在一趟扫描中完成排序，满足题目要求。
