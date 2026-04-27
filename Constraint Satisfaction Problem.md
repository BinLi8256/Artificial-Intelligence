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

3. Toy Example

Let's use a toy example to run through the algorithm.

- Variables: A, B, C
- Domains: {blue, red} for each of them
- Constraints: A != B, B != C
- Goal: assign colors so neighbors are different

**Steps**

step 0: assignment = {}
step 1: pick variable A

A = Red
assignment = {A: Red}

Step 2: pick variable B

Try values in order: 
   (1) try B = Red: violates A != B
       --> Reject
   (2) try B = Blue: satisfies A != B
       assignment = {A: Red, B: Blue}

Step 3: Pick Variable C

Try values in order:
   (1) try C = Red: satisfies B != C
       assignment = {A: Red, B: Blue, C: Red}

Step 4: Return Solution

# Backtracking Example

Let's use a toy example to run through the algorithm.

## Problem Setup

- **Variables:** A, B, C  
- **Domains:** {blue, red} for each  
- **Constraints:**  
  - A ≠ B  
  - B ≠ C  
- **Goal:** Assign colors so neighbors are different  

---

## Steps

### Step 0
```text
assignment = {}
```

---

### Step 1: Pick variable A
```text
A = Red
assignment = {A: Red}
```

---

### Step 2: Pick variable B

Try values in order:

1. Try `B = Red` ❌  
   Violates A ≠ B → Reject  

2. Try `B = Blue` ✅  
```text
assignment = {A: Red, B: Blue}
```

---

### Step 3: Pick variable C

Try values in order:

1. Try `C = Red` ✅  
   Satisfies B ≠ C  
```text
assignment = {A: Red, B: Blue, C: Red}
```

---

### Step 4: Return Solution 🎉

```text
{A: Red, B: Blue, C: Red}
```
