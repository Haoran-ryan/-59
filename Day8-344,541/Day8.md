# Leetcode - 344 Reverse String

## My code - slow 

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s)-1
        
        while left < right:
            temp = s[left]
            s[left] = s[right]
            s[right] = temp
            
            left += 1
            right -=1
```

## Better performance 

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        s.reverse()
```

# Leetcode - 541 Reserse string 

## Takeway

1. index range , esp for `end`
2. it's all right to modify `i` and `end = i + k - 1` in the while loop - `i` is loop controller and `i` gets assigned by `for range` at the beginning of each iteration regardless of the modification made 

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        s_list = list(s)
        n = len(s_list)

        for i in range(0,n,2*k):
            j = i
            m = n
            end = j+k if j+k <= n else m

            while j < end:
                s_list[j], s_list[end] = s_list[end], s_list[j]
                j += 1
                end -= 1

        return "".join(s_list)
```