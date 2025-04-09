---
title: LeetCode 15 - 三数之和
date: 2025-04-02 10:00:00
tags: 
- 双指针
- 排序
categories:
- algorithms
---

## 问题描述

给你一个整数数组 `nums`，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k`，同时还满足 `nums[i] + nums[j] + nums[k] == 0`。请你返回所有和为 `0` 且不重复的三元组。

---

## 算法思路

1. **排序**：首先对数组进行排序，方便后续使用双指针。
2. **固定一个数**：遍历数组，固定一个数 `nums[i]`，然后在剩余部分寻找两个数的和为 `-nums[i]`。
3. **双指针**：
   - 使用两个指针 `left` 和 `right`，分别指向当前范围的起点和终点。
   - 根据三数之和的大小调整指针位置：
     - 如果和小于 0，移动 `left` 向右。
     - 如果和大于 0，移动 `right` 向左。
     - 如果和为 0，记录结果并跳过重复元素。

---

## 为什么双指针方法有效？

双指针方法是一种贪心策略，它利用了数组排序后的性质，确保不会漏掉任何可能的结果。以下是详细分析：

1. **排序的作用**：
   - 排序后，数组中的元素从小到大排列，便于我们通过调整指针位置来控制三数之和的大小。
   - 通过排序，我们可以避免重复解的出现，因为相同的元素会被连续地排列在一起。

2. **双指针的贪心策略**：
   - 固定一个数 `nums[i]` 后，双指针 `left` 和 `right` 分别从剩余部分的两端向中间移动。
   - 如果当前三数之和小于 0，则移动 `left` 向右以增大和。
   - 如果当前三数之和大于 0，则移动 `right` 向左以减小和。
   - 如果当前三数之和等于 0，则记录结果，并跳过重复元素。

3. **为什么不会漏掉结果？**
   - 假设我们已经确定了 `nums[i]`，并且当前的 `left` 和 `right` 指针分别指向 `nums[left]` 和 `nums[right]`。
   - 如果存在一个三元组 `[nums[i], nums[j], nums[k]]` 满足 `nums[i] + nums[j] + nums[k] == 0`，那么一定有 `left <= j < k <= right`。
   - 不可能出现交叉情况：
     - **情况 1**：`left <= j < right < k`。这种情况不可能，因为当 `right` 在 `k` 时，`nums[left] + nums[right]` 小于目标值，应该移动 `left` 而不是 `right`。
     - **情况 2**：`j < left < k <= right`。这种情况也不可能，因为当 `left` 在 `j` 时，`nums[left] + nums[right]` 大于目标值，应该移动 `right` 而不是 `left`。
   - 因此，双指针的移动策略确保了所有可能的三元组都会被遍历到。

4. **总结**：
   - 双指针方法利用了排序后的数组性质，通过贪心策略调整指针位置，避免了二重循环的冗余计算。
   - 这种方法不仅高效，而且通过严格的指针移动规则，确保不会漏掉任何可能的结果。

---

## 代码实现

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()  # 排序
        res = []
        
        for i in range(len(nums)):
            # 跳过重复元素
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            
            target = -nums[i]
            left, right = i + 1, len(nums) - 1
            
            while left < right:
                sum_ = nums[left] + nums[right]
                if sum_ == target:
                    res.append([nums[i], nums[left], nums[right]])
                    left += 1
                    right -= 1
                    # 跳过重复元素
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1
                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1
                elif sum_ < target:
                    left += 1
                else:
                    right -= 1
        
        return res
```

---

## 复杂度分析

- **时间复杂度**：O(n²)
  - 排序需要 O(n log n)，双指针遍历每个元素需要 O(n²)。
- **空间复杂度**：O(1)
  - 仅使用了常数额外空间。

---

## 总结

三数之和问题是双指针的经典应用之一。通过排序和双指针的结合，我们可以高效地解决问题，同时避免重复解的出现。
