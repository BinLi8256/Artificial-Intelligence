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
   
**BacktrackingSearch(assignment, cap):** <br>
&nbsp;&nbsp;&nbsp;&nbsp;**if** assignment is complete:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**return** assignment<br>

&nbsp;&nbsp;&nbsp;&nbsp;var = **SelectUnassignedVariable(cap)**<br>

&nbsp;&nbsp;&nbsp;&nbsp;**for** each value **in** OrderDomainValues(var, cap):<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**if** value is consistent with assignment:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assignment[var] = value<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result = BacktrackingSearch(assignment, cap)<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**if** result != failure:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**return** result<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assignment[var] = null<br>

&nbsp;&nbsp;&nbsp;&nbsp;**return** failure<br>
           **return** result<br>
         **return** failure<br>
