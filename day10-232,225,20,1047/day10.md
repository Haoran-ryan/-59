# Leetcode - 232 Implement Queue using Stacks

## Takeaway

1. Why do we need two stacks instead of one list?
   - A queue is FIFO (First-In-First-Out) while a stack is LIFO (Last-In-First-Out)
   - Using two stacks allows us to reverse the order of elements, effectively converting LIFO to FIFO behavior
   - Using one list with operations like `list[0]` or slicing wouldn't be using pure stack operations

2. How should elements flow between the two stacks?
   - Elements should only flow in one direction: from `stack_in` to `stack_out`
   - Elements are never transferred back from `stack_out` to `stack_in`
   - This one-way flow maintains the correct ordering for queue operations

3. When should we transfer elements between stacks?
   - Only transfer elements from `stack_in` to `stack_out` when `stack_out` is empty and we need to pop/peek
   - This minimizes the number of transfers and gives us amortized O(1) time complexity
   - Each element is moved at most once from input to output stack

4. How should we transfer elements between stacks?
   - Use `while` loops with `pop()` operations, not `for` loops with iteration
   - Example: `while stack_in: stack_out.append(stack_in.pop())`
   - This properly uses stack operations and reverses the order of elements

5. How do we implement the empty() method correctly?
   - Check if both stacks are empty: `return len(stack_in) == 0 and len(stack_out) == 0`
   - Elements can be in either stack, so both need to be checked

6. How do we implement peek() correctly?
   - Similar logic to pop(), but use `stack_out[-1]` to view the top element without removing it
   - Make sure to transfer elements from `stack_in` first if `stack_out` is empty

7. Do we need to update both stacks whenever we push or pop?
   - No! When pushing, just add to `stack_in`
   - When popping, just remove from `stack_out` (transferring from `stack_in` only if needed)
   - The two stacks maintain their separate purposes

8. Why do I need to check both lists for `empty`? 

## Code

```python
class MyQueue:

    def __init__(self):
        self.stack_in = []  # handle append
        self.stack_out = []  # handle pop

    def push(self, x: int) -> None:
        self.stack_in.append(x)

    def pop(self) -> int:
        # If stack_out has elements, just pop from there
        if self.stack_out:
            return self.stack_out.pop()
        
        # Otherwise, transfer all elements from stack_in to stack_out
        while self.stack_in:  # Use while loop to pop elements
            self.stack_out.append(self.stack_in.pop())
        
        # Now return the top element from stack_out (if any)
        if self.stack_out:
            return self.stack_out.pop()
        return None  # Queue is empty

    def peek(self) -> int:
        # Similar to pop, but we don't remove the element
        if self.stack_out:
            return self.stack_out[-1]  # Look at top element without removing
        
        # Transfer if needed
        while self.stack_in:
            self.stack_out.append(self.stack_in.pop())
        
        if self.stack_out:
            return self.stack_out[-1]
        return None  # Queue is empty

    def empty(self) -> bool:
        # Need to check both stacks
        return len(self.stack_in) == 0 and len(self.stack_out) == 0
```