Linear Algebra (Link) : <br>
<br>
Main Theorem : Let $A$ be an $n \times n$ whose distinct eigenvalues are $\lambda_1,...,\lambda_p$. Then the following statements are equivalent
1. $A$ is diagonaliable
2. The sum of the dimensions of the eigenspaces equals $n$
3. The characteristic polynomial factors completely into linear factors and <br>
   The dimension of the eigenspace for each $\lambda_k$ equals the multiplicity of $\lambda_k \ \ $ ( for $k = 1, ..., p$ )

Theorem 1 : The dimenison of the eigenspace for $\lambda_k$ is less than or equal to the multiplicity of the eigenvalue $\lambda_k$
- Let $S_{\lambda_k}$ be the eigenspace corresponding to $\lambda_k$, and let $r = \dim (S_{\lambda_k})$
- Suppose $\{x_1,x_2,...,x_r\}$ is a basis for the eigenspace $S_{\lambda_k}$, then by Spanning Set Theorem, <br>
  we can let $P$ be any invertible matrix having $x_1,x_2,...,x_k$ as its first $k$ columns, say $P = \begin{bmatrix} x_1 & \cdots & x_k & x_{k+1} & \cdots & x_n \end{bmatrix}$
- In block form, we may write $P = \begin{bmatrix} B & C \end{bmatrix}$ and $P^{-1} = \begin{bmatrix} D \\ E \end{bmatrix}$ <br>
  where $B$ is $n \times k$, $C$ is $n \times (n-r)$, $D$ is $r \times n$, and $E$ is $(n-r) \times n$
- Notice that $I_n = P^{-1}P = \begin{bmatrix} BC & DC \\ EB & EC \end{bmatrix} = \begin{bmatrix} I_r & O_{k\ n-r} \\ O_{n-r\ r} & I_{n-r} \end{bmatrix}$ <br>
  and $AB = \lambda_k B \ $ because $x_1,x_2,...,x_r$ is in $S_{\lambda_k}$ so $Ax_i = \lambda_k x_i$ for $i = 1, ..., r$ 
- Then, $P^{-1}AP = \begin{bmatrix} D \\ E \end{bmatrix} A \begin{bmatrix} B & C \end{bmatrix} = \begin{bmatrix} DAB & DAC \\ EAB & EAC \end{bmatrix} = \begin{bmatrix} \lambda_k DB & DAC \\ \lambda_k EB & EAC \end{bmatrix} = \begin{bmatrix} \lambda_k I_k & DAC \\ O & EAC \end{bmatrix}$
- Therefore, by Theorem 4 in Chapter 5 in Linear Algebra, since $A$ and $P^{-1}AP$ have the same characteristic polynomial, <br>
  $\det (A - \lambda I) = \det (P^{-1}AP - \lambda I) = \begin{vmatrix} (\lambda_k - \lambda) I_r & DAC \\ O & EAC - \lambda I_{n-r} \end{vmatrix} = (\lambda_k - \lambda)^r \det (EAC - \lambda I_{n-r})$, <br>
  which means that the multiplicity of the eigenvalue $\lambda_k$ is more than or equal to $S_{\lambda_k}$

Main Theorem $1 \Rightarrow 3$ proof
- Since $A$ is diagonalizable, there are invertible matrix $P$ and diagonal matrix $D$ such that $A = PDP^{-1}$  <br>
  And by Theorem 4 in Chapter 5 in Linear Algebra, $A$ and $D$ have the same characteristic polynomial <br>
  Thus, since It's obvious that $\det (D - \lambda I) = (\lambda_1 - \lambda)^{m_1} ... (\lambda_p - \lambda)^{m_p}$ for some $m_1,...,m_p$,<br>
  The characteristic polynomial factors completely into linear factors, and $m_1 + \cdots + m_p = n \ \ \cdots \ $ (1)
- By Theorem 4 in Chapter 5 in Linear Algebra, since $A$ is diagonalizable, $A$ has $n$ linearly independent eigenvectors <br>
  So, $\dim (S_{\lambda_1}) + \cdots + \dim (S_{\lambda_p}) = n \ \ \cdots \ $ (2)
- And By Theorem 1, $m_k \geq \dim (S_{\lambda_k})$ for $k = 1,...,p$, thus $m_1 + \cdots + m_p \geq \dim (S_{\lambda_1}) + \cdots + \dim (S_{\lambda_p})$ <br>
  But by (1) and (2), both sides are equal by $n$, therefore $m_k = \dim (S_{\lambda_k})$ for $k = 1,...,p$, <br>
  which means that the dimension of the eigenspace for each $\lambda_k$ equals the multiplicity of $\lambda_k$

Main Theorem $3 \Rightarrow 2$ proof : $\dim (S_{\lambda_1}) + \cdots + \dim (S_{\lambda_p}) = m_1 + \cdots + m_p = n$ <br>
<br>
Main Theorem $2 \Rightarrow 1$ proof
- Let $\{v_{k1},...,v_{kr_k}\}$ where $r_k = \dim (S_{\lambda_k})$, be a basis of $S_{\lambda_k}$
- Since $v_{ij}$ is eigenvector and $r_1 + \cdots + r_p = n$, If $c_1v_{11} + \cdots + c_nv_{pr_p} = 0$ has only trivial solution, <br>
  then $v_{11},...,v_{pr_p}$ are linearly independent and thus $A$ has $n$ linearly independent eigenvectors <br>
  Therefore by Theorem 5 in Chapter 5 in Linear Algebra, $A$ is diagonalizable
- So, for $2 \Rightarrow 1$ proof, we should show that $c_1v_{11} \cdots + c_nv_{pr_p} = 0 \Rightarrow c_1 = \cdots = c_n = 0$
  - Let $u_k = c_{r_1 + \cdots + r_{k-1} + 1}v_{k1} + ... + c_{r_1 + \cdots + r_{k-1} + r_k}v_{kr_k}$. <br>
  - Since $v_{k1}, ..., v_{kr_k}$ is linaerly independent, $u_k = 0 \Rightarrow c_{r_1 + \cdots + r_{k-1} + 1} = \cdots = c_{r_1 + \cdots + r_{k-1} + r_k} = 0$ <br>
    Thus $c_1v_{11} + ... + c_nv_{pr_p} = 0 \Rightarrow c_1 = ... = c_n = 0$ can write $u_1 + ... + u_p = 0 \Rightarrow u_1 = \cdots = u_p = 0$
  - Let's define $p_i(x) = A_i (x - \lambda_1)...(x - \lambda_{i-1})(x - \lambda_{x+1})...(x - \lambda_p)$ where $A_i = \frac{1}{(\lambda_i - \lambda_1)...(\lambda_i - \lambda_{i-1})(\lambda_i - \lambda_{i+1})...(\lambda_i - \lambda_k)}$ <br>
    such that if $i = j$ then $p_i(\lambda_j) = 1$, otherwise $p_i(\lambda_j) = 0$.
  - Since $u_k$ is one of the eigenvectors that correspond to $\lambda_k$, $\ Au_k = \lambda_ku_k$ <br>
    Therefore for some polynomial function $f$, $\ f(A)u_k = f(\lambda_k)u_k$
  - Thus, since $p_i$ is $n$-degree polynomial function, $\ p_i(A)u_k = p_i(\lambda_k)u_k$ <br>
  - So, If $u_1 + u_2 + ... + u_p = 0$, for $i = 1, ..., p$, <br>
    $u_i = p_i(\lambda_1)u_1 + \cdots + p_i(\lambda_p)u_p = p_i(A)u_1 + \cdots + p_i(A)u_p = p_i(A) (u_1 + \cdots + u_p) = 0$ <br>
    which means that If $u_1 + u_2 + ... + u_k = 0$, then $u_1 = \cdots = u_i = 0$
  - Therefore $c_1v_{11} \cdots + c_nv_{pr_p} = 0 \Rightarrow c_1 = \cdots = c_n = 0$
