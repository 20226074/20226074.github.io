Linear Algebra : (link) <br>
<br>
Main Theorem : Let $M$ be a partitioned matrix of the form $M = \begin{bmatrix} A & B \\ C & D \end{bmatrix}$ where $A$ and $D$ are square matrices
- If $A$ is invertible, then $\det (M) = \det (A) \det (D - C A^{-1} B)$ <br>
- If $D$ is invertible, then $\det (M) = \det (A - B D^{-1} C) \det (D) \ \ $ <br>

Theorem 1 : If $M = \begin{bmatrix} A & 0 \\ 0 & I_p \end{bmatrix}$ or $M = \begin{bmatrix} I_p & 0 \\ 0 & A \end{bmatrix}$, then $\det (M) = \det (A)$
- If $M = \begin{bmatrix} A & 0 \\ 0 & I_p \end{bmatrix}$, then cofactor expansion $p$ columns from the **right** shows that $\det (M) = \det (A)$
- If $M = \begin{bmatrix} I_p & 0 \\ 0 & A \end{bmatrix}$, then cofactor expansion $p$ columns from the **left**, shows that $\det (M) = \det (A)$

Theorem 2 : if $M = \begin{bmatrix} A & B \\ 0 & D \end{bmatrix}$ or $M = \begin{bmatrix} A & 0 \\ C & D \end{bmatrix}$ where $A$ is $p \times p$ and $D$ is $q \times q$, then $\det (M) = \det (A) \det (D)$
- If $M = \begin{bmatrix} A & B \\ 0 & D \end{bmatrix}$, we can check $\begin{bmatrix} I_p & 0 \\ 0 & D \end{bmatrix} \begin{bmatrix} I_p & B \\ 0 & I_q \end{bmatrix} \begin{bmatrix} A & 0 \\ 0 & I_q \end{bmatrix} = \begin{bmatrix} A & B \\ 0 & D \end{bmatrix}$
  - By Theorem 1, $\begin{vmatrix} I_p & 0 \\ 0 & D \end{vmatrix} = \det (D)$, and $\begin{vmatrix} A & 0 \\ 0 & I_q \end{vmatrix} = \det (A)$
  - And $\begin{vmatrix} I_p & B \\ 0 & I_q \end{vmatrix} = \begin{vmatrix} I_q \end{vmatrix} = 1$, which can show by cofactor expansion down a column from the left
  - Therefore, $\det (M) = \det (D) \cdot 1 \cdot \det (A) = \det (A) \det (D)$
- If $M = \begin{bmatrix} A & 0 \\ C & D \end{bmatrix}$, we can check $\begin{bmatrix} A & 0 \\ 0 & I_q \end{bmatrix} \begin{bmatrix} I_p & 0 \\ C & I_q \end{bmatrix} \begin{bmatrix} I_p & 0 \\ 0 & D \end{bmatrix} = \begin{bmatrix} A & 0 \\ C & D \end{bmatrix}$
  - By Theorem 1, $\begin{vmatrix} A & 0 \\ 0 & I_q \end{vmatrix} = \det (A)$, and $\begin{vmatrix} I_p & 0 \\ 0 & D \end{vmatrix} = \det (D)$
  - And $\begin{vmatrix} I_p & 0 \\ C & I_q \end{vmatrix} = \begin{vmatrix} I_p \end{vmatrix} = 1$, which can show by cofactor expansion down a column from the right
  - Therefore, $\det (M) = \det (D) \cdot 1 \cdot \det (A) = \det (A) \det (D)$
- Also, obviously, if $M = \begin{bmatrix} A & 0 \\ 0 & D \end{bmatrix}$, then $\det (M) = \det (A) \det (D)$

Main Theorem proof
- If $A$ is invertible, by LDU decomposition, $M = \begin{bmatrix} A & B \\ C & D \end{bmatrix} = \begin{bmatrix} I_p & 0 \\ CA^{-1} & I_q \end{bmatrix} \begin{bmatrix} A & 0 \\ 0 & D - CA^{-1}B \end{bmatrix} \begin{bmatrix} I_p & A^{-1}B \\ 0 & I_q \end{bmatrix}$
  - By Theorem 2, $\begin{vmatrix} I_p & 0 \\ CA^{-1} & I_q \end{vmatrix} = 1$, $\begin{vmatrix} I_p & A^{-1}B \\ 0 & I_q \end{vmatrix} = 1$, and $\begin{vmatrix} A & 0 \\ 0 & D - CA^{-1}B \end{vmatrix} = \det (A) \det (D - CA^{-1}B)$
  - Therefore $\det M = 1 \cdot \det (A) \det (D - CA^{-1}B) \cdot 1 = \det (A) \det (D - CA^{-1}B)$
- If $D$ is invertible, by LDU decomposition, $M = \begin{bmatrix} A & B \\ C & D \end{bmatrix} = \begin{bmatrix} I_p & BD^{-1} \\ 0 & I_q \end{bmatrix} \begin{bmatrix} A - BD^{-1}C & 0 \\ 0 & D \end{bmatrix} \begin{bmatrix} I_p & 0 \\ D^{-1}C & I_q \end{bmatrix}$
  - By Theorem 2, $\begin{vmatrix} I_p & BD^{-1} \\ 0 & I_q \end{vmatrix} = 1$, $\begin{vmatrix} I_p & 0 \\ D^{-1}C & I_q \end{vmatrix} = 1$, and $\begin{vmatrix} A - BD^{-1}C & 0 \\ 0 & D \end{vmatrix} = \det (A - BD^{-1}C) \det (D) $
  - Therefore $\det M = 1 \cdot \det (A - BD^{-1}C) \det (D) \cdot 1 = \det (A - BD^{-1}C) \det (D)$
