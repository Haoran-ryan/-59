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

# Leetcode 225

# Stack vs Queue Confusion - Q&A

**Q1: Why do we need to rotate the queue after pushing a new element?**
A1: We rotate the queue to keep the newest element at the front. This way, when we pop, we get the last element added first (like a stack should). Without rotation, we'd get the oldest element first, which is not stack behavior.

**Q2: Why rotate during push instead of pop?**
A2: It's more efficient. By doing the work during push, we make pop operations faster (O(1) instead of O(n)). Since pop is usually called more frequently in stacks, this is a good trade-off.

**Q3: Does changing the order of elements mess up our stack?**
A3: No. The internal order [3,2,1] is just an implementation detail. What matters is that if you push 1,2,3, then when you pop, you get 3,2,1 out. The methods behave like a stack, even if the internal storage looks different.

**Common mistakes in your code:**

1. Spelling: It's `deque` not `dequeue`
2. Typo: `qppend` should be `append`
3. Unnecessary condition: The if-else in push isn't needed since we always append and rotate

**Correct simple implementation:**

```python
from collections import deque

class MyStack:
    def __init__(self):
        self.q = deque()

    def push(self, x):
        self.q.append(x)
        # Rotate to bring newest element to front
        for _ in range(len(self.q) - 1):
            self.q.append(self.q.popleft())

    def pop(self):
        return self.q.popleft()

    def top(self):
        return self.q[0]

    def empty(self):
        return len(self.q) == 0
```

# Leetcode 20 - Valid Parentheses

## Takeaway

1. Does `[{]}` count as valid?
   - No, because the brackets are not in the correct order. The last opened bracket must be the first one to close.

## code improvements

```python
class Solution:
    def isValid(self, s: str) -> bool:
        """
        len - odd ? -> False
        dict - kv pair for parentheses
        ({)} - incorrect order

        if '(' in pair dict keys, push its value ')' to stack
        if ')' not in dict keys,
            - stack empty -> False
            - pop the stack, if ==, continue , if not -> False

        end of loop , if stack not empty -> False
        """
        if len(s) % 2 != 0:
            return False

        pair = {
            '(':')',
            '[':']',
            '{':'}'
        }
        stack = []

        for ch in s:
            if ch in pair.keys():
                stack.append(pair[ch])
            else:
                if not stack:
                    return False
                ck = stack.pop()
                if ch == ck: continue
                else:
                    return False
        return False if stack else True
```

```python
def isValid(self, s: str) -> bool:
    if len(s) % 2 != 0:
        return False

    pair = {
        '(': ')',
        '[': ']',
        '{': '}'
    }
    stack = []

    for ch in s:
        if ch in pair:  # Simplified from pair.keys()
            stack.append(pair[ch])
        else:
            if not stack:
                return False
            ck = stack.pop()
            if ch != ck:  # Simplified logic (removed continue)
                return False

    return not stack  # Simplified from "False if stack else True"
```

# Leetcode 1047 - Remove All Adjacent Duplicates In String

## Takeaway

1. Why do we use stack?
   - Stack is a LIFO data structure, which allows us to easily remove the last added character if it matches the current character.
   - This is efficient for removing adjacent duplicates as we can simply pop from the stack when we find a match.
2. The key: 'adjacent duplicates'
   - We only need to check the last character in the stack to see if it matches the current character.
   - If it does, we pop it off the stack. If not, we push the current character onto the stack.
