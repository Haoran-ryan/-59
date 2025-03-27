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

# Matrix simulation 

## Key
- define the boundary 
- understand the move of 'pointers' in 4 directions and how the boundary interacts with the pointers : the fileds waiting for filling shrink 
  - TODO: the change of boundary flat (left,right,top,bottom) happens when a for loop finishes (outside the loop, NOT inside)

![spiralMatrix](./59SpiralMatrix.jpg)

### Mistakes
1. boundary 
2. we move `i` to fill in the fileds instaed of the 4 boundary flags 
3. the boudnaries of the for loop shall be defined by the 4 markers 

```python
# Mistake answer
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        left, right = 0, n-1
        top, bottom = 0, n-1
        res = [[0]*n for _ in range(n)]
        n = 0

        while left <= right and top <= bottom:
            for i in range(right + 1):
                res[left][right+i] += n
                # top += 1
            top += 1
            
            for i in range(bottom + 1):
                res[top+i][right] += n
                # top += 1
            right -= 1

            # if left <= right:
            if top <= bottom:
                for i in range(right + 1):
                    res[bottom][right - i] += n
                bottom -= 1

            if left<= right:
                for i in range(right + 1):
                    res[bottom][left+1] += n
                
                left += 1
        return res 

```

```python
#correct code
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        left, right = 0, n-1
        top, bottom = 0, n-1
        res = [[0]*n for _ in range(n)]
        num = 1  # Use a different variable name for counter
        
        while left <= right and top <= bottom:
            # Fill top row (left to right)
            for i in range(left, right + 1):  # Range should be from left to right
                res[top][i] = num  # Direct assignment
                num += 1           # Increment counter
            top += 1
            
            # Fill right column (top to bottom)
            for i in range(top, bottom + 1):  # Range should be from top to bottom
                res[i][right] = num
                num += 1
            right -= 1
            
            # Fill bottom row (right to left)
            if top <= bottom:
                for i in range(right, left - 1, -1):  # Range should go backward
                    res[bottom][i] = num
                    num += 1
                bottom -= 1
            
            # Fill left column (bottom to top)
            if left <= right:
                for i in range(bottom, top - 1, -1):  # Range should go backward
                    res[i][left] = num
                    num += 1
                left += 1
                
        return res
```

# 区间和 

题目描述

给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。

输入描述

第一行输入为整数数组 Array 的长度 n，接下来 n 行，每行一个整数，表示数组的元素。随后的输入为需要计算总和的区间，直至文件结束。

输出描述

输出每个指定区间内元素的总和。

输入示例
```
5
1
2
3
4
5
0 1
1 3
```
输出示例
```
3
9
```
数据范围：
```
0 < n <= 100000
```

1. **预处理前缀和数组**：
   - 创建一个长度比原数组大1的前缀和数组`preSum`，其中`preSum[i]`表示原数组前`i`个元素的和。
   - 例如，原数组`[1,2,3,4,5]`对应的前缀和数组为`[0,1,3,6,10,15]`。

2. **快速计算区间和**：
   - 对于每个查询区间`[a, b]`，其和等于`preSum[b+1] - preSum[a]`。这利用了前缀和数组的差值特性，将区间和的计算复杂度从O(n)降至O(1)。

**代码实现**：
```python
def main():
    import sys
    input = sys.stdin.read().split()
    ptr = 0
    n = int(input[ptr])
    ptr += 1
    array = []
    for _ in range(n):
        array.append(int(input[ptr]))
        ptr += 1
    # 构建前缀和数组
    preSum = [0] * (n + 1)
    for i in range(1, n+1):
        preSum[i] = preSum[i-1] + array[i-1]
    # 处理查询
    while ptr < len(input):
        a = int(input[ptr])
        b = int(input[ptr+1])
        ptr +=2
        res = preSum[b+1] - preSum[a]
        print(res)
        
if __name__ == "__main__":
    main()
```

**关键点解释**：
- **前缀和数组**：通过预处理将前i项和存储起来，避免重复计算。
- **查询优化**：每次查询只需两次数组访问和一次减法操作，极大提升效率。
- **输入处理**：使用`sys.stdin.read`一次性读取所有输入，避免逐行读取的性能问题。

这种方法的时间复杂度为O(n + q)，其中n是数组长度，q是查询次数，能够高效处理大规模数据。