### Core Idea

Constraint Satisfaction Problems (CSPs) find values for a set of variables such that all constraints are satisfied. Instead of teaching for a path to a goal, it searches for an assignment that satisfies all the fules.

### Components

It has three components.
1. Variables
   A set of things that need values assigned
2. Domains
   The possible values each variable can take
3. Constraints
   Rules that restrict which combinations of values are allowed

### Types of Constraints

Common constraints are
1. Uniary -- restricts one variable
2. Binary -- restricts two variables
3. Higher-order -- restricts 3+ variables

### How to Solve CSPs

1. Naive approach: generate and test
   + Generate all possible assignments --> test each one
   - problem: exponential number of combinations
2. Backtracking Search (The standard algorithm for CSPs)
   **BacktrackingSearch(assignment, cap):**
     **if** assignment is complete:
       **return** assignment
     var = **SelectUnassignedVariable(cap)
     **for** each value **in** Order DomainValyes(var, cap):
       **if** valueis consistent with assignment:
         assignment[var] = value
         result = BacktrackingSearch(assignment, cap)
         **if** result != failure:
           **return** result
         **return** failure
