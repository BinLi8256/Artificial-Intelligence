### Minimax

**Minimax** is a decision-making algorithm for two-palyer zero sum games. One player tries to maximize their score, the other tries to minimize it. Each player assumes the opponent plays perfectly.

Minimax **assumes** both players always make the optional move. This is what mnakes it powerful -- it prepares for the worse case scenario rather than hoping the opponent makes mistakes. **MAX** knows **MIN** will always pick the worst outcome for **MAX**, so **MAX** picks the best of those worst cases.


### Pseudo Code

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
