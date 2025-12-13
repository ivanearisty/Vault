(ii) Row operations by blocks

We start with the block matrix M=(1u​−vtA​). We want to eliminate the u in the (2,1) block. We can perform the block row operation R2​→R2​−uR1​. This is equivalent to pre-multiplying by the block matrix (1−u​0I​).
$$\begin{pmatrix} 1 & 0 \\ -u & I \end{pmatrix} \begin{pmatrix} 1 & -v^t \\ u & A \end{pmatrix} = \begin{pmatrix} 1 & -v^t \\ -u(1) + I(u) & -u(-v^t) + I(A) \end{pmatrix} = \begin{pmatrix} 1 & -v^t \\ 0 & A + uv^t \end{pmatrix} $$The determinant of the block lower-triangular matrix $\begin{pmatrix} 1 & 0 \\ -u & I \end{pmatrix}$ is $\det(1) \cdot \det(I) = 1$. Since $\det(XY) = \det(X)\det(Y)$, we have: $$\det\left(\begin{pmatrix} 1 & 0 \\ -u & I \end{pmatrix}\right) \det\left(\begin{pmatrix} 1 & -v^t \\ u & A \end{pmatrix}\right) = \det\left(\begin{pmatrix} 1 & -v^t \\ 0 & A + uv^t \end{pmatrix}\right) \\ 1 \cdot \det\left(\begin{pmatrix} 1 & -v^t \\ u & A \end{pmatrix}\right) = \det\left(\begin{pmatrix} 1 & -v^t \\ 0 & A + uv^t \end{pmatrix}\right) $$This shows the required equality. ----- ### (iii) Column operations by blocks We again start with $M = \begin{pmatrix} 1 & -v^t \\ u & A \end{pmatrix}$. We want to eliminate the $u$ in the (2,1) block using column operations. Let the first column block be $C_1 = \begin{pmatrix} 1 \\ u \end{pmatrix}$ and the second column block be $C_B = \begin{pmatrix} -v^t \\ A \end{pmatrix}$. We need to find a vector $x$ such that $C_1 + C_B x = \begin{pmatrix} \ast \\ 0 \end{pmatrix}$. $$

\begin{pmatrix} 1 \ u \end{pmatrix} + \begin{pmatrix} -v^t \ A \end{pmatrix} x = \begin{pmatrix} 1 - v^t x \ u + Ax \end{pmatrix} $$To make the (2,1) block zero, we set u+Ax=0. Since A is invertible, we can solve for x: x=−A−1u.

Now, substitute this x back into the (1,1) block: 1−vtx=1−vt(−A−1u)=1+vtA−1u.

The column operation C1​→C1​+CB​(−A−1u) does not change the determinant. This operation transforms the matrix M into:

$$\begin{pmatrix} 1 + v^t A^{-1} u & -v^t \ 0 & A \end{pmatrix} $$Thus, we have shown the equality: