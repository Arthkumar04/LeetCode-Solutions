# LeetCode Solutions

Daily LeetCode problem statements and Python 3 solutions, organized by day.

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
- Explanation:
  - Increase `nums[0]` from 2 to 3.
  - Increase `nums[1]` from 1 to 2.
  - Increase `nums[1]` from 2 to 3.
  - All elements are now equal to 3.

**Example 2:**
- Input: `nums = [4,4,5]`
- Output: `2`
- Explanation:
  - Increase `nums[0]` from 4 to 5.
  - Increase `nums[1]` from 4 to 5.
  - All elements are now equal to 5.

### Constraints

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

### Approach

Because we can only increase elements, the smallest possible common target is the maximum value already present in the array. For each element, calculate `max(nums) - num` and add these differences.

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

- **Time Complexity:** `O(n)` — finding the maximum and traversing the array each take linear time.
- **Space Complexity:** `O(1)` — only a constant amount of extra space is used.

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
- Explanation: Decrease 3 to 2 and increase 1 to 2.

**Example 2:**
- Input: `nums = [1,10,2,9]`
- Output: `16`

### Constraints

- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- The answer is guaranteed to fit in a 32-bit integer.

### Approach

The median minimizes the sum of absolute differences between all elements and a chosen target. Sort the array, select the middle element, and add the absolute difference between every element and the median.

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

- **Time Complexity:** `O(n log n)` — sorting dominates the traversal.
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

- **Time Complexity:** `O(n)` — calculating the sum and minimum requires linear time.
- **Space Complexity:** `O(1)` — only constant extra space is used.

---

*Keep practicing. Add a new day whenever you solve another problem!*
