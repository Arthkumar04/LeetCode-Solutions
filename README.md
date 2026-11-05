# LeetCode Solutions

Daily LeetCode problem statements and Python 3 solutions, organized by day.

---

## Day 5 — [1385. Find the Distance Value Between Two Arrays](https://leetcode.com/problems/find-the-distance-value-between-two-arrays/)

**Difficulty:** Easy  
**Language:** Python 3  
**Status:** Solved

### Question

Given two integer arrays `arr1` and `arr2`, and the integer `d`, return the distance value between the two arrays.

The distance value is the number of elements `arr1[i]` such that there is no element `arr2[j]` where `|arr1[i] - arr2[j]| <= d`.

### Examples

**Example 1:**
- Input: `arr1 = [4,5,8]`, `arr2 = [10,9,1,8]`, `d = 2`
- Output: `2`
- Explanation: The values 4 and 5 are each more than distance 2 from every element in `arr2`. The value 8 is within distance 2 of elements in `arr2`, so it is not counted.

**Example 2:**
- Input: `arr1 = [1,4,2,3]`, `arr2 = [-4,-3,6,10,20,30]`, `d = 3`
- Output: `2`

**Example 3:**
- Input: `arr1 = [2,1,100,3]`, `arr2 = [-5,-2,10,-3,7]`, `d = 6`
- Output: `1`

### Constraints

- `1 <= arr1.length, arr2.length <= 500`
- `-1000 <= arr1[i], arr2[j] <= 1000`
- `0 <= d <= 100`

### Approach

For each value in `arr1`, compare it with every value in `arr2`. If any absolute difference is less than or equal to `d`, do not count that value. Otherwise, increment the answer.

### Python 3 Solution

```python
from typing import List

class Solution:
    def findTheDistanceValue(self, arr1: List[int], arr2: List[int], d: int) -> int:
        count = 0

        for num in arr1:
            valid = True
            for x in arr2:
                if abs(num - x) <= d:
                    valid = False
                    break

            if valid:
                count += 1

        return count
```

### Complexity Analysis

- **Time Complexity:** `O(m * n)`, where `m` and `n` are the lengths of `arr1` and `arr2`.
- **Space Complexity:** `O(1)` auxiliary space.

---

## Day 4 — 3505. Minimum Operations to Make Elements Within K Subarrays Equal

**Difficulty:** Hard  
**Language:** Python 3  
**Status:** Solved

### Question

You are given an integer array `nums` and two integers, `x` and `k`. You can perform the following operation any number of times (including zero):

- Increase or decrease any element of `nums` by 1.

Return the minimum number of operations needed to have at least `k` non-overlapping subarrays of size exactly `x` in `nums`, where all elements within each subarray are equal.

### Examples

**Example 1:**
- Input: `nums = [5,-2,1,3,7,3,6,4,-1]`, `x = 3`, `k = 2`
- Output: `8`
- Explanation: Change the subarray at indices 1 through 3 to `[1,1,1]` using 5 operations, and the subarray at indices 5 through 7 to `[4,4,4]` using 3 operations. The two windows do not overlap, for a total of 8 operations.

**Example 2:**
- Input: `nums = [9,-2,-2,-2,1,5]`, `x = 2`, `k = 2`
- Output: `3`
- Explanation: Decrease the value at index 4 from 1 to -2 using 3 operations. The windows at indices 1–2 and 3–4 are both `[-2,-2]`.

### Constraints

- `2 <= nums.length <= 10^5`
- `-10^6 <= nums[i] <= 10^6`
- `2 <= x <= nums.length`
- `1 <= k <= 15`
- `2 <= k * x <= nums.length`

### Approach

1. For each window of size `x`, the minimum cost to make all values equal is achieved by changing every value to a median.
2. Two Fenwick trees maintain the counts and sums of values in the sliding window, allowing the median and window cost to be calculated efficiently.
3. Dynamic programming selects exactly `k` non-overlapping windows with minimum total cost. `dp[i][j]` is the minimum cost to select `j` windows from the first `i` elements.

### Python 3 Solution

```python
from typing import List

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def query(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def kth(self, k):
        idx = 0
        bit_mask = 1 << (self.n.bit_length() - 1)
        while bit_mask:
            nxt = idx + bit_mask
            if nxt <= self.n and self.bit[nxt] < k:
                idx = nxt
                k -= self.bit[nxt]
            bit_mask >>= 1
        return idx + 1


class Solution:
    def minOperations(self, nums: List[int], x: int, k: int) -> int:
        n = len(nums)
        vals = sorted(set(nums))
        m = len(vals)
        pos = {v: i + 1 for i, v in enumerate(vals)}

        bit_count = Fenwick(m)
        bit_sum = Fenwick(m)
        total = 0
        costs = []

        for i, v in enumerate(nums):
            p = pos[v]
            bit_count.add(p, 1)
            bit_sum.add(p, v)
            total += v

            if i >= x:
                old = nums[i - x]
                q = pos[old]
                bit_count.add(q, -1)
                bit_sum.add(q, -old)
                total -= old

            if i >= x - 1:
                mid = bit_count.kth((x + 1) // 2)
                med = vals[mid - 1]
                left_count = bit_count.query(mid)
                left_sum = bit_sum.query(mid)
                right_count = x - left_count
                right_sum = total - left_sum
                cost = (med * left_count - left_sum
                        + right_sum - med * right_count)
                costs.append(cost)

        # dp[i][j]: minimum cost to choose j non-overlapping
        # windows from the first i array elements.
        INF = float("inf")
        dp = [[INF] * (k + 1) for _ in range(n + 1)]
        for i in range(n + 1):
            dp[i][0] = 0

        for i in range(1, n + 1):
            for j in range(1, min(k, i // x) + 1):
                dp[i][j] = dp[i - 1][j]
                if i >= x:
                    dp[i][j] = min(
                        dp[i][j],
                        dp[i - x][j - 1] + costs[i - x]
                    )

        return dp[n][k]
```

### Complexity Analysis

Let `n` be the array length and `m` the number of distinct values.

- **Time Complexity:** `O(n log m + n * k)` — Fenwick tree operations calculate window costs, and dynamic programming selects the non-overlapping windows.
- **Space Complexity:** `O(n * k + m)` — the DP table uses `O(n * k)` space; coordinate compression and Fenwick trees use `O(m)` space.

---

## Day 3 — 3736. Minimum Moves to Equal Array Elements III

**Difficulty:** Easy  
**Language:** Python 3  
**Status:** Solved

### Question

You are given an integer array `nums`.

In one move, you may increase the value of any single element `nums[i]` by 1.

Return the minimum total number of moves required so that all elements in `nums` become equal.

### Examples

**Example 1:**
- Input: `nums = [2,1,3]`
- Output: `3`
- Explanation: Increase 2 to 3, then increase 1 to 2 and from 2 to 3.

**Example 2:**
- Input: `nums = [4,4,5]`
- Output: `2`
- Explanation: Increase both 4s to 5.

### Constraints

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

### Approach

Since elements can only be increased, the smallest possible common target is the maximum value in the array. Sum the difference between this maximum and each element.

### Python 3 Solution

```python
class Solution:
    def minMoves(self, nums: List[int]) -> int:
        max_num = max(nums)
        moves = 0
        for num in nums:
            moves += max_num - num
        return moves
```

### Complexity Analysis

- **Time Complexity:** `O(n)`.
- **Space Complexity:** `O(1)`.

---

## Day 2 — 462. Minimum Moves to Equal Array Elements II

**Difficulty:** Medium  
**Language:** Python 3  
**Status:** Solved

### Question

Given an integer array `nums` of size `n`, return the minimum number of moves required to make all array elements equal.

In one move, you can increment or decrement an element by 1.

### Examples

**Example 1:**
- Input: `nums = [1,2,3]`
- Output: `2`

**Example 2:**
- Input: `nums = [1,10,2,9]`
- Output: `16`

### Constraints

- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- The answer is guaranteed to fit in a 32-bit integer.

### Approach

The median minimizes the sum of absolute differences. Sort the array, choose a middle element, and sum the absolute difference between each element and that median.

### Python 3 Solution

```python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        nums.sort()
        median = nums[len(nums) // 2]
        moves = 0
        for num in nums:
            moves += abs(num - median)
        return moves
```

### Complexity Analysis

- **Time Complexity:** `O(n log n)` due to sorting.
- **Space Complexity:** Python's in-place sorting may use additional temporary memory.

---

## Day 1 — 453. Minimum Moves to Equal Array Elements

**Difficulty:** Medium  
**Language:** Python 3  
**Status:** Solved

### Question

Given an integer array `nums` of size `n`, return the minimum number of moves required to make all array elements equal.

In one move, you can increment `n - 1` elements by 1.

### Examples

**Example 1:**
- Input: `nums = [1,2,3]`
- Output: `3`
- Explanation: `[1,2,3]` becomes `[2,3,3]`, then `[3,4,3]`, then `[4,4,4]`.

**Example 2:**
- Input: `nums = [1,1,1]`
- Output: `0`

### Constraints

- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- The answer is guaranteed to fit in a 32-bit integer.

### Approach

Incrementing `n - 1` elements by 1 has the same effect on relative differences as decrementing the one element left unchanged. Therefore, the optimal strategy is equivalent to reducing every element to the minimum value.

The number of moves is `sum(nums) - min(nums) * len(nums)`.

### Python 3 Solution

```python
class Solution:
    def minMoves(self, nums: List[int]) -> int:
        return sum(nums) - min(nums) * len(nums)
```

### Complexity Analysis

- **Time Complexity:** `O(n)`.
- **Space Complexity:** `O(1)`.

---

*Keep practicing. Add a new day whenever you solve another problem!*
