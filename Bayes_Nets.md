### 1. What is a Bayesian Network?

Bayes nets are a graphical model that represents probabilistic relationships between variables. It shows how variables depend on each other using a directed graph, allowing us to reason about uncertainty efficiently.

A Bayes Net Consists of:
  1. Nodes
     - random variables
  2. Directed Edges
     - causal or dependency relationships
  3. Conditional Probability Tables (CPTs)
     - Probability of a node given its parents
     - P(node|parents): probability of the node given its parents' value
     - P(node): root nodes (no parents) just have prior probabilities

### 2. Why Bayesian Networks?
Without a Bayes Net, for a binary variables, there would be 2^n possible assignments. For example, 10 binary variables would have 2^10 combinations.

The power of Bayes Nets comes from conditional independence. Once you know a node's parents, it becomes independent of its non-descendants.

Exmaple:
``
cloudy -> rain -> wetgrass
``
joint distribution
``
P(C,R,W)
``
becomes 
``
P(C)P(R|C)P(W|R)
``
much fewer probabilities.

### 3. Bayes Rule Review

```
$P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}$
```

#### 3.1 Cancer Example (Single Test)
- Variables: cancer, test
- Network: cancer -> test
- Suppose: P(C) = 0.01, P(+|C) = 0.9, P(+|-C) = 0.05
- Question: P(C|+) = ?
- Solution
  ``
  Use Bayes Rule:
                  P(C|+) = P(+|C)P(C)/(P+)
  where P(+) = P(+|C)P(C) + P(+|-C)P(-C)
  ``
#### 3.2 Two-Test Cancer Example
- Variables: cancer, test1, test2
- Network:
  ``
        Cancer
      /    \
   Test1  Test2
  ``
- Given cancer,
  ```
  Test1 ⊥ Test2 | Cancer --> P(T1, T2|C) = P(T1|C)P(T2|C)

  and also

  P(T1|T2,C) = P(T1|C)
  ``
 (The notation means: once you know whether the patient has cancer, the outcome of test1 tells you nothing more about test2. This conditional independence dramatically simplifies inference.)

 **What does that mean intuitively?**
 Suppose 
 ``
 Cancer → Test1
 Cancer → Test2
 ``

 The only reason the tests are correlated is because both are affected by Cancer. 
 If
 ``
 Cancer = True
 ``
 Then,
 + Test1 positive doesn't make Test2 more likely
 + Test1 negative doesn't make Test2 less likely
 because Cancer already explains everything connecting them.

** The Subtle Point **
``
Test1 ⊥ Test2 | Cancer
``
does not mean T1 and T2 are independent. It means the random varaibles Test1 and Test2 are conditionally independent given Cancer.

As a consequence, every combination factorizes:

``
P(T1 = +, T2 = +|C) = P(T1 = +|C)P(T2 = +|C)
P(T1 = +, T2 = -|C) = P(T1 = +|C)P(T2 = -|C)
P(T1 = -, T2 = +|C) = P(T1 = -|C)P(T2 = +|C)
``

So, the statement is about the entire variables, not just the positive-positive outomes.

**How does the conditional independence relationship come from?**

This is one of the most important concepts in Bayes Nets.

The short answer is, conditional independence comes from the structure of the Bayesian Networ and the modeling assumptions behind it.

**Step 1: The Network Structure**
Suppose we have
``
      Cancer
      /    \
   Test1  Test2
``

which means
``
Cancer causes Test1 result
Cancer causes Test2 result
``



#### Types of Reasoning
  1. Forward (causal): given causes, infer effects: P(traffic | rain)
  2. Backward (diagnostic): given evidence, infer causes: P(rain | wet grass)
  3. Intercausal: given evidence of effect, reason about competing causes. E.g. sprinkler explains away the wet grass

#### D-Separation (Independence in Graphs)

Three patterns that determine independece: chain, fork, and collider.

**Chain**
A --> B --> C: A and C are independent given B (B blocks the path between A and C)

**Fork**
A <-- B --> C: A and C are independent given B (B is a common cause)

**Collider**
A --> B <-- C: A and C are dependent given B (Observing B opens the path)

#### Summary
Bayes nets are powerful because they:
  1. represent complex probabilistic relationships compactly
  2. exploit conditional independence to simplify computation
  3. support reasoning in multiple directions (causal, diagnostic)
  4. update belifs efficiently as new evidence arrives

