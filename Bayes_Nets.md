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
P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}
```

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

