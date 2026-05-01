### Minimax

**Minimax** is a decision-making algorithm for two-palyer zero sum games. One player tries to maximize their score, the other tries to minimize it. Each player assumes the opponent plays perfectly.

Minimax **assumes** both players always make the optional move. This is what mnakes it powerful -- it prepares for the worse case scenario rather than hoping the opponent makes mistakes. **MAX** knows **MIN** will always pick the worst outcome for **MAX**, so **MAX** picks the best of those worst cases.


#### Pseudo Code

```python
def Minimax(state, isMaximizing):
    if state is terminal (game over):
        return utility(state)          # actual score of outcome

    if isMaximizing:                  # MAX player's turn
        bestScore = -infinity
        for each possible move:
            child = apply move to state
            score = Minimax(child, False)  # opponent's turn next
            bestScore = max(bestScore, score)
        return bestScore

    else:                             # MIN player's turn
        bestScore = +infinity
        for each possible move:
            child = apply move to state
            score = Minimax(child, True)   # our turn next
            bestScore = min(bestScore, score)
        return bestScore
```


### Alpha-Beta Prunning

Alpha-Beta pruning is an optimization of minimax that skips evaluating branches that cannot possibly affect the final decision. It produces the exact same result as minimax but much faster.

#### The Two Parameters

Alpha-beta tracks two values at all times:

Alpha --> MAX player --> Best value MAX has found so far (MAX won't accept anything lower)
Beta  --> MIN player --> Best value MIN has found so far (MIN won't accept anything higher)

They get passes down the tree as you explore

#### Prunning Conditions

At a MIN node: if current value <= alpha --> PRUNE (MAX won't choose this)
At a MAX node: if current value >= beta  --> PRUNE (MIN won't choose this)

