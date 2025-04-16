# Leetcoe 110 Balanced Binary Tree

## Takeaways

1. The key is move from 'bottom-up' - Calculate results for leaf nodes first
2. Recursive function - not modify existing variables but creating new values which get passed up the call stack
3. Break into small bits - is a node balanced? If yes, height
4. Base case - difficult to define
5. Recursive call - FUNNY THOUGHT 'Trust The Recursion'

   ```python
    left_balanced, left_height = check_balance(node.left)
    # "This will give me accurate information about the balance and height of the left subtree." Don't worry about how it does this—just trust that it will, because you're defining the function to do exactly that.
   ```

```python
def isBalanced(self, root):
    balanced, _ = self.check_balance(root)
    return balanced

def check_balance(self, node):
    if not node:
        return True, -1 # TODO: key - if the prev node is a leaf node

    left_balanced, left_height = self.check_balance(node.left)

    # Early return optimization: if left subtree is unbalanced,
    # the whole tree is unbalanced
    if not left_balanced:
        return False, 0  # Height doesn't matter anymore

    right_balanced, right_height = self.check_balance(node.right)

    # Early return optimization: if right subtree is unbalanced,
    # the whole tree is unbalanced
    if not right_balanced:
        return False, 0  # Height doesn't matter anymore

    # Check if current node maintains the balance condition
    is_balanced = abs(left_height - right_height) <= 1

    # Calculate the height of current subtree
    height = max(left_height, right_height) + 1

    return is_balanced, height  # two values: bool, int
```

# Leetcode 257 Binary Tree Paths

## Takeaways

1. Base case definition
   1. This base case in `check_path` does not work
   2. should check if a recursive call has reached a leaf node (instead of stepping outside from a leaf)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if root is None:
            return root

        cur_path = ''
        def check_path(node, cur_path):
            if node is None:
                return cur_path

            left = check_path(node.left, cur_path)
            right = check_path(node.right, cur_path)



```

2. When to add a node to a sub path?
3. How to manage subpath, clean it for a new around of path searching ?

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if root is None:
            return root  # ISSUE: Should return empty list [] instead of root

        cur_path = []  # This is a good start - creating a path list
        def check_path(node, cur_path):

            if node.left is None and node.right is None:
                cur_path.append(node.val)  # ISSUE: This only adds the leaf node value
                                           # ISSUE: Not converting to string for joining later
                                           # ISSUE: Not adding this complete path to any result collection
                return cur_path  # ISSUE: Returning the path doesn't collect it anywhere

            # ISSUE: You're not adding the current node's value to the path before recursing
            # ISSUE: Missing the case where a node has BOTH left and right children

            if node.left is None:  # This handles only right child
                return check_path(node.right, cur_path)  # ISSUE: Using same cur_path object for all paths

            if node.right is None:  # This handles only left child
                return check_path(node.left, cur_path)  # ISSUE: Using same cur_path object for all paths

            # ISSUE: No handling for the case when node has both children

        check_path(root,cur_path)  # ISSUE: Not collecting the result of this call
                                   # ISSUE: Not creating a master list to collect all paths

        return cur_path  # ISSUE: This returns only the last path found, not all paths
                         # ISSUE: Not formatting paths as strings with "->" separator

```

4. When to use `.copy()` in recursion?
   You should use `.copy()` (or similar methods for creating copies) in recursive functions when you need to maintain separate, independent versions of a mutable data structure across different recursive branches. Here are the key scenarios where copying is important:

1. **When tracking paths in tree traversals**: Like in the binary tree paths problem we discussed, you need separate copies of the path for each branch of the tree.

1. **When maintaining state that shouldn't be shared** between different recursive calls, especially in depth-first searches or backtracking algorithms.

1. **When a recursive function modifies a data structure** and you don't want those modifications to affect other recursive calls.

The general rule is: use `.copy()` when multiple recursive calls need to work with their own versions of the same data structure, particularly when:

- The data structure is mutable (like lists, dictionaries, sets)
- Changes to the data structure in one branch shouldn't affect other branches
- You're building up solutions incrementally (like paths, combinations, permutations)

Common examples in algorithms:

- Path finding in graphs or trees
- Generating all possible combinations or permutations
- Backtracking problems (N-Queens, Sudoku solver, etc.)
- Building trees or trying different decision paths

You typically don't need `.copy()` when:

- Working with immutable data (integers, strings, tuples)
- Only one recursive branch is explored at a time
- The recursive function doesn't modify the shared data structure
- You're accumulating results in a global collection that all recursive calls can safely modify

### code

```python

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if root is None:
            return []  # Return empty list instead of root

        result = []  # Create a master list to store all complete paths

        def check_path(node, cur_path):
            # Add current node's value to the path
            cur_path.append(str(node.val))  # Convert to string for later joining

            # Check if current node is a leaf node
            if node.left is None and node.right is None:
                # We found a complete path - add it to our result list
                result.append("->".join(cur_path))  # Join with -> delimiter
                return  # No need to return anything - we've added path to result

            # If node has left child, explore that path
            if node.left:
                check_path(node.left, cur_path.copy())  # Use copy to avoid modifying the same list

            # If node has right child, explore that path
            if node.right:
                check_path(node.right, cur_path.copy())  # Use copy to avoid modifying the same list

            # No need to return anything from this function

        # Start recursion with empty path
        check_path(root, [])

        return result  # Return the master list of all paths
```
