### Bayes Nets

Bayes nets are a graphical model that represents probabilistic relationships between variables. It shows how variables depend on each other using a directed graph, allowing us to reason about uncertainty efficiently.

Bayes nets use conditioanl probability tables (CPTs) often. Each node has a table showing:
  1. P(node|parents): probability of the node given its parents' value
  2. P(node): root nodes (no parents) just have prior probabilities

The power of Bayes Nets comes from conditional independence. Once you know a node's parents, it becomes independent of its non-descendants.

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

