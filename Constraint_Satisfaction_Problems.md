### 1. Core Idea

Constraint Satisfaction Problems (CSPs) fine values for a set of variables such that all constraints are satisfied. Unlike search algorithms, a CSP is not trying to find a path from a start state to a goal state. It tries to find a complete assignment of values to variables that satisfies all rules.

A useful way to think about it:

``````
Search Problems:
    How do I get there?

CSPs:
    How should I fill in the blanks?
``````

**Example**

``````
A = ?
B = ?
C = ?
``````

subject to:

`````
A ≠ B
B ≠ C
`````

### 2. Components

Every CSP consists of three pieces.

(1) variables

Things that need values assigned.

Example:

```
A, B, C
```

or

```
meeting 1
meeting 2
meeting 3
```

(2) domains
Possible values each variable can take.

```
A ∈ {Red, Blue}
B ∈ {Red, Blue}
C ∈ {Red, Blue}
```

(3) constraints
Rules restricting allowed assignments.

```
A ≠ B
B ≠ C
```

A solution is valid only if all constraints are satisfied simultaneously.


### 3. Types of Constraints

**Unary Constrain**
Restricts a single variable.

Example:
```
A ≠ Red
```
or
```
Meeting 1 must be on Monday
```

**Binary Constraint**
Restricts two variables.

Example:
```
A ≠ B
```
or
```
Meeting 1 and Meeting 2 cannot occur at the same time
```
Most CSP algorithms are designed around binary constraints.

**Higher-Order Constraint**
Involves three or more variables.

Example:
```
A + B + C = 10
```
or 
```
At least one of A, B, C must be Red
```

### 4. How to Solve CSPs
#### Naive Approach: Generate and Test

**Idea**

Generate every possible assignment and test whether it satisfies the constraints.

Example
```
A ∈ {R,B}
B ∈ {R,B}
C ∈ {R,B}
```

Total assignments:
```
2 × 2 × 2 = 8
```
Try all 8 possibilities.

**Problem**
For:
```
100 variables
10 values each
```
the search space becomes
```
10^100
```
which is impossible.

#### Backtracking Search
The standard CSP algorithm.

**Key Idea**
Instead of generating a complete assignment first like:
```
Generate everything
↓
Check constraints
```
we do:
```
Assign one variable
↓
Check constraints immediately
↓
Continue only if valid
```
This prunes huge parts of the search tree.

**Backtracking Algorithm**

```
BacktrackingSearch(assignment, csp):
    if assignment is complete:
        return assignment

    var = SelectUnassignedVariable(csp)

    for each value in OrderDomainValues(var, csp):
        if value is consistent with assignment:
            assignment[var] = value
            result = BacktrackingSearch(assignment, csp)

            if result != failure:
                return result
            assignment[var] = null
    return failure
```

**Mental Model**
Backtracking is simple:
```
Try
↓
Check
↓
Go deeper

If fail:
↓
Undo
↓
Try another option
```

### 5. Backtracking Example
Backtracking never explores branches that already violate constraints.
```
A = Red
↓
B = Red   → fail
↓
undo
↓
B = Blue
↓
continue
```

### 6. Heuristics: Making Backtracking Smarter

Backtracking works, but variable ordering matters.

Suppose we have
```
A connected to 10 constraints
B connected to 2 constraints
```
Which should we assign first?
If we choose poorly, we may waster a lot of search. This motives heuristics.

### 7. Degree Heuristic
Backtracking must choose an unassigned variable.

Degree Heuristic answers:
```
Which variable should I assign next?
```
Rule:
```
Choose the variable involved in the largest number of constraints.
```
Why?
Because assigning it early will constrain many other variables and reveal conflicts sooner.

**Example**
Constraint graph:
```
       A
     / | \
    B  C  D
```

Constraints:
```
A ≠ B
A ≠ C
A ≠ D
```

Degrees:
```
A = 3
B = 1
C = 1
D = 1
```
Choose **A** first


















