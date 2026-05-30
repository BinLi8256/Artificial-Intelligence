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






