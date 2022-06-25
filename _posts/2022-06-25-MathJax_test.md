---
title: "MathJax 로 수학식 표현하기"
tags: [Blog, MathJax, Jekyll, LaTex]
use_math: true
---

# Testing


### This formula $f(x) = x^2$ is an example

This formula $f(x) = x^2$ is an example


### Test lim

$$
\lim_{x\rightarrow\0}{\frac{e^x-1}{2x}}
=
\lim_{x\rightarrow\0}{\frac{e^x}{2}}={\frac{1}{2}}
$$


### Test sort

$$
\begin{align}
\sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\
 & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\ 
 & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\
 & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\ 
 & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right)
\end{align}
$$


### Test matrix

$$
\begin{bmatrix}
1 & x & x^2 \\
1 & \ddots & \vdots \\
1 & \cdots & z^2
\end{bmatrix}
= 100
$$
