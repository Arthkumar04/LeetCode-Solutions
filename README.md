# LeetCode Solutions

My daily LeetCode practice, with complete problem statements and Python 3 solutions documented directly on this page.

Each entry includes the question, examples, constraints, approach, code, and complexity analysis. Everything is visible on this repository's homepage.

---

# Day 1 — Question 1

## LeetCode 453. Minimum Moves to Equal Array Elements

**Difficulty:** Medium  
**Language:** Python 3

### Question

Given an integer array `nums` of size `n`, return the minimum number of moves required to make all array elements equal.

In one move, you can increment `n - 1` elements of the array by `1`.

### Example 1

**Input:**
```text
nums = [1, 2, 3]
```

**Output:**
```text
3
```

**Explanation:**

Only three moves are needed (each move increments two elements):

```text
[1, 2, 3]
[2, 3, 3]
[3, 4, 3]
[4, 4, 4]
```

### Example 2

**Input:**
```text
nums = [1, 1, 1]
```

**Output:**
```text
0
```

### Constraints

- `n == nums.length`
- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- The answer is guaranteed to fit in a 32-bit integer.

### Approach: Mathematical Observation

Incrementing `n - 1` elements by `1` is equivalent, in terms of relative differences, to decrementing the remaining element by `1`.

So we can think of reducing every element to the minimum value in the array.

If `m` is the minimum value:

```text
Minimum moves = sum(nums) - (m * len(nums))
```

### Python 3 Solution

```python
from typing import List


class Solution:
    def minMoves(self, nums: List[int]) -> int:
        return sum(nums) - min(nums) * len(nums)
```

### Complexity Analysis

- **Time Complexity:** `O(n)` — finding the minimum and calculating the sum each take linear time.
- **Space Complexity:** `O(1)` auxiliary space.

### Key Takeaway

When an operation changes all elements except one, consider whether it can be reframed as changing just the excluded element in the opposite direction. This can turn a simulation problem into a mathematical formula.

---

# Day 2 — Question 2

## LeetCode 462. Minimum Moves to Equal Array Elements II

**Difficulty:** Medium  
**Language:** Python 3

### Question

Given an integer array `nums` of size `n`, return the minimum number of moves required to make all array elements equal.

In one move, you can increment or decrement an element of the array by `1`.

Test cases are designed so that the answer will fit in a 32-bit integer.

### Example 1

**Input:**
```text
nums = [1, 2, 3]
```

**Output:**
```text
2
```

**Explanation:**

Only two moves are needed (each move increments or decrements one element):

```text
[1, 2, 3]
[2, 2, 3]
[2, 2, 2]
```

### Example 2

**Input:**
```text
nums = [1, 10, 2, 9]
```

**Output:**
```text
16
```

### Constraints

- `n == nums.length`
- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

### Approach: Sorting and Median

The key observation is that the **median** minimizes the sum of absolute differences between all array elements and a chosen target.

1. Sort the array.
2. Select the middle element as the median.
3. Calculate the sum of the absolute differences between each element and the median.

For an even-sized array, either middle element is a valid median and gives the same minimum number of moves. This solution chooses `nums[len(nums) // 2]`.

### Python 3 Solution

```python
from typing import List


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

- **Time Complexity:** `O(n log n)` — sorting takes `O(n log n)`, and calculating the moves takes `O(n)`.
- **Space Complexity:** `O(1)` auxiliary space for the move calculation; Python's in-place sorting may use additional implementation-dependent memory.

### Key Takeaway

For minimizing the sum of absolute distances to a single target, use the median. This is different from minimizing the sum of squared distances, where the mean is the natural target.

---

*More daily solutions will be added below this line.*
