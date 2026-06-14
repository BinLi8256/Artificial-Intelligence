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

possible assignments (combinations of values).

For example:

```math
2^{10}=1024
```

possible assignments for 10 binary variables.

### Why is this a problem?

A complete joint probability distribution would require storing a probability for every assignment.

For 10 binary variables:

```math
1024
```

possible assignments.

For 20 binary variables:

```math
2^{20}=1,048,576
```

possible assignments.

The storage requirement grows exponentially.

---

### The Power of Conditional Independence

Bayes Nets exploit **conditional independence**.

> Once you know a node's parents, it becomes independent of its non-descendants.

This dramatically reduces the number of probabilities we need to store.

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

which requires far fewer probabilities.

---

## 3. Factorization of Joint Distributions

The most important property of a Bayesian Network is that the joint probability distribution can be factorized according to the graph structure.

For a network with variables:

```text
X₁, X₂, ..., Xₙ
```

the joint distribution is:

```math
P(X_1,\ldots,X_n)
=
\prod_i P(X_i \mid Parents(X_i))
```

In words:

> The probability of the entire world is the product of each node conditioned on its parents.

This factorization is what makes Bayesian Networks scalable.

---

### Example

Consider:

```text
        Cloudy
        /    \
     Rain  Sprinkler
        \    /
       WetGrass
```

The joint distribution is:

```math
P(C,R,S,W)
=
P(C)
P(R|C)
P(S|C)
P(W|R,S)
```

Instead of storing probabilities for all possible assignments, we only store the CPTs associated with each node.

---

## 4. Bayes Rule Review

Bayes Rule is the foundation of probabilistic inference.

```math
P(A|B)
=
\frac{P(B|A)P(A)}
     {P(B)}
```

Interpretation:

- `P(A)` = Prior belief
- `P(B|A)` = Likelihood
- `P(A|B)` = Posterior belief
- `P(B)` = Evidence

Bayes Rule tells us how to update our beliefs when new evidence arrives.

---

## 5. Cancer Example (Single Test)

### Variables

- Cancer (`C`)
- Test Result (`T`)

### Network

```text
Cancer → Test
```

### Given

```math
P(C)=0.01
```

```math
P(+|C)=0.90
```

```math
P(+|\neg C)=0.05
```

### Question

Find:

```math
P(C|+)
```

### Solution

Using Bayes Rule:

```math
P(C|+)
=
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

## 6. Conditional Independence

Conditional independence is the key idea that makes Bayesian Networks efficient.

### Definition

We say:

```math
A \perp B \mid C
```

if:

```math
P(A,B|C)
=
P(A|C)P(B|C)
```

Equivalently:

```math
P(A|B,C)
=
P(A|C)
```

Meaning:

> Once C is known, knowing B provides no additional information about A.

---

## 7. Cancer Example (Two Tests)

### Network

```text
      Cancer
      /    \
   Test1  Test2
```

### Conditional Independence

Given Cancer:

```math
Test1 \perp Test2 \mid Cancer
```

which means:

```math
P(T_1,T_2|C)
=
P(T_1|C)P(T_2|C)
```

and

```math
P(T_1|T_2,C)
=
P(T_1|C)
```

---

### Intuition

The only reason Test1 and Test2 are correlated is because both depend on Cancer.

If Cancer is unknown:

```text
Test1 = Positive
```

makes Cancer more likely.

And if Cancer becomes more likely:

```text
Test2 = Positive
```

also becomes more likely.

Therefore:

```text
Test1 and Test2 are dependent
```

---

If we know:

```text
Cancer = True
```

then Test1 no longer provides information about Cancer.

The information pathway is removed.

Therefore:

```math
Test1 \perp Test2 \mid Cancer
```

---

### Important Subtlety

The statement:

```math
Test1 \perp Test2 \mid Cancer
```

does **not** mean Test1 and Test2 are always independent.

It means they become independent **after Cancer is known**.

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

---

## 8. Where Does Conditional Independence Come From?

Conditional independence comes from:

1. The graph structure.
2. The modeling assumptions of the Bayesian Network.

### Fundamental Assumption

For every node:

> A node is conditionally independent of its non-descendants given its parents.

Example:

```text
      Cancer
      /    \
   Test1  Test2
```

Given Cancer, Test1 is independent of everything else that is not its descendant.

Therefore:

```math
Test1 \perp Test2 \mid Cancer
```

---

## 9. Types of Reasoning

Bayesian Networks support several forms of reasoning.

### 9.1 Forward (Causal) Reasoning

Given causes, infer effects.

Example:

```text
Rain → Traffic
```

Question:

```math
P(Traffic|Rain)
```

---

### 9.2 Backward (Diagnostic) Reasoning

Given effects, infer causes.

Example:

```text
Rain → WetGrass
```

Question:

```math
P(Rain|WetGrass)
```

---

### 9.3 Intercausal Reasoning

Given an observed effect, reason about competing causes.

Example:

```text
Rain → WetGrass ← Sprinkler
```

If WetGrass is observed, evidence for Rain can reduce belief in Sprinkler and vice versa.

This phenomenon is called:

**Explaining Away**

---

## 10. Common Inference Questions

Bayesian Networks are used to answer questions such as:

```math
P(Cancer|PositiveTest)
```

```math
P(Rain|WetGrass)
```

```math
P(Burglary|Alarm)
```

```math
P(Disease|Symptoms)
```

The goal is:

> Compute posterior beliefs after observing evidence.

---

## 11. D-Separation

D-Separation is the graphical method used to determine conditional independence.

### Key Idea

Two variables are conditionally independent if every path between them is blocked.

---

## 12. Three Fundamental Structures

### 12.1 Chain

```text
A → B → C
```

Without observing B:

```text
A and C are dependent
```

Given B:

```math
A \perp C \mid B
```

Observing B blocks the path.

---

### 12.2 Fork (Common Cause)

```text
A ← B → C
```

Without observing B:

```text
A and C are dependent
```

Given B:

```math
A \perp C \mid B
```

Observing the common cause blocks the path.

---

### 12.3 Collider

```text
A → B ← C
```

Without observing B:

```math
A \perp C
```

The path is blocked by default.

Given B:

```text
A and C become dependent
```

Observing the collider opens the path.

This is the basis of explaining away.

---

## 13. D-Separation Rules

A path is blocked if:

### Rule 1: Chain

```text
A → B → C
```

Observing B blocks the path.

---

### Rule 2: Fork

```text
A ← B → C
```

Observing B blocks the path.

---

### Rule 3: Collider

```text
A → B ← C
```

The path is blocked by default.

Observing B opens the path.

Observing a descendant of B also opens the path.

This is the most commonly missed exam rule.

---

## 14. Cheat Sheet

| Structure | Unobserved | Observe Middle |
|------------|------------|------------|
| Chain `A→B→C` | Dependent | Independent |
| Fork `A←B→C` | Dependent | Independent |
| Collider `A→B←C` | Independent | Dependent |

Memorizing this table solves many Bayes Net independence questions.

---

## 15. Learning vs Inference

### Inference

The graph and CPTs are known.

Question:

```math
P(Cancer|Positive)
```

Goal:

> Compute probabilities using the existing model.

---

### Learning

The graph or CPTs are unknown.

Goal:

> Learn the probabilities or graph structure from data.

Example:

```text
Medical records
→ estimate disease probabilities
→ estimate test reliability
```

Inference uses the model.

Learning builds the model.

---

## 16. Bayesian Networks in Artificial Intelligence

Bayesian Networks are one of the foundational tools in Artificial Intelligence because AI systems must reason under uncertainty.

Real-world AI systems face:

- Noisy sensors
- Missing information
- Ambiguous observations
- Hidden causes

Bayesian Networks provide a mathematical framework for handling these uncertainties.

---

### Applications

#### Expert Systems

```text
Disease → Symptoms
```

Medical diagnosis and troubleshooting systems.

---

#### Robotics

```text
Robot Position → Sensor Reading
```

Used for localization and sensor fusion.

---

#### Autonomous Vehicles

Combining information from:

- Cameras
- Radar
- LiDAR
- GPS

to estimate the state of the environment.

---

#### Natural Language Processing

Examples:

- Spam filtering
- Document classification
- Naive Bayes classifiers

---

#### Computer Vision

Estimating:

- Object identities
- Scene structure
- Hidden causes behind image observations

---

## 17. Bayesian Networks in the AI Landscape

Bayesian Networks belong to the family of **probabilistic reasoning methods**.

Major AI topics include:

- Search (BFS, UCS, A*)
- Constraint Satisfaction Problems (CSPs)
- Bayesian Networks
- Markov Decision Processes (MDPs)
- Reinforcement Learning
- Machine Learning

Each addresses a different aspect of intelligent behavior.

Bayesian Networks specifically focus on:

> Reasoning under uncertainty.

---

## 18. Exact Inference Algorithms

For small networks, probabilities can be computed directly.

For larger networks, AI uses specialized algorithms:

- Enumeration
- Variable Elimination
- Belief Propagation

These methods avoid enumerating the full joint distribution.

---

## 19. Summary

Bayesian Networks are powerful because they:

1. Represent complex probabilistic relationships compactly.
2. Exploit conditional independence to reduce computation.
3. Support causal, diagnostic, and intercausal reasoning.
4. Update beliefs when new evidence arrives.
5. Provide a graphical framework for understanding independence.
6. Enable intelligent agents to reason under uncertainty.

### Most Important Formula

```math
P(X_1,\ldots,X_n)
=
\prod_i P(X_i \mid Parents(X_i))
```

### Most Important Assumption

> A node is conditionally independent of its non-descendants given its parents.

### Most Important Table

| Structure | Unobserved | Observe Middle |
|------------|------------|------------|
| Chain | Dependent | Independent |
| Fork | Dependent | Independent |
| Collider | Independent | Dependent |

If you understand:
- Factorization
- Bayes Rule
- Conditional Independence
- Chain/Fork/Collider
- D-Separation
- Explaining Away

then you understand the core concepts of Bayesian Networks used in AI.
