# Calculus Review
## First order Regularity
Let $f : R^n → R$ be a multivariate function satisfying the following properties:
- $f ∈ C^1(R^n)$, meaning $f (x)$ is continuously differentiable, and the gradient (row) vector $\nabla f(x)$ exists.
- The gradient $\nabla f(x)$ is Lipschitz continuous, i.e.,
  $$
  ∥\nabla f(x) − \nabla f(y)∥ ≤ γ∥x − y∥, \quad \forall x, y ∈ R
  $$
### Taylor series of first order for $f$:
$$
f(x+t d)= f(x)+t \nabla f(x)d+o(t)
$$
## Second order Regularity
Consider again a multivariate function $f: \mathbb{R}^n\to \mathbb{R}$ but now assume the following:
- $f\in C^2(\mathbb{R}^n)$ meaning $f(x)$ is twice continuously differentiable. Both the gradient vector $\nabla f(x)$ and the Hessian matrix $\nabla^2 f(x)$ exists.
- The Hessian $\nabla^2 f(x)$ is Lipschitz Continuous:
  $$
  ||\nabla^2 f(x)-\nabla^2f(y)|| \le \gamma ||x-y||, \quad \forall x,y \in \mathbb{R}^n
  $$
### Taylor series of second order for $f$:
$$
f(x+t d)= f(x)+t \nabla f(x)d+\frac{t^2}{2}d^T\nabla^2f(x)d+ o(t^2)
$$
## Local Minimum
### First Order Conditions
- [[#First order Regularity]]
- Necessary condition for local minimum: $\nabla f(x^*)^T = 0$
	- Not sufficient to confirm $x^*$ as a minimum
### Second Order Conditions
- [[#Second order Regularity]]
- Necessary Condition: the Hessian matrix in $x^*$ must be **semi-positive definite** (SDP)
  $$
  d^T \nabla^2 f(x^*)d\ge 0 \quad \forall d \in \mathbb{R}^n
  $$
### Sufficient conditions
For $x^*$ to be a local minimum, a sufficient condition is that the Hessian matrix $\nabla^2 f(x^*)$ is positive definite.
$$
d^T\nabla^2 f(x^*)d > 0 \quad \forall d\ne 0\in \mathbb{R}^n
$$
The condition is equivalent to say that all the Hessian Matrix $\nabla^2 f(x^*)$ are positive.
In summary:
- A point where the gradient vanishes may be a local minimum, maximum, or saddle point.
- To classify a point as a (strict) local minimum, the Hessian must be positive definite.
- If the Hessian matrix is not semi-positive definite the point is surely not a minimum point.

There is a grey zone when the point satisfy necessary condition:
- the Hessian matrix is semi-positive definite
but not the sufficient condition, i.e:
- the Hessian matrix is not positive definite
in this case the point can be a minimal point or not a require further investigation
# Lagrange Multiplier
See [[Numerical Optimization and optimal control for Dynamical systems - BERTOLAZZI/Lagrange Multiplier|Lagrange Multiplier]].
To
Minimize: $f(x)$
Subject to: $h_k(x) = 0,\quad k = 1,\dots , m$
To simplify Lagrange Multiplier theorem can be used.
## Lagrangian Function
$$
\mathcal{L}(x,\lambda)=f(x) - \sum_{k=1}^m \lambda_{k}h_{k}(x)
$$
where $\lambda=(\lambda_{1}, \dots, \lambda_{m})$ is the vector of Lagrange multipliers.
## Solving the Constrained Minimization Problem
To solve and find $x^*$ we need to solve:
$$
\begin{cases}
\nabla_{x}\mathcal{L}(x,y)=\nabla f(x) - \sum_{k=1}^m \lambda_{k}\nabla h_{k}(x)=0 \\
\nabla_{\lambda}\mathcal{L}(x,y) =-h(x) = 0 
\end{cases}
$$
The $x^*$ are only candidate, further condition are necessary to define if they are minimum, maximum or saddle.
## Second order conditions
$$
z^T \nabla^2 _{x}\mathcal{L}(x^*, \lambda)z>0 \quad \forall kernel[{\nabla h(x^*)}]
$$
Here:
- $z$ is any vector in the null space (or kernel) of the contraint Jacobian, $kernel[{\nabla h(x^*)}]$
- condition means that, in the directions allowed by the constraints, the Hessian of the Lagrangian should be positive.
As seen in [[#Second Order Conditions]] if the condition is $\ge$ the condition is necessary but not sufficient.
# Sylvester theorem
Sylvester's theorem provides an operational criterion to determine whether a matrix is symmetric positive definite (SPD).

>[!Definition]
> A symmetric matrix $A \in \mathbb{R}^{n \times n}$ is PD (positive definite) if and only if  all its leading principal minors are positive.
> $$
> \det(A_{k})>0 \quad \text{for} k=1,2,\dots, n
> $$

### Why Sylvester theorem
Given $A = \nabla^2_{x}\mathcal{L}(x^*, \lambda)$, $B=\nabla h$ and $z \in ker[\nabla h(x^*)]$.
The vector $k$ can be expressed as linear combination of the vectors $k_i$ (base of $B$) as:
$$
z = k_{1}\alpha_{1}+k_{2}\alpha_{2}+\dots k_{p}\alpha_{p}\quad \alpha \in \mathbb{R}^p 
$$
that can be reduced to a multiplication of matrix:
- $K \in \mathbb{R}^{n \times p}$ whose columns are the vectors $k_i$
- vector $\alpha$ 
$$
z^T A z = \alpha^T K^T A K \alpha = \alpha^T C \alpha,  \quad C:=K^T A K \in \mathbb{R}^{p\times p}
$$
### Trick for semi-positive matrices
 A way to determine if the matrix is semi-positive defined is by considering that the matrix $A + \epsilon I$ should be positive defined for values of $\epsilon$ approaching _zero_ from the positive direction (for $\epsilon \to  0^+$).
>[!Lemma]
> A symetric matrix $A \in \mathbb{R}^{n \times n}$ is semi-positive definite if and only if all its leading principal minors $A_k$ satisfy the following conditions:
> - $det(A_k)>0$
> - $\det(A_{k})=0$ and the derivative of the perturbated determinant at $\epsilon=0$ is positive:
>   $$
> \frac{d}{d \epsilon} \det(A_{k}+\epsilon I )\Big\vert_{\epsilon=0}>0
> $$

# Inequality constraints
If the constraints are inequalities the minimization problem is:
minimize: $f(x)$
subject to: $g_k(x)\ge 0 \quad k = , \dots , p$
We can transform the inequalities to equality adding the **slack variables** ($\epsilon$):
$$
g_{k}(x)=\epsilon ^2_{k}\ge 0 \Rightarrow h_{k}(x, \epsilon_{k})=g_{k}(x)-\epsilon_{k}^2=0
$$
## Karush-Kuhn-Tucker conditions
This condition are a set of necessary and sufficient conditions that allows to describe if a point $x^*$ is a minimum point of a minimization problem subjected to equality and inequality constraints:

minimize: $f(x)$
subject to:        $h_{k}(x)=0\quad k = 1, \dots, m$
			$g_{k}(x)\ge 0\quad k = 1,\dots, p$
It' important to define the set of active constraints:
$$
\mathcal{A}(x^*)=\left\{ k | g_{k}(x^*)=0\right\}
$$
A constrain is active for $x^*$ if $g(x^*)=0$.
### Qualified constraints
Given the inequality and equality constraints map ($g$ and $h$), than the point $x^*$ is defined **qualified** if the gradients of the active constraints (and the equality ones) are linearly independent, and so:
$$
\left\{\nabla g_{k}(x^*): k \in \mathcal{A}(x^*)\right\} \cup \left\{\nabla h_{1}(x^*), \dots, \nabla h_{m}(x^*)\right\}
$$
are linearly independent.
### KKT first order necessaty condition
Given the above minimization problem, the necessary condition for $x^*$ to be a local minima is the existance of $m+p$ scalar $\lambda_{i}, \mu_{j}$ such to satisfy:
$$
\begin{matrix}
\lambda_{x}\mathcal{L}(x^*, \lambda^*, \mu^*)=0 \\
h_{k}(x^*)=0 \\
g_{k}(x^*)\ge 0 \\
\mu^*_{k}g_{k}(x^*)=0 \\
\mu^*_{k}\ge 0
\end{matrix}
$$
Where the Lagrangian is in this case defined as:
$$
\mathcal{L}(x,\lambda, \mu)=f(x)-\sum_{k=1}^p\mu_{k}g_{k}(x)-\sum_{k=1}^m \lambda_{k}h_{k}(x)
$$
### KKT second order necessary conditions
$$
z^T \nabla^2_{x}\mathcal{L}(x^*, \lambda^*, m^*)z \ge 0
$$
### KKT second order sufficient conditions
$$
z^T \nabla^2_{x}\mathcal{L}(x^*, \lambda^*, m^*)z > 0
$$
# Summary of KKT with proofs