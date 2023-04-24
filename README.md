# Lotka-Voltera
Solution to Lota-Voltera prey-predator model using Fortran

Lotka Voltera prey-predator model, for an autocatalytic reaction. 

For the reaction

$A \rightarrow 2X $

$X + Y \rightarrow 2Y$

$Y \rightarrow B$

```
\begin{equation}\label
{{dX} \over {dt}} = k_1[A][X] - k_2 [X][Y]
\end{equation}
```
