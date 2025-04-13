```python

from collections import deque

# ======= Breadth-First Search (BFS) =======

def bfs(graph, start_node):
"""
Breadth-First Search algorithm implementation.

    Args:
        graph: Dictionary representing adjacency list of the graph
        start_node: Starting node to begin the search

    Returns:
        List of nodes in the order they were visited
    """
    # Initialize a queue for BFS traversal
    queue = deque([start_node])

    # Initialize set to keep track of visited nodes
    # We mark the start node as visited BEFORE processing it
    # This prevents the node from being added to the queue multiple times
    visited = {start_node}

    # List to store the order in which nodes are visited
    result = []

    while queue:
        # Dequeue a node from the front of the queue
        current_node = queue.popleft()

        # Process the current node (here we just add it to our result)
        result.append(current_node)

        # Visit all adjacent nodes of the current node
        for neighbor in graph[current_node]:
            # Only process nodes that haven't been visited yet
            if neighbor not in visited:
                # Mark the neighbor as visited IMMEDIATELY when discovered
                # This is crucial in BFS to avoid adding the same node to the queue multiple times
                visited.add(neighbor)

                # Add neighbor to the queue for later processing
                queue.append(neighbor)

    return result

# ======= Depth-First Search (DFS) - Recursive Implementation =======

def dfs_recursive(graph, start_node):
"""
Recursive Depth-First Search algorithm implementation.

    Args:
        graph: Dictionary representing adjacency list of the graph
        start_node: Starting node to begin the search

    Returns:
        List of nodes in the order they were visited
    """
    visited = set()  # Initialize visited set outside the helper function
    result = []      # Initialize result list to track traversal order

    def dfs_helper(node):
        # Mark the current node as visited BEFORE exploring its neighbors
        # This prevents infinite loops in case of cycles
        visited.add(node)

        # Process the current node
        result.append(node)

        # Recursively visit all adjacent nodes
        for neighbor in graph[node]:
            # Only visit neighbors that haven't been visited yet
            if neighbor not in visited:
                dfs_helper(neighbor)

    # Start DFS from the specified start node
    dfs_helper(start_node)
    return result

# ======= Depth-First Search (DFS) - Iterative Implementation =======

def dfs_iterative(graph, start_node):
"""
Iterative Depth-First Search algorithm implementation using a stack.

    Args:
        graph: Dictionary representing adjacency list of the graph
        start_node: Starting node to begin the search

    Returns:
        List of nodes in the order they were visited
    """
    # Initialize a stack for DFS traversal
    stack = [start_node]

    # Initialize set to keep track of visited nodes
    # In iterative DFS, we have two options for when to mark nodes as visited:
    # 1. When pushed onto the stack (like in BFS) - PREFERRED FOR MOST CASES
    # 2. When popped from the stack (during processing)

    # We'll use option 1 here, which is similar to BFS approach
    visited = {start_node}  # Mark start node as visited immediately

    # List to store the order in which nodes are visited
    result = []

    while stack:
        # Pop a node from the top of the stack
        current_node = stack.pop()

        # Process the current node
        result.append(current_node)

        # Visit all adjacent nodes of the current node
        # Note: we iterate in reverse order to maintain the same traversal
        # order as the recursive version (optional)
        for neighbor in reversed(graph[current_node]):
            if neighbor not in visited:
                # Mark the neighbor as visited IMMEDIATELY when discovered
                # This prevents adding the same node to the stack multiple times
                visited.add(neighbor)

                # Add neighbor to the stack for later processing
                stack.append(neighbor)

    return result

# Example usage:

if **name** == "**main**": # Example graph represented as an adjacency list
example_graph = {
'A': ['B', 'C'],
'B': ['A', 'D', 'E'],
'C': ['A', 'F'],
'D': ['B'],
'E': ['B', 'F'],
'F': ['C', 'E']
}

    print("BFS Traversal:", bfs(example_graph, 'A'))
    print("DFS Traversal (Recursive):", dfs_recursive(example_graph, 'A'))
    print("DFS Traversal (Iterative):", dfs_iterative(example_graph, 'A'))

    # Expected output:
    # BFS Traversal: ['A', 'B', 'C', 'D', 'E', 'F']
    # DFS Traversal (Recursive): ['A', 'B', 'D', 'E', 'F', 'C']
    # DFS Traversal (Iterative): ['A', 'C', 'F', 'E', 'B', 'D']

"""
```

KEY DIFFERENCES IN VISITED NODE MARKING:

1. BFS (Breadth-First Search):

   - Marks nodes as visited WHEN DISCOVERED (before adding to the queue)
   - This ensures each node is added to the queue at most once
   - Guarantees the shortest path (in terms of number of edges) in unweighted graphs

2. DFS (Depth-First Search):

   - Recursive: Marks nodes as visited BEFORE exploring neighbors
   - Iterative: Can mark nodes either when discovered (pushed to stack) or
     when processed (popped from stack)
   - Marking when discovered (as shown in the code) is generally more efficient
     as it prevents adding duplicate nodes to the stack

3. Why the timing of marking matters:

   - If we don't mark nodes as visited early enough, we risk:

     - Processing the same node multiple times
     - Creating infinite loops in graphs with cycles
     - Wasting memory with redundant queue/stack entries

   - In BFS, marking as visited right before enqueueing is CRITICAL because
     a node might be discovered multiple times at the same level before it's
     processed

   - In DFS recursive, marking as visited at the start of the function is
     necessary to prevent infinite recursion in graphs with cycles
     """
