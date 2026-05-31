# Simulated Annealing (SA)

## Overview

**Simulated Annealing (SA)** is a search and optimization algorithm that sometimes accepts a worse solution on purpose to avoid getting stuck in local optima.

The idea comes from metallurgy: when cooling molten metal, the material starts at a high temperature and is gradually cooled. At high temperatures, atoms can move freely; at low temperatures, they settle into a stable structure.

---

## The Problem It Solves

Suppose we want to minimize a function:

\[
f(x) = x^4 - 14x^2 + 24x
\]

The search landscape might look like:

```text
      Global Minimum
            v
       __
      /  \__
 __  /       \__
/  \/           \__

^
Local Minimum
```

A simple Hill Climbing algorithm works as follows:

```text
Current State
      ↓
Look at Neighbors
      ↓
Move to a Better Neighbor
```

### Problem

```text
      __
     /  \
 ___/    \____
    ^
    Stuck Here
```

Hill Climbing gets trapped in a local minimum because every neighboring move appears worse.

---

## Key Idea

Instead of only accepting better moves:

- Better move → Always accept
- Worse move → Sometimes accept

The probability of accepting a worse move depends on:

1. How much worse the new solution is
2. The current temperature

### High Temperature

```text
Accept many bad moves
Explore aggressively
```

### Low Temperature

```text
Accept very few bad moves
Act more greedily
```

---

## Acceptance Probability

Suppose:

```text
Current Cost  = 10
Neighbor Cost = 12
```

The solution became worse by:

```text
ΔE = 12 - 10 = 2
```

The acceptance probability is:

$P = e^{-\Delta E / T}$


where:

- \$ \Delta E $ = increase in cost
- \( T \) = current temperature

---

### Example 1: High Temperature

```text
ΔE = 2
T = 100
```

\[
P = e^{-2/100} \approx 0.98
\]

**98% chance of accepting the worse move.**

---

### Example 2: Low Temperature

```text
ΔE = 2
T = 0.1
```

\[
P = e^{-20} \approx 2 \times 10^{-9}
\]

**Almost never accepted.**

---

## Algorithm

1. Start with a random solution.
2. Set an initial temperature \(T\).
3. Repeatedly generate neighboring solutions.
4. Always accept better solutions.
5. Sometimes accept worse solutions according to the acceptance probability.
6. Gradually reduce the temperature.
7. Stop when the temperature becomes sufficiently low.

---

## Pseudocode

```python
current = random_state()
T = initial_temperature

while T > minimum_temperature:

    neighbor = random_neighbor(current)

    delta = cost(neighbor) - cost(current)

    if delta < 0:
        current = neighbor

    else:
        p = exp(-delta / T)

        if random() < p:
            current = neighbor

    T *= cooling_rate

return current
```

---

## Cooling Schedule

The temperature gradually decreases during the search.

A common cooling schedule is:

\[
T_{new} = \alpha T_{old}
\]

where:

```text
α = 0.95
or
α = 0.99
```

Example:

```text
100
95
90.25
85.74
81.45
...
```

In general:

- Slow cooling → Better solutions, longer runtime
- Fast cooling → Faster runtime, lower solution quality

---

## Why It Works

Early in the search:

```text
High Temperature
      ↓
Accept many bad moves
      ↓
Escape local optima
      ↓
Explore the search space
```

Later in the search:

```text
Low Temperature
      ↓
Reject most bad moves
      ↓
Focus on promising solutions
      ↓
Converge to a good solution
```

As the temperature approaches zero, Simulated Annealing behaves increasingly like Hill Climbing.

---

## When to Use Simulated Annealing

Simulated Annealing is useful when:

- The state space is enormous
- Exact search is computationally expensive
- Many local optima exist
- A good solution is sufficient (optimality is not required)

---

## Common Applications

- Traveling Salesman Problem (TSP)
- Scheduling
- Vehicle Routing
- Chip/Layout Design
- Hyperparameter Tuning
- Resource Allocation

---

## Advantages

- Can escape local optima
- Simple to implement
- Works well on large optimization problems

---

## Disadvantages

- No guarantee of finding the global optimum
- Performance depends on the cooling schedule
- May require many iterations

---

## Key Takeaway

> Simulated Annealing is essentially Hill Climbing with controlled randomness. By occasionally accepting worse moves—especially early in the search—it can escape local optima and explore more of the search space before gradually becoming greedy as the temperature decreases.
