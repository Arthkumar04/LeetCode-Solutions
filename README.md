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

Only three moves are needed (remember each move increments two elements):

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

The key observation is that incrementing `n - 1` elements by `1` is equivalent to decrementing the one remaining element by `1`, when considering the differences between elements.

Therefore, the minimum number of moves is the number of decrements needed to bring every element down to the minimum value.

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
- **Space Complexity:** `O(1)` — only a constant amount of extra space is used.

### Key Takeaway

When an operation changes all elements except one, consider whether it can be reframed as changing just the excluded element in the opposite direction. This can turn a simulation problem into a simple mathematical formula.

---

*More daily solutions will be added below this line.*
