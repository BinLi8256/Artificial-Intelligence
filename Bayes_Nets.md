# Bayesian Networks

## 1. What is a Bayesian Network?

A **Bayesian Network (Bayes Net)** is a graphical model that represents probabilistic relationships between variables. It shows how variables depend on each other using a directed graph, allowing us to reason about uncertainty efficiently.

A Bayes Net consists of:

1. **Nodes**
   - Random variables

2. **Directed Edges**
   - Causal or dependency relationships

3. **Conditional Probability Tables (CPTs)**
   - Probability of a node given its parents
   - `P(Node | Parents)`
   - Root nodes (no parents) have prior probabilities: `P(Node)`

---

## 2. Why Bayesian Networks?

Without a Bayes Net, for `n` binary variables, there are:

```math
2^n
```

possible assignments.

For example:

```math
2^{10} = 1024
```

possible combinations for 10 binary variables.

### Why is this a problem?

A full joint probability distribution would require storing a probability for every possible assignment.

As the number of variables grows, the number of combinations grows exponentially.

### The Power of Conditional Independence

Bayes Nets exploit **conditional independence**.

> Once you know a node's parents, it becomes independent of its non-descendants.

### Example

Network:

```text
Cloudy → Rain → WetGrass
```

Instead of storing:

```math
P(C,R,W)
```

we can factorize it as:

```math
P(C)P(R|C)P(W|R)
```

This requires far fewer probabilities.

---

## 3. Bayes Rule Review

Bayes Rule is the foundation of probabilistic inference:

```math
P(A|B) = \frac{P(B|A)P(A)}{P(B)}
```

---

## 3.1 Cancer Example (Single Test)

### Variables

- Cancer (`C`)
- Test Result (`T`)

### Network

```text
Cancer → Test
```

### Given

```math
P(C) = 0.01
```

```math
P(+|C) = 0.90
```

```math
P(+|\neg C) = 0.05
```

### Question

Find:

```math
P(C|+)
```

### Solution

Using Bayes Rule:

```math
P(C|+) =
\frac{P(+|C)P(C)}
     {P(+)}
```

where

```math
P(+)
=
P(+|C)P(C)
+
P(+|\neg C)P(\neg C)
```

This illustrates the **base-rate effect**:

> A positive test does not necessarily imply a high probability of disease.

---

## 3.2 Cancer Example (Two Tests)

### Variables

- Cancer (`C`)
- Test1 (`T1`)
- Test2 (`T2`)

### Network

```text
      Cancer
      /    \
   Test1  Test2
```

### Conditional Independence

Given Cancer:

```math
T_1 \perp T_2 \mid C
```

which means:

```math
P(T_1,T_2|C)
=
P(T_1|C)P(T_2|C)
```

and equivalently:

```math
P(T_1|T_2,C)
=
P(T_1|C)
```

### Interpretation

Once you know whether the patient has cancer, the outcome of Test1 provides no additional information about Test2.

This conditional independence dramatically simplifies inference.

---

### Intuition

Suppose:

```text
Cancer → Test1
Cancer → Test2
```

The only reason the tests are correlated is because both depend on Cancer.

If we know:

```text
Cancer = True
```

then:

- Test1 being positive does not make Test2 more likely.
- Test1 being negative does not make Test2 less likely.

Cancer already explains the relationship.

---

### Important Subtlety

The statement

```math
T_1 \perp T_2 \mid C
```

does **not** mean Test1 and Test2 are always independent.

It means:

> Test1 and Test2 are conditionally independent **after Cancer is known**.

For example:

```math
P(T_1=+,T_2=+|C)
=
P(T_1=+|C)P(T_2=+|C)
```

```math
P(T_1=+,T_2=-|C)
=
P(T_1=+|C)P(T_2=-|C)
```

```math
P(T_1=-,T_2=+|C)
=
P(T_1=-|C)P(T_2=+|C)
```

The statement applies to the entire random variables, not only the positive-positive outcome.

---

## 3.3 Where Does Conditional Independence Come From?

Conditional independence comes from:

1. The structure of the Bayesian Network
2. The modeling assumptions encoded by the network

---

### Step 1: Network Structure

Consider:

```text
      Cancer
      /    \
   Test1  Test2
```

This means:

```text
Cancer causes Test1
Cancer causes Test2
```

This structure is called a **fork** or **common-cause structure**.

---

### Step 2: Why Are Test1 and Test2 Related?

Suppose we do **not** know whether the patient has cancer.

If we observe:

```text
Test1 = Positive
```

then Cancer becomes more likely.

If Cancer becomes more likely, then:

```text
Test2 = Positive
```

also becomes more likely.

Therefore:

```text
Test1 and Test2 are dependent
```

Mathematically:

```math
P(T_2=+|T_1=+)
>
P(T_2=+)
```

because Test1 provides information about Cancer.

---

### Step 3: What Happens When Cancer Is Known?

Suppose we know:

```text
Cancer = True
```

Now Test1 no longer provides information about Cancer.

Cancer is already known.

The information flow is cut off.

As a result:

```math
T_1 \perp T_2 \mid C
```

---

### Fundamental Bayes Net Assumption

For every node:

> A node is conditionally independent of its non-descendants given its parents.

For example:

```text
      Cancer
      /    \
   Test1  Test2
```

Given Cancer, Test1 is independent of everything else that is not its descendant.

Therefore:

```math
T_1 \perp T_2 \mid C
```

---

## 4. Types of Reasoning

Bayesian Networks support several forms of reasoning.

### 4.1 Forward (Causal) Reasoning

Given causes, infer effects.

Example:

```math
P(\text{Traffic}|\text{Rain})
```

```text
Rain → Traffic
```

---

### 4.2 Backward (Diagnostic) Reasoning

Given evidence, infer causes.

Example:

```math
P(\text{Rain}|\text{WetGrass})
```

```text
Rain → WetGrass
```

---

### 4.3 Intercausal Reasoning

Given an effect, reason about competing causes.

Example:

```text
Rain → WetGrass ← Sprinkler
```

If WetGrass is observed, evidence for Rain can reduce belief in Sprinkler and vice versa.

This phenomenon is called:

**Explaining Away**

---

## 5. D-Separation (Independence in Graphs)

D-Separation provides a graphical method for determining conditional independence.

Three fundamental structures appear repeatedly:

1. Chain
2. Fork
3. Collider

---

### 5.1 Chain

Structure:

```text
A → B → C
```

Without observing B:

```text
A and C are dependent
```

Given B:

```text
A ⊥ C | B
```

Observing B blocks the path.

---

### 5.2 Fork (Common Cause)

Structure:

```text
A ← B → C
```

Without observing B:

```text
A and C are dependent
```

Given B:

```text
A ⊥ C | B
```

Observing the common cause blocks the path.

---

### 5.3 Collider

Structure:

```text
A → B ← C
```

Without observing B:

```text
A ⊥ C
```

Given B:

```text
A and C become dependent
```

Observing the collider opens the path.

This is the basis of **explaining away**.

---

## 6. Summary

Bayesian Networks are powerful because they:

1. Represent complex probabilistic relationships compactly.
2. Exploit conditional independence to simplify computation.
3. Support multiple forms of reasoning:
   - Causal
   - Diagnostic
   - Intercausal
4. Update beliefs efficiently as new evidence arrives.
5. Provide a graphical framework for understanding independence through D-Separation.

### Key Formula

Joint distributions factorize according to the network structure:

```math
P(X_1,\ldots,X_n)
=
\prod_i P(X_i \mid Parents(X_i))
```

This factorization is the primary reason Bayesian Networks scale far better than storing a full joint probability table.
