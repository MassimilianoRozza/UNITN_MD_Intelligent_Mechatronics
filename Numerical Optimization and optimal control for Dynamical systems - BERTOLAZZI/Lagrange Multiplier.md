---
data: 2026-09-29
tags:
  - BERTOLAZZI
---
# EXAMPLE
## Problem
minimize $f(x,y) = x+y$  
subject to $h(x,y) = x^2+y^2-1$
## Solution
Write the two variables as other more simple to handle in the required domain
$$
\begin{aligned}
x &= \cos \alpha \\
y &= \sin \alpha
\end{aligned}
$$
Then rewrite the constrain substituting the variable as just defined:
$h(x,y)=x^2+y^2-1 = \cos^2\alpha + \sin^2\alpha - 1 = 0$  
$h(x,y) = 0$
**this eliminates the constrain**
$f(\theta) = f(\cos \alpha; \sin \alpha) = \cos \alpha + \sin(\alpha) \quad \text{function restricted}$

### Stationary points
To define the stationary point we calculate the first derivative: 
$f'(\theta) = -\sin \theta + \cos(\theta) = 0 \implies \cos(\theta) \neq 0 \quad \text{assuming}$
The assumption have been introduced to ensure that $\sin(\theta) \ne \pm 1$ because if
$\cos(\theta) = 0 \quad \theta = \pm\frac{\pi}{2} \implies \sin(\theta) = \pm 1$  
thus $\cos(\theta) \neq 0$ we can:
![[tangent.webp|367]]
$$
\begin{aligned}
-\frac{\sin \theta}{\cos \theta} + 1 &= 0 \\
-\tan(\theta) + 1 &= 0 \\
\theta &= \arctan(1)
\end{aligned}
$$
$\tan(\theta) = 1 \quad \text{if} \quad \theta = \frac{\pi}{4} \pm \pi k$

Stationary point are $\theta = \frac{\pi}{4}$ and $\theta = \frac{5}{4}\pi$
$$
\begin{aligned}
\theta_1 &= \frac{\pi}{4} \\
\theta_2 &= \frac{5}{4}\pi
\end{aligned}
$$
To understand if the stationary points just founded are minimum or maximum.
$$f''(\theta) = \frac{d}{d\theta}\left(-\sin(\theta) + \cos(\theta)\right) = -\cos \theta - \sin(\theta)$$

> **Diagram Note:** A rough sketch of a unit circle with a vertical line intersecting the right side. It illustrates an angle $\alpha$ and another angle $\beta$, accompanied by the formula $\tan(\alpha) = \tan(\beta)$.

$$
\begin{matrix}
f''\left(\frac{\pi}{4}\right) = -\sqrt{2} \rightarrow 2^{\text{nd}} \text{ derivative} < 0 \quad \text{max}\\
f''\left(\frac{5}{4}\pi\right) = +\sqrt{2} \rightarrow 2^{\text{nd}} \text{ derivative} > 0 \quad \text{minimum}
\end{matrix}
$$
BACK to $x,y$
$$
\begin{matrix}
\theta_1 = \frac{\pi}{4} \rightarrow (x_{1},y_{1}) = \left(\frac{\sqrt{2}}{2}; \frac{\sqrt{2}}{2}\right) \quad \max\\
\theta_2 = \frac{5}{4}\pi \rightarrow (x_2,y_2) = \left(-\frac{\sqrt{2}}{2}; -\frac{\sqrt{2}}{2}\right) \quad \min
\end{matrix}
$$
$x+y=f(x,y)$ is a plain we needed constrain to lock the domain to define a min and max.
![[defining_costraint.webp|254]]
If we solve the problem using another mapping
$$
\begin{cases}
x = \frac{t}{\sqrt{1+t^2}} \\
y = \frac{1}{\sqrt{1+t^2}}
\end{cases}
$$
![[Lagrange Multiplier_map 1.webp|181]]
$$
\begin{matrix}
x^2 + y^2 = 1\\
f(t) = \frac{t}{\sqrt{1+t^2}} + \frac{1}{\sqrt{1+t^2}} = \frac{1+t}{\sqrt{1+t^2}}
\end{matrix}
$$stationary points:
$$
f'(t) = \frac{1-t}{(1+t^2)^{3/2}} \rightarrow f'(t) = 0 \Leftrightarrow t = 1
$$
$$
f''(t) = \frac{2t^2 - 3t - 1}{(1+t^2)^{5/2}} \Rightarrow f''(1) = \frac{2 - 3 - 1}{2^{5/2}} = -\frac{2}{2^{5/2}} < 0 \rightarrow \max
$$
- another mapping
  ![[new map.webp]]
$$
\begin{aligned}
x &= \frac{t}{\sqrt{1+t^2}} \\
y &= -\frac{1}{\sqrt{1+t^2}}
\end{aligned}
$$
