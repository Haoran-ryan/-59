# Leetcode - 150 Evaluate Reverse Polish Notation

## Takeaways

1. `isdigit()` not work on negatives 


## Codes

```python
# wrong code 
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        ops = {
            '+': lambda x,y: x+y,
            '-': lambda x,y: x-y,
            '*': lambda x,y: x*y,
            '/': lambda x,y: x/y,
        }
        stack = []

        for i in tokens:
            if i.isdigit():
                stack.append(int(i))
            else:
                left = stack.pop()
                right = stack.pop()
                res = ops[i](left,right)
                stack.append(res)
        
        return stack.pop()

```

```python
# correct
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        ops = {
            '+': lambda x, y: x + y,
            '-': lambda x, y: x - y,
            '*': lambda x, y: x * y,
            '/': lambda x, y: int(x / y),  # Integer division for negative numbers
        }
        stack = []

        for i in tokens:
            if i in ops:  # Check if it's an operator
                # Order matters: second operand is popped first
                second = stack.pop()
                first = stack.pop()
                res = ops[i](first, second)
                stack.append(res)
            else:  # It's a number (can be positive or negative)
                stack.append(int(i))
        
        return stack[0]  # The final result should be the only item left in the stack
```