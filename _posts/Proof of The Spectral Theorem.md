Linear Algebra : [link] <br>
<br>
The Spectral Theorem : An $n \times n$ symmetric matrix $A$ has the following properties:
1. $A$ has $n$ real eigenvalues, counting multiplicities
2. The dimension of the eigenspace for each eigenvalue $\lambda$ equals <br>
   the multiplicity of $\lambda$ as a root of the characteristic equation
3. The eigenspaces are mutually orthogonal, in the sense that <br>
   eigenvectors corresponding to different eigenvalues are orthogonal
4. $A$ is orthogonally diagnoalizable

$1$ proof
- By the fundamental theorem of algebra, the characteristic equation of $A$ has $n$ roots. Thus $A$ has $n$ complex eigenvalues.
- Suppose that $Ax = \lambda x$, where $x$ is in $\mathbb{C}^n$ and $x \neq 0$
- Let $q = \bar{x}^TAx$, then $\bar{q} = \bar{\bar{x}^TAx} = x^T\bar{Ax} = x^TA\bar{x} = (x^TA\bar{x})^T = x^TA^Tx = x^TAx = q$ <br>
  and since $q = \bar{x}^TAx = \bar{x}^T \lambda x = \lambda \bar{x}^T x$, we also have $\bar{q} = \bar{\lambda \bar{x}^T x} = \bar{\lambda} x^T \bar{x} = \bar{\lambda} (x^T \bar{x})^T = \bar{\lambda} \bar{x}^T x$
- Therefore $0 = q - \bar{q} = (\lambda - \bar{\lambda}) \bar{x}^T x$ <br>
  and since $\bar{x}^T x = x^* x = \|x\|^2 \neq 0$, we must have $\lambda - \bar{\lambda} = 0$, therefore $\lambda$ is real
  - Plus, let $x = u + iv$, where $u, v$ is in $\mathbb{R}^n$, then $Ax = \lambda x \Rightarrow Au + iAv = \lambda u + i \lambda v$ <br>
    $\Rightarrow Au = \lambda u \land Av = \lambda v$, therefore $u$, the real part of $x$, is an eigenvector of $A$ corresponding to $\lambda$

$3$ proof
- Let $v_1$ and $v_2$ be eigenvectors that correspond to distinct eigenvalues, say, $\lambda_1$ and $\lambda_2$
- $\lambda_1 v_1 \cdot v_2 = (\lambda_1 v_1)^T v_2 = (Av_1)^T v_2 = (v_1^T A^T) v_2$, and since $A^T = A$, <br>
  $= v_1^T (Av_2) = v_1^T ( \lambda_2v_2 ) = \lambda_2 v_1^T v_2 = \lambda_2 v_1 \cdot v_2$
- Hence $(\lambda_1 - \lambda_2) v_1 \cdot v_2 = 0$, but $\lambda_1 - \lambda_2 \neq 0$, so $v_1 \cdot v_2 = 0$

Schur factorization : $A = URU^T$ where $U$ is an orthogonal matrix and $R$ is an $n \times n$ upper triangular matrix
- in complex space, we can prove **any** $n \times n$ matrix $A$ admits a Schur factorization <br>
  using that we can find Householder matrix $R$ such that $Rx = ce_1$, which is hermitian and unitary 
- in real space, if $n \times n$ matrix $A$ has $n$ real eigenvalues, counting multiplicities, denoted by $\lambda_1, ..., \lambda_n$, <br>
  then $A$ admits a (real) Schur factorization
  - Let $u_1$ be a unit eigenvector corresponding to $\lambda_1$, <br>
    and we can construct $u_2,...,u_n$ such that $\{u_1,...,u_n\}$ is an orthonormal basis for $\mathbb{R}^n$
  - Let $V_1 = \begin{bmatrix} u_2 & \cdots & u_n \end{bmatrix}$, and let $P_1 = \begin{bmatrix} u_1 & V_1 \end{bmatrix}$ <br>
    then $P_1^TAP_1 = \begin{bmatrix} u_1^TAu_1 & u_1^TAV_1 \\ V_1^TAu_1 & V_1^TAV_1 \end{bmatrix} = \begin{bmatrix} \lambda_1 & * \\ 0 & V_1^TAV_1 \end{bmatrix}$ <br>
    because $Au_1 = \lambda_1u_1$ and $u_i^Tu_i = 1$, $u_i^Tu_j = 0$ for $i \neq j$ 
  - By Theorem 4 in Chapter 5 in Linear Algebra, $A$ and $P_1^TAP_1$ have the same eigenvalues <br>
    And $\det (P_1^TAP_1 - \lambda I) = \begin{vmatrix} \lambda_1 - \lambda & * \\ 0 & V_1^TAV_1 - \lambda I_{n-1} \end{vmatrix} = (\lambda_1 - \lambda) \det (V_1^TAV_1 - \lambda I_{n-1})$ <br>
    Therefore $A_1 = V_1^TAV_1$ has $n-1$ eigenvalues $\lambda_2, ..., \lambda_n$ <br>
    And let $U_1 = P_1$, and let $R_1 = U_1^TAU_1$, then $A = U_1R_1U_1^T$ because $U_1$ is orthogonal matrix
  - **New**, let $u_2$ be a unit eigenvector corresponding to $\lambda_2$, and process $A_1$ with the same way <br>
    And let $U_2 = \begin{bmatrix} 1 & 0 \\ 0 & P_2 \end{bmatrix}$, then $U_2^T(U_1^TAU_1)U_2 = \begin{bmatrix} \lambda_1 & * & * \\ 0 & \lambda_2 & * \\ 0 & 0 & V_2^TAV_2 \end{bmatrix} = R_2$
  - repeating this process with $U_3 = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & P_3 \end{bmatrix}, \cdots$, which are orthogonal matrix, <br>
    we will reach $U_n^T ... U_1^TAU_1 ... U_n = \begin{bmatrix} \lambda_1 & * & \cdots & * \\ 0 & \lambda_2 & \cdots & * \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & \lambda_n \end{bmatrix} = R_n$ <br>
    so $A = U_1...U_nR_nU_n^T...U_1^T = URU^T$ where $R = R_n$ and $U = U_1...U_n$, which is orthogonal matrix

$4$ proof (way 1)
- By $1$, the $n \times n$ symmetric matrix $A$ has $n$ real eigenvalues, so admits a Schur factorization <br>
  Thus we can find $A = URU^T$ where $U$ is an orthogonal matrix and $R$ is an $n \times n$ upper triangular matrix
- And since $A^T = (URU^T)^T = (U^T)^TR^TU^T = UR^TU^T = A$, we have $R = R^T$, which means that $R$ is diagonal matrix <br>
  Therefore $A$ is orthogonally diagonalizable

$2$ proof (way 1)
- By Theorem 7 in Chapter 5 in Linear Algebra, if $A$ is diagonalizable, <br>
  then the dimension of the eigenspace for each $\lambda_k$ equals the multiplicity of $\lambda_k$
- Therefore, by $4$, since $A$ is orthogonally diagonalizable, $2$ is true

Generalized eigenvector : [link]

$4$ proof (way 2)
- By the property of Generalized eigenvector, if the symmetric $A$ is not diagonalizable, <br>
  then we have $(A - \lambda I)^2v = 0$ and $(A - \lambda I)v \neq 0$, where $\lambda$ is some repeated eigenvalue
- but $0 = v^T (A - \lambda I)^2 v = v^T (A - \lambda I)^T (A - \lambda I) v = ( (A - \lambda I) v )^T (A - \lambda I) \neq 0$, which is a contradiction <br>
  Therefore $A$ is diagonalizable, and by $3$, all eigenvectors are orthogonal, which means that $A$ is orthogonally diagonalizable
  - because $A$ is symmetric, $(A - \lambda I)^T = (A^T - \lambda I) = (A - \lambda I)$

2 proof (way 2)
- for any matrix $M$, not only $\mathrm{Nul}\ M \subseteq \mathrm{Nul}\ M^TM$, but also $\mathrm{Nul}\ M \supseteq \mathrm{Nul}\ M^TM$, so that $\mathrm{Nul}\ M = \mathrm{Nul}\ M^TM$
  - $x \in \mathrm{Nul}\ M^TM \Rightarrow M^TMx = 0 \Rightarrow x^TM^TMx = 0 \Rightarrow (Mx)^T(Mx) = 0 \Rightarrow Mx = 0 \Rightarrow x \in \mathrm{Nul}\ M$
- Let $M = A - \lambda I$, then $\dim \mathrm{Nul}\ (A - \lambda I)^2 = \dim \mathrm{Nul}\ (a - \lambda I)$, <br>
  which means that the algebraic and geometric multiplicities are same by the property of Generalized eigenvector
