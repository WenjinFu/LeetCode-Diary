# Array

## LeetCode Problem Solution 

Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.

Return *the sum of the three integers*.

You may assume that each input would have exactly one solution.

**Example 1:**

```mathematica
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Example 2:**

```mathematica
Input: nums = [0,0,0], target = 1
Output: 0
```

### CODE:

#### First Draft-Not a Solution, Brute Force (n^3)

Unworkable on LeetCode, workable in IDE

```python
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        comp = float('inf')
        res = 0
        for i, value in enumerate(nums):
            j = i + 1
            while j < len(nums):
                k = j + 1
                while k < len(nums):
                    sn = nums[i] + nums[j] + nums[k]
                    diff = abs(sn - target)
                    if comp > diff:
                        res = sn
                        comp = diff
                    k += 1
                j += 1
        return res
```

### Solution 1 (Optimized-Time complexity: n^2) 

<img src="https://github.com/WenjinFu/LeetCode-Diary/blob/main/Python/3Sum_Closest.jpeg" alt="3Sum_Closest" style="width:34%, height34%;"/>

```python
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        res = sum(nums[:3])
        for i in range(len(nums) - 2):
            s = i + 1
            e = len(nums)-1
            while s < e:
                sum3 = nums[i] + nums[s] + nums[e]
                if abs(sum3-target) < abs(res-target):
                    res = sum3
                if sum3 < target:
                    s += 1
                else:
                    e -=1
        return res
```













