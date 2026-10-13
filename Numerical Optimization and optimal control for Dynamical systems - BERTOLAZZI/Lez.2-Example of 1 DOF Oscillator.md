---
tags:
  - BORTOLUZZI
  - Dynamics_Vibrations
data: 2026-09-25
handwriting-page-id: 73772887-a0db-4587-bd95-bcc8f6443550
---
# 1 DOF Oscillator
Oscillator with only $y(t)$ as describing parameter.
We defined $y=0$ when the spring is relaxed.
Whe need to define a procedure to analyze this kind of systems:
1. Unassemble the systems in its fondamentals components
2. apply forces to each element
3. writes every forces as functions of the describing parameter $y(t)$.
4. writes forces equilibriumas $f(y)$
5. find a **static solution** (if exist)
6. try to find solutions to the forces equilibrium.
## 1., 2. and 32.
$F_k = K \cdot y$
$F_C = C \cdot \dot{y}$
$F_{tot} = m \cdot \ddot{y}$
## 4.
$$
m \cdot \ddot y = F(t) - F_K - F_C - mg = F(t) - mg - (K\cdot y + C\cdot \dot{y})
$$
$$
K \cdot y + C \cdot \dot{y}+m\cdot \ddot{y} = F(t) -mg
$$
## 5.
Does a Static Solution with $y$ static?
Yes for $F(t) = 0$
$$
K \cdot y + \cancel{C \cdot \dot{y}} + \cancel{m \cdot \ddot{y}} = - mg
$$
equilibrium with spring compressed by math:
$$
y = -\frac{m \cdot g}{K} = y_{0}
$$
now we can define a new variable $x$ as:
$$
x=y-y_{0}
$$
that define a new starting point for the system.
$$
\begin{matrix}
 y = x +y_{0} \\
 m(\ddot{x}+\ddot{ y_{0}}) = F - K (x+y_{0}) - C (\dot{x}+\dot{y_{0}})-mg
\end{matrix}
$$
Knowing that $\ddot{y_{0}}=0$ and $\dot{y_{0}}=0$ static config.
$$
\begin{matrix}
m \ddot{x} = F -K \cdot x - K \cdot y_{0} - C\cdot \dot{x}-mg
\end{matrix}
$$
knowing that $y_0 = -\frac{m g}{K}$:
$$
\begin{matrix}
m\cdot \ddot{x} = F -K \cdot x - C \cdot \dot{x} \rightarrow m \cdot \ddot{x} + K \cdot x + C \cdot \dot{ x} = F
\end{matrix}
$$
If gravity has been consider durign the definition of the initial states $g$ doesn't appear on the final equation:
$$
m \cdot \ddot{x}+C \cdot \dot{ x}+K\cdot x = F
$$
This equation has the following properties:
- 2° order
- constant coeff.
- linear
- ordinary: depends only by $y$ and its differentials
## 6.
 To find a solution we can apply [[Laplace Transformation]]