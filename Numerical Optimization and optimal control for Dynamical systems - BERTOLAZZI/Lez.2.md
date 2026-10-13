---
tags:
  - BERTOLAZZI
data: 2026-09-25
---
# Condizioni per la minimizzazione
The function that we need to minimize have to respect at least a minimum
amount of regularity.
## First order regularity
This is the minimum regularity requirement for studying the function. 
Let $f: \mathbb{R^n}\rightarrow\mathbb{R}$ be a multivariate function satisfaying the following prop.
>  f ∈ C1(Rn), meaning f (xxx) is continuously differentiable, and the gradient (row) vector ∇ f (xxx) exists.
> - The gradient ∇ f (xxx) is Lipschitz continuous, i.e.,

[[Numerical-optimization-and-optimal-control-for-dynamical-systems-[146442].pdf#page=21&selection=26,0,77,7|Numerical-optimization-and-optimal-control-for-dynamical-systems-[146442], pagina 21]]
## ==Multidimensional minimization==
Need to implement taylor series for multidimensional functions:
$$
f(x, y) = a_0 + a_1 x + a_2 y + a_3 x^2 + a_4 y^2 + \dots
$$
This is not a good strategy, new strategy, go from: $f(x_0+\Delta x, y_0 +\Delta y) = f(x,y) + \dots$
si considera una retta descritta come:
$g(t) = f(x_0 + t\Delta x, y_0 + t\Delta y)$
che si espande in serie di taylor di una funzione in una variabile:
$g(1) = g(0) + g'(0) \cdot 1 + \frac{g''(0)}{2!} 1^2 + \dots + \frac{g^{(p)}(0)}{p!} 1^p + R_p$
