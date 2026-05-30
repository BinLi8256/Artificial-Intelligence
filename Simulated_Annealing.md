### Simulated Annealing

#### Overview
  
**Simulated Annealing (SA)** is a search/optimization algorithm that sometimes accepts a worse solution on purpose to aviod getting stuck in local optima.

The idea comes from metallurgy: when cooling molten metal, you start a high temperature and gradually cool it. High temperature allows atoms to move freely, low temperature locks them into a stable structure.

#### The Problem It Solves

Suppose you are tring to minimize a function f(x)=x^4-14x^2+24x. The landscape might look like:

      Global Minimum
            v
       __
      /  \__
 __  /       \__
/  \/           \__

^
Local Minimum

A simple hill-climbing algorithm does:

curent state --> look at neighbors --> move to a better neighbor.

Problem:

      __
     /  \
 ___/    \____
    ^
    Stuck here

Hill climbing gets tracpped in a local minimum because every nearby move is worse.

#### Key Idea

Instead of always accepting better moves, it always accepts a better move and sometimes accepts a worse move. The chance of accepting a worse move depends on
  1. how much worse it is
  2. the current temperature
High temperature accepts many bad moves. Low temperature accepts very few bad moves.

#### Acceptance Probability

Suppose 
current cost = 10, 
neighbor cost = 12. 

The solution got worse by 
ΔE = 12 - 10 = 2

Acceptance probability 

P=e^{-\Delta E/T}






