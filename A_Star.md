### A* Search

A-star is a pathfinding algorithm used to find the lowest-cost path from a start node to a goal node. It is widely used in maps, games, and robotics because it's both complete and optiomal.

f(n) = g(n) + h(n)

- g(n) = cost from start --> current node
- h(n) = estimated cost from current node --> goal(heuristic)
- f(n) = total estimated cost of a path going through node n

It balances what you have already spent (g) and what you expect to spend (h)

#### Steps

1. put the start node in a priority queue
2. repeat:
     - pick node with lowest f(n)
     - if it is the goal --> done
     - otherwise:
         - explore neighbors
         - update their g(n), compute f(n)
         - add/update them in the queue
3. track parents to reconstruct the final path

#### Pseudo Code

```python
def A_star(start, goal):
    # create a priority queue to store nodes we still need to explore
    # the total cost (priority) is f(n), where f(n) = g(n) + h(n)
    # g(n) is the known cost from start to n
    # h(n) is the estimated cost from n to goal
    open_set = priority queue ordered by f(n)

    # Add the start node to the queue
    # Since we have not moved yet, its g-cost is 0
    # so its priority is just h(start)
    open_set.push(start, priority = h(start))

    # Dictionary storing the cheapest known cost from start to each node
    # The star node has cost 0 beacuse we begin there
    g = {start: 0}

    # Dictionary used to rebuild the final path
    # For each node, parent[node] stores the node we came from
    parent = {}

    # Keep searching while there are still nodes to explore
    while open_set is not empty:

        # Remove the node with the loest estimated total cost f(n)
        # this is the node A* believes is currenly most promising
        current = open_set.pop_lowest_f()

        # if we reached the goal, rebuild and return the path by following parent links backward from goal to start
        if current == goal:
            return reconstruct_path(parent, current)

        # Check every node connected to the current node
        for neighbor in neighbors(current):
            # Calculate the cost of reaching this neighbor through the current node
            tentative_g = g[current] + cost(current, neighbor)

            # If we have never seen this neighbor before, or we found a cheaper path to it, update our records
            if neighbor not in g or tentative_g < g[neighbor]:

                # Record that the best path to neighbor currently comes through current
                parent[neighbor] = current

                # Save the new cheapest known cost from start to neighbor
                g[neighbor] = tentative_g

                # Calculate the estimated total cost: actual cost so far + estimated cost to goal
                f = g[neighbor] + h(neighbor)

                # Add the neighbor to the queue, or update its priority if it is already there with a worse cost
                add/update neighbor in open_set with priority f
     # if the queue becomes empty and we never reached the goal, then no path exists
     return failure
```

A-star finds the best path only if heuristic is admissible.

#### Heuristic Function

A heuristic function h(n) is an estimate of the cost to reach the goal from a given node n.

Since finding the true remaining cost is often as hard as solving the whole problem, the heuristic gives a "best guess" based on available information -- allowing search algorithms to prioritize which nodes to explore first.

The **key idea** is, instead of blindly expanding every possible path, a heuristic guides the search toward the goal more efficiently.

##### Important properties

Admissible - never overestimates the true cost. This guarantees A-star finds the optimal solution. Think of it as an "optimistic" guess.

Consistent - the estimate decreases smoothly along edges. h(n) <= cost(n-->n')+h(n'). Your current heuristic estimate should never be more than the actual edge cost plus the heuristic at the next node.

Perfect - equals the exact true cost. Rare in practive, but leads to perfectly efficient search with no wasted exploration.
    
