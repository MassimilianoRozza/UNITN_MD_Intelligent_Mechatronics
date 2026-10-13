---
tags:
  - BERTOLAZZI
data: 2026-10-09
brutta: "[[Bertolazzi 2026-10-09 08.41.10.excalidraw]]"
---
# Introduction
==ODE== = Ordinary Differential Equations
$$
\begin{cases}
y'=f(x,y) & \text{ODE equal to } y'(x) = f(x,y(x))  \\
t(0)=y_{0}& \text{initial condition}
\end{cases}
$$
# Example
Using the heat transfer law:
$$
\begin{cases}
T'=-k(T-T_{env})\leftarrow \text{fourier law} \\
T(0) = T_{0}
\end{cases}
$$
The law is a first order ordinary differential equation has as solution a growing or lowering exponential law toward $T_{env}$ 
![[Fourier Law.webp]]
Another example can be:
$$
\begin{cases}
x'=x+y \\
y'=e^x-y \\
x(0) = 1 \\
y(0)=1
\end{cases}
$$
If we are in front of a set of ODE is it better to use the vectorial notation:
$$
\begin{matrix}
\begin{matrix}
z=
\begin{pmatrix}
x \\
y
\end{pmatrix} &
F(t,z)=\begin{pmatrix}
x+y \\
e^x-y
\end{pmatrix} \\
\end{matrix}\\
\begin{cases}
z'=F(t,z) \rightarrow \text{ODE} \\
E(0)=\begin{pmatrix}
1 \\
1
\end{pmatrix}\rightarrow \text{initial condition}
\end{cases}
\end{matrix}
$$
We will use scalar case for simplicity, but for multidimensional notatio we can always use vectorial notation.
# Special case
- **Linear ODE**: often used when linearizing.
	$$
	y'(t)=A(t)y+b\rightarrow F(t,y(t))
	$$
	Where $A(t)$ is a matrix and $b$ is a vector.
	In this case a formal exact solution can be written using Matrix exponential:
	$$
	y(t)=e^{t-t_{0}A}y_{0}
	$$
	Where $e^x=1+x+{x^2\over 2}+\dots+{x^p\over p!}$ using taylor series expanction, if the exponent is a square matrix:$e^B=I+B+{B^2\over 2}+\dots+{B^p\over p!}$ with $B^2 = B*B$, $B^3=B*B*B$ and so on.
	To compiute $e^B$ special method exist.
- **Implicit Written ODE**: Is it possible to find implicitly written ODE:
	$$
	\begin{cases}
	y'=f(x,y) \\
	y(0)=y_{0}
	\end{cases}\Longrightarrow
	\begin{cases}
	g(x,y,y')=0 \\
	y(0)=y_{0}
	\end{cases}
	$$
	is an ODE if $\partial g(x,y,y')\over \partial y$ so that using implicit theorem $\implies y'=f(x,y)$ by solving $g(x,y,y')=0$
- **High order ODE**
  $$
  \begin{cases}
  y=f(x,y,y',y'', \dots) \\
  y(0)= \\
  y'(0)= \\ \\
  \dots \\
  y^{(n)}(0)= 
  \end{cases}
  $$
- **Special case ODE: Autonomous:**
  $$
  \begin{cases}
  y'(x)=f(x,y(x)) \\
  y'(0)=0
  \end{cases}
  \longrightarrow
  \begin{cases}
  y'(x)=f(y(x)) \\
  y'(0)=0
  \end{cases}
  $$
  An ODE is autonomous if it doesn't depend from the variable. Most of the physic equations fall on this classification because law doesn't changes with time.
# REMARK
1. ==watch the notes manca==
2. Only ODE can be transformed to autonomous increasing by 1 the number of equations:
   $$
   \begin{cases}
x'=x+y+t \\
y'=xyt-t^2 \\
x(0)=1 \\
y(0)=2
   \end{cases}
   $$
   In this example $x$ and $y$ are $f(t)$ while $t$ is the indipendent variable.
   We can introduce $\tau$ such that $\tau'=1$ and $\tau(0)=0\implies \tau(t)=t$:
   $$
   \begin{cases}
   x'=x+y+\tau \\
y'=xy\tau-\tau^2 \\
\tau'=1 \\
x(0)=1 \\
y(0)=2 \\
\tau(0)=0
   \end{cases}
   $$
   
   ---
# From theory
We need:
- well posedness $\rightarrow$ continuous dependance from parameters $=$ changing initial condition the solutions changes only a little in the nearness and than starts to explode
- existance and uniqueness
## Peano Theorem
This theorem grant the existance of solutions:
$$
\begin{cases}
y'=f(t,y) \leftarrow \begin{matrix}
\text{continuous, especially in an ``intorno'' of } (0,y_0)\\
\downarrow\\
\text{than at least a solution exist (at least "in un intorno")}
\end{matrix} \\[6pt]
y(0)=y_{0}
\end{cases}
$$
We can find at least one non Liepschlitz continuous example ($\sqrt{|y|}$ that does not have solution near $0$.
### Example
![[example_peano_theorem.webp]]
## Picard-Lindelof Theorem
To grant the uniqueness of the solution.
$$
\begin{matrix}
f(t,y)=\dots & \text{continuous} \\
f(t,y)=\dots & \text{liepschlitz continuous for y} \\
\end{matrix}
$$
The ODE ha **unique** solutions.
# Euler Method
$$
\begin{cases}
y'=f(t,y) \\
y(0)=y_{0}
\end{cases}
$$
Calling $y(t)$ exact solution of the system.
Is it possible to write the exact solution as a taylor series:
$$
y(t+\Delta t)=y(t)+y'(t+\Delta t)+\dots+o(\Delta t^2)
$$
For $\Delta t$ small  -> $y(t+\Delta t) = y(t) + y'(t) \Delta t = y(t)+f(t,y(t))\Delta t$
![[time_graph.webp]]
$\Delta t = {b-a \over n} \quad t_{k}=a+k \Delta t$
Whit this new formulation the serie became:
$$
y(t_{k+1})=y(t_{k}+\Delta t) = y(t_{k})+\Delta t  f(t_{k}, y(t_{k}))

$$
Where $y_{k}$ is the approximation of $y(t_k)$.
As the solution of the ODE exist (for our assumption) we can graph the solution $y(t)$ and the approximation $y(t_{k+1})$ both starting from the known initial condition $y_{0}=y_{a}$
[insert graph]
The question is: *Does $|y(t_k)-y_{k}|\to 0$ if $\Delta t \to 0$?*
## Error estimation
At the moment we have tho form of the equation:
- **EXACT**: 
  $$
  \begin{matrix}
 y(t_{k+1})=y(t_{k})+y'(t_{k})\Delta t + 0.5 y''(\xi_{k})\Delta t^2 = \\
 =y(t_{k})+\Delta t f(t_{k}, y(t_{k}))+0.5*\Delta t^2 y''(\xi_{k}) & with \xi \in(t_{k}, t_{k+1}) 
  \end{matrix}
  $$
- **NUMERIC**: $y_{k+1}=y_{k}+\Delta t*f(t_{k}, y_{k})$
The difference from the two is the error:
**EXACT - NUMERIC**=
$$
\begin{matrix}
y(t_{k+1})-y_{k+1}=y(t_{k})-y_{k}+\Delta t \left[ f\left(t_{k}, y(t_{k})\right)-f(t_{k}; y_{k})\right]+0.5* \Delta t^2 y''(\xi_{k}) \\
|\epsilon_{k+1}|\le|\epsilon_{k}|+\Delta t \left| f(t_{k},y(t_{k}))-f(t_{k,y_{k}})\right| + 0.5* \Delta t^2 |y''(\xi_{k})| \\
\text{Using Liepschlitz} \\
|f(t_{k},y_{t_{k}})|-f(t_{k},y_{k})|\le L|\dots| \\
|y''(\xi_{k})|\le\max|y''(t)|=M_{2-\dots} & \text{for } t \in [a,b] 
\end{matrix}
$$
Rearranging:
$$
\begin{matrix}
A=(1+\Delta t L)\quad B=0.5*\Delta t \\
|\epsilon_{k+1}|\le A |\epsilon_{k}|+B
\end{matrix}
$$
Can this new formulation be used to estimate:
$$
\begin{matrix}
E_{\Delta t}=\begin{matrix}N\\ \max \\ k=0\end{matrix}|\xi_{k}| \\
\epsilon_{0} = y(t_{0})-y_{0}=0 \\
|\epsilon_{1}| = A |\epsilon_{0}|+B=B \\
|\epsilon_{2}| = A |\epsilon_{1}|+B\le A B+B = (1+A)B \\ \\
|\epsilon_{3}| = \dots \le A (1+A)B+B = (1+A+A^2)B \\ \\
\text{and so on } 
\end{matrix}
$$
Therefore, in general:
$$
|\epsilon_{k}|\le(1+A+A^2+\dots+A^k)B \\
$$
the content of the parentesis is a geometric series $\frac{1-q^{k+1}}{1-q}$
$$
|\epsilon_{k}|=\frac{A^k-1}{A-1}B
$$
Da cui:
$$
E_{\Delta t}=
\begin{matrix}
N \\
\max \\
K=0
\end{matrix}
|\epsilon_{k}|\le \frac{A^N-1}{A-1}B
$$
Thus using $A=1+\Delta t L \quad B=0.5*\Delta t^2 M_{2}$
$$
E_{\Delta t}\le \frac{(1+\Delta t L)^N}{2L}M_{2}\Delta t
$$
After some manipulation:
$$
E_{\Delta t} \le \left(\frac{e^{(b-a)L}}{2L}M_{2}\right)\Delta t
$$
## REMARK
A general 1 step method for:
$$
\begin{cases}
y'=f(t,y) \\
y(0)=y_{0}
\end{cases}
$$
the solution has the general form: $y_{k+1}=\Psi(y_{k}, t_{k}, h)=y_{k}+h \Phi(t_{k}, y_{k}, h)$
if $\Phi$ is liepschlitz continuous:
$$
||\Phi(t_{k},y_{k}, h)-\Phi(t_{k},z,h)||\le L ||y-z||
$$
Then the convergence can be proved as in the [[#Euler Method]].