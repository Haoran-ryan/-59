# Binary Tree in Python

```python
class Node:
    """
    A class representing a node in a binary tree.
    Each node has a value and references to its left and right children.
    """
    def __init__(self, value):
        self.value = value      # The data stored in this node
        self.left = None        # Reference to the left child node
        self.right = None       # Reference to the right child node


class BinaryTree:
    """
    A class representing a binary tree structure.
    """
    def __init__(self):
        self.root = None        # Initialize with an empty root node

    def insert(self, value):
        """
        Insert a new value into the binary tree.
        This implementation creates a simple binary search tree
        where smaller values go to the left and larger values go to the right.
        """
        # Create a new node with the given value
        new_node = Node(value)

        # If the tree is empty, the new node becomes the root
        if self.root is None:
            self.root = new_node
            return

        # If the tree already has nodes, find the correct position for the new node
        self._insert_recursive(self.root, new_node)

    def _insert_recursive(self, current_node, new_node):
        """
        Helper method to recursively find the correct place to insert a new node.
        Uses the binary search tree property (left < parent < right).
        """
        # If the new value is less than the current node's value, go left
        if new_node.value < current_node.value:
            # If there's no left child, place the new node here
            if current_node.left is None:
                current_node.left = new_node
            else:
                # Otherwise, continue searching in the left subtree
                self._insert_recursive(current_node.left, new_node)

        # If the new value is greater than or equal to the current node's value, go right
        else:
            # If there's no right child, place the new node here
            if current_node.right is None:
                current_node.right = new_node
            else:
                # Otherwise, continue searching in the right subtree
                self._insert_recursive(current_node.right, new_node)

    def inorder_traversal(self):
        """
        Perform an inorder traversal of the tree (left-root-right).
        Returns a list of values in sorted order for a binary search tree.
        """
        result = []
        self._inorder_recursive(self.root, result)
        return result

    def _inorder_recursive(self, node, result):
        """
        Helper method for inorder traversal.
        """
        # Base case: if node is None, return
        if node is None:
            return

        # First, visit the left subtree
        self._inorder_recursive(node.left, result)

        # Then, visit the current node
        result.append(node.value)

        # Finally, visit the right subtree
        self._inorder_recursive(node.right, result)

    def preorder_traversal(self):
        """
        Perform a preorder traversal of the tree (root-left-right).
        """
        result = []
        self._preorder_recursive(self.root, result)
        return result

    def _preorder_recursive(self, node, result):
        """
        Helper method for preorder traversal.
        """
        # Base case: if node is None, return
        if node is None:
            return

        # First, visit the current node
        result.append(node.value)

        # Then, visit the left subtree
        self._preorder_recursive(node.left, result)

        # Finally, visit the right subtree
        self._preorder_recursive(node.right, result)

    def postorder_traversal(self):
        """
        Perform a postorder traversal of the tree (left-right-root).
        """
        result = []
        self._postorder_recursive(self.root, result)
        return result

    def _postorder_recursive(self, node, result):
        """
        Helper method for postorder traversal.
        """
        # Base case: if node is None, return
        if node is None:
            return

        # First, visit the left subtree
        self._postorder_recursive(node.left, result)

        # Then, visit the right subtree
        self._postorder_recursive(node.right, result)

        # Finally, visit the current node
        result.append(node.value)

    def display(self):
        """
        Display the tree structure in a simple text-based format.
        This is a helper method to visualize the tree.
        """
        lines = []
        self._display_recursive(self.root, lines, 0)
        return '\n'.join(lines)

    def _display_recursive(self, node, lines, level):
        """
        Helper method to recursively build the text representation of the tree.
        """
        # Base case: if node is None, return
        if node is None:
            return

        # Display the right subtree first (will appear at the top)
        self._display_recursive(node.right, lines, level + 1)

        # Display the current node
        if level >= len(lines):
            lines.append(' ' * 4 * level + str(node.value))
        else:
            lines[level] = ' ' * 4 * level + str(node.value)

        # Display the left subtree last (will appear at the bottom)
        self._display_recursive(node.left, lines, level + 1)


# Example usage
if __name__ == "__main__":
    # Create a new binary tree
    tree = BinaryTree()

    # Insert some values
    values = [50, 30, 70, 20, 40, 60, 80]
    for value in values:
        tree.insert(value)

    # Display the tree structure
    print("Tree structure:")
    print(tree.display())

    # Perform different tree traversals
    print("Inorder traversal:", tree.inorder_traversal())     # Should print sorted: [20, 30, 40, 50, 60, 70, 80]
    print("Preorder traversal:", tree.preorder_traversal())   # Root first: [50, 30, 20, 40, 70, 60, 80]
    print("Postorder traversal:", tree.postorder_traversal()) # Root last: [20, 40, 30, 60, 80, 70, 50]
```

## Questions

1. **Why not call `Node` when initializing a BinaryTree instance (in `__init__`) ?**

- We start with empty tree (no nodes)
- If we call `Node` in `__init__`, the structure will be different

```python
def __init__(self, root_value=None):
    if root_value is not None:
        self.root = Node(root_value)  # Create a root node with the given value
    else:
        self.root = None  # Empty tree
```

- Advantages:

1. You could create a tree with an initial root in one step
1. It might feel more intuitive if you always want to start with a non-empty tree

- Disadvantages:

1. You lose the ability to create a completely empty tree unless you explicitly pass None
1. It makes the interface slightly more complex
1. The current implementation is more aligned with standard tree implementations that start empty

# Leetcode 144 - Binary Tree Preorder Traversal

## Questions

1. In the iterativer version, why don't we need to mark 'visited nodes'?
2. STACK features FILO. Therefore, we need to push right child to the stack before the left

## Code

```python

# Recursion

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def traverse(node):
            if node is None:
                return

            res.append(node.val)
            traverse(node.left)
            traverse(node.right)

        traverse(root)
        return res

```

```python
# iterative

def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
    if not root:
        return []

    result = []
    stack = [root]

    while stack:
        # Pop the top node from the stack
        node = stack.pop()

        # Visit the node
        result.append(node.val)

        # Push right child first (so that left is processed first)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)

    return result
```

# Leetcode
