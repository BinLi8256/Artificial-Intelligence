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

def A_start(start, goal):
    open_set = priority queue ordered by f(n)
    open_set.push(start)
    g = {start:0}
    parent = {}
    while open_set is not empty:
      current = node with lowesr f(n)
      if current == goal:
        return reconstruct_path(parent, current)
      for neighbor in neighbors(current):
        tentative_g = g[current] + cost(current, neighbor)
        if neighbor not in g or tentative_g < g[neighbor]:
          parent[neighbor] = current
          g[neighbor] = tentative_g
          f = g[neighbor] + h(neighbor)
          add/update neighbor in open_set with priority f

A-star finds the best path only if heuristic is admissible.

#### Heuristic Function

A heuristic function h(n) is an estimate of the cost to reach the goal from a given node n.

Since finding the true remaining cost is often as hard as solving the whole problem, the heuristic gives a "best guess" based on available information -- allowing search algorithms to prioritize which nodes to explore first.

The **key idea" is, instead of blindly expanding every possible path, a heuristic guides the search toward the goal more efficiently.

##### Important properties

Admissible - never overestimates the true cost. This guarantees A-star finds the optimal solution. Think of it as an "optimistic" guess.

Consistent - the estimate decreases smoothly along edges. h(n) <= cost(n-->n')+h(n'). Your current heuristic estimate should never be more than the actual edge cost plus the heuristic at the next node.

Perfect - equals the exact true cost. Rare in practive, but leads to perfectly efficient search with no wasted exploration.
    
