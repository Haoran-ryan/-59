# Sliding window

## Two types

1. Fixed size window
2. Dynamic size window

### 209. Minimum Size Subarray Sum

```python
import sys

class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        # Edge case: if the array is empty
        if not nums:
            return 0

        # Initialize variables
        left = 0
        cur_sum = 0
        min_len = sys.maxsize

        # Iterate through the array (moving the right pointer)
        for right in range(len(nums)):
            # Add the current element to our running sum
            cur_sum += nums[right]

            # While our sum is greater than or equal to the target,
            # try to minimize the window by moving the left pointer
            while cur_sum >= target:
                # Update the minimum length
                min_len = min(min_len, right - left + 1)

                # Remove the leftmost element from the sum
                cur_sum -= nums[left]

                # Move the left pointer to the right
                left += 1

        # If we never found a valid subarray, return 0
        return min_len if min_len != sys.maxsize else 0
```

#### Key points

1. break donw the conditions
   1. if current sum >= s, update min_len and move left pointer
   2. if current sum < s, move right pointer
2. `while cur_sum >= target` use `while` instead of `if` because we need to keep moving left pointer until the condition is not satisfied
