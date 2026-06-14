# Bayesian Networks

## 1. What is a Bayesian Network?

A **Bayesian Network (Bayes Net)** is a graphical model that represents probabilistic relationships among variables.

It uses a **directed acyclic graph (DAG)** to describe how variables depend on one another, allowing us to reason about uncertainty efficiently.

### A Bayes Net Consists of

#### 1. Nodes
- Random variables

#### 2. Directed Edges
- Causal or dependency relationships between variables

#### 3. Conditional Probability Tables (CPTs)
- Specify the probability of a node given its parents

For a node \(X\) with parents \(Pa(X)\):

\[
P(X \mid Pa(X))
\]

For root nodes (no parents):

\[
P(X)
\]

which are simply prior probabilities.

---

## 2. Why Bayesian Networks?

Without a Bayes Net, a joint probability distribution grows exponentially with the number of variables.

For \(n\) binary variables:

\[
2^n
\]

possible assignments (combinations of values) exist.

### Example

For 10 binary variables:

\[
2^{10} = 1024
\]

possible assignments.

To represent the full joint distribution, we would need a probability for each assignment.

---

### The Power of Conditional Independence

Bayes Nets dramatically reduce complexity by exploiting **conditional independence**.

#### Example

```text
Cloudy → Rain → WetGrass
```

Instead of storing:

\[
P(C, R, W)
\]

we factorize it as:

\[
P(C)P(R \mid C)P(W \mid R)
\]

This requires far fewer probabilities than storing the entire joint distribution.

---

## 3. Bayes Rule Review

Bayes Rule is the foundation of probabilistic inference:

\[
P(A \mid B)
=
\frac{P(B \mid A)P(A)}
{P(B)}
\]

where:

- \(P(A)\): prior probability
- \(P(B \mid A)\): likelihood
- \(P(B)\): evidence
- \(P(A \mid B)\): posterior probability

---

## 3.1 Cancer Example (Single Test)

### Variables

- Cancer (\(C\))
- Test Result (\(T\))

### Network

```text
Cancer → Test
```

### Given

\[
P(C) = 0.01
\]

\[
P(T=+ \mid C)=0.90
\]

\[
P(T=+ \mid \neg C)=0.05
\]

### Question

Compute:

\[
P(C \mid T=+)
\]

### Solution

Using Bayes Rule:

\[
P(C \mid +)
=
\frac{P(+ \mid C)P(C)}
{P(+)}
\]

where

\[
P(+)
=
P(+ \mid C)P(C)
+
P(+ \mid \neg C)P(\neg C)
\]

This illustrates how a positive test result updates our belief about having cancer.

---

## 3.2 Cancer Example (Two Tests)

### Variables

- Cancer (\(C\))
- Test1 (\(T_1\))
- Test2 (\(T_2\))

### Network

```text
      Cancer
      /    \
   Test1  Test2
```

---

### Conditional Independence

Given Cancer:

\[
T_1 \perp T_2 \mid C
\]

This means:

\[
P(T_1,T_2 \mid C)
=
P(T_1 \mid C)P(T_2 \mid C)
\]

and equivalently:

\[
P(T_1 \mid T_2,C)
=
P(T_1 \mid C)
\]

---

### Intuition

The only reason Test1 and Test2 are related is because both depend on Cancer.

```text
Cancer → Test1
Cancer → Test2
```

If Cancer is already known:

```text
Cancer = True
```

then:

- Test1 positive does not make Test2 more likely
- Test1 negative does not make Test2 less likely

because Cancer already explains everything connecting them.

---

### Important Subtle Point

The statement

\[
T_1 \perp T_2 \mid C
\]

does **not** mean Test1 and Test2 are always independent.

It means they are independent **after conditioning on Cancer**.

Without knowing Cancer, they are generally dependent.

---

### Consequences

The conditional independence assumption allows us to factorize every outcome:

\[
P(T_1=+,T_2=+ \mid C)
=
P(T_1=+ \mid C)
P(T_2=+ \mid C)
\]

\[
P(T_1=+,T_2=- \mid C)
=
P(T_1=+ \mid C)
P(T_2=- \mid C)
\]

\[
P(T_1=-,T_2=+ \mid C)
=
P(T_1=- \mid C)
P(T_2=+ \mid C)
\]

This greatly simplifies inference.

---

## 4. Where Does Conditional Independence Come From?

One of the most important ideas in Bayes Nets is that conditional independence is implied by the graph structure and modeling assumptions.

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

---

### Step 2: Why Are Test1 and Test2 Related?

Suppose we do **not** know whether the patient has cancer.

If:

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

\[
P(T_2=+ \mid T_1=+)
>
P(T_2=+)
\]

and Test1 and Test2 are dependent.

---

### Step 3: What If Cancer Is Known?

Suppose:

```text
Cancer = True
```

Now Test1 no longer provides any new information about Cancer.

Cancer is already known.

The information flow is effectively blocked:

```text
Test1 ← Cancer → Test2
```

Thus:

\[
T_1 \perp T_2 \mid C
\]

---

### Fundamental Bayes Net Assumption

For every node:

> A node is conditionally independent of its non-descendants given its parents.

For Test1:

- Parent = Cancer
- Non-descendant = Test2

Therefore:

\[
Test1 \perp Test2 \mid Cancer
\]

---

## 5. Types of Reasoning

Bayes Nets support multiple forms of inference.

### 5.1 Forward (Causal) Reasoning

Given causes, infer effects.

Example:

\[
P(\text{Traffic} \mid \text{Rain})
\]

---

### 5.2 Backward (Diagnostic) Reasoning

Given evidence, infer causes.

Example:

\[
P(\text{Rain} \mid \text{WetGrass})
\]

---

### 5.3 Intercausal Reasoning

Given an effect, reason about competing causes.

Example:

```text
Rain → WetGrass ← Sprinkler
```

If WetGrass is observed, evidence for Rain can reduce belief in Sprinkler and vice versa.

This phenomenon is called **Explaining Away**.

---

## 6. D-Separation (Graphical Independence)

D-Separation is the graphical method used to determine conditional independence.

There are three fundamental structures.

---

### 6.1 Chain

```text
A → B → C
```

or

```text
A ← B ← C
```

#### Rule

Given \(B\):

\[
A \perp C \mid B
\]

Observing \(B\) blocks the path.

---

### 6.2 Fork (Common Cause)

```text
A ← B → C
```

#### Rule

Given \(B\):

\[
A \perp C \mid B
\]

Observing the common cause blocks the path.

---

### 6.3 Collider

```text
A → B ← C
```

#### Rule

Without observing \(B\):

\[
A \perp C
\]

Observing \(B\):

\[
A \not\!\perp C \mid B
\]

Observing a collider opens the path.

---

## 7. Summary Table

| Structure | Unobserved | Observe Middle Node |
|------------|------------|------------|
| Chain \(A \to B \to C\) | Dependent | Independent |
| Fork \(A \leftarrow B \to C\) | Dependent | Independent |
| Collider \(A \to B \leftarrow C\) | Independent | Dependent |

---

## 8. Key Takeaways

Bayesian Networks are powerful because they:

1. Represent complex probabilistic relationships compactly.
2. Exploit conditional independence to reduce computation.
3. Support causal, diagnostic, and intercausal reasoning.
4. Allow efficient updating of beliefs when new evidence arrives.
5. Provide a graphical method (D-Separation) for determining independence relationships.
6. Factor large joint distributions into smaller, manageable conditional probabilities.

### Core Concepts to Master

- Bayes Rule
- Conditional Probability
- Conditional Independence
- Factorization of Joint Distributions
- Chain, Fork, and Collider Structures
- Explaining Away
- D-Separation
