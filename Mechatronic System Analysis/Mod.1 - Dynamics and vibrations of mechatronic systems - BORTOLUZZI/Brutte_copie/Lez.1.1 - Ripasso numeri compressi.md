---
tags:
  - BORTOLUZZI
data: 2026-10-02
---
# Definitions
The complex unit is defined as: $j = \sqrt{-1}$
An Immaginary number can be written in different way:
- algebrically: $s = a+ j \cdot b$
	- where we can identify two components: $a = \mathrm{Re}(s)\ b = \mathrm{Im}(s)$
- angular: $\alpha = \mathrm{Ang}(s)= \arctan(\frac{b}{a})$
- exponential: $s = m \cdot e^{j \alpha}$
	- therefore $e^{j \alpha}=\cos(\alpha)+j\cdot \sin(\alpha)$
	- $e^s = e^{\alpha + j \cdot b} = e ^ \alpha \cdot e ^{j \cdot b} = e^\alpha (\cos(b) + j \cdot \sin b$
	- and
	  $$
	\begin{matrix}
	\cos(\alpha) = \frac{e^{j \alpha}+e^{-j \alpha}}{2} \\
	\sin(\alpha) = \frac{e^{j \alpha}-e^{-j \alpha}}{2}
	\end{matrix}
	$$
In both form the modulis is:
$$
|s| = \sqrt{ a^2+b^2 }
$$
## Properties
$$
\begin{matrix}
s_1+s_2 = (a_1+j b_1)(a_2+j b_2)\\
= a_1 a_2 - b_1 b_2 + j (a_1 b_2 + a_2 b_1)\\
| s_1 s_2 | = |s_1| | s_2| = m_1 m_2\\
\mathrm{Ang}(s_1 s_2)= \mathrm{Ang} (s_1) + \mathrm{Ang}(s_2)= \alpha_1+\alpha_2\\
\end{matrix}
$$
![[Euler_complex_number.webp|359]]
## Coniugate
$$
s* = a - j \cdot b
$$
### Properties
$$
\begin{matrix}
s \cdot s^* = a^2+b^2 = |s|^2 \\
s +s^* = a+b \cdot j + a -b \cdot j  = 2a \\
s -s^* = a + j \cdot b - a + j \cdot b = 2j \cdot b
\end{matrix}
$$
# From time domain to Laplace Transformation
In a complex number expressed in the angular form we can assume the angle $\alpha$ as a function of the time $\alpha = \omega t$ where $\omega$ is the speed of rotation of the complex number in angular notation.
Therefore $e^\alpha = e^{\omega t}$.
If we consider $\omega$ a complex number $s$ we can rewrite the exponential as:
$$
\begin{matrix}
e^{st}=e^{(a+j \cdot b)t}= e^{a \cdot t}e^{jb\cdot t}= \\
e^{a \cdot t}(\cos(b t)+j \sin(bt))
\end{matrix}
$$
We can write the following sum:
$$
\begin{matrix}
s_{1}e^{s_{2}t}+s_{1}^* \\
m_{1} e^{j\alpha_{1}}\cdot e^{(a_{2}+j b_{2})t}+m_{1} e^{-j \alpha_{1}}e^{(a_{2}-jb_{2})t}= \\

\end{matrix}
$$