# Matrices

## Hermitian matrix

A **Hermitian matrix** is a complex square matrix that is equal to its own conjugate transpose - that is, the element in the $i$-th row and $j$-th column is equal to the complex conjugate of the element in the $j$-th row and $i$-th column, for all indices $i$ and $j$. A is a Hermitian matrix $A$ contains elements $a_{ij}$, then 

$$
a_{ij} = \overline{a_{ij}}
$$

or in a quantum mechanics notation

$$
A^{H} = \overline{A^{T}} = A^{\dagger} .
$$

For example, we have a complex square matrix $A$ such that

$$
A = 
\begin{bmatrix}
0 & a-ib & c-id \\
a+ib & 1 & m-in \\
c+id & m+in & 2
\end{bmatrix}
$$

we apply transpose, it becomes

$$
{A^{T}} =
\begin{bmatrix}
0 & a+ib & c+id \\
a-ib & 1 & m+in \\
c-id & m-in & 2
\end{bmatrix} 
$$

then we find its cimplex conjugate, we have 

$$
\overline{A^{T}} =
\begin{bmatrix}
0 & a-ib & c-id \\
a+ib & 1 & m-in \\
c+id & m+in & 2
\end{bmatrix} 
= A^{\dagger} = A
$$

### Diagonal values 
The entries on the main diagonal of any Hermitian matrix are real.

### Symmetric 
A matrix that has only real entries is symmetric if and only if it is a Hermitian matrix. A real and symmetric matrix is simply a special case of a Hermitian matrix.

### Normal 
Every Hermitian matrix is a normal matrix. $AA^{\dagger} = A^{\dagger}A$.

### Properties
For any Hermitian matrix $A$ and $B$, we have 

1. $(A+B)_{ij} = A_{ij}+B_{ij} = \overline{A_{ij}}+\overline{B_{ij}} = \overline{A+B}_{ij}$ 
2. For an invertible Hermitian matrix. $A^{-1} = (A^{-1})^{H}$


## Normal matrix 
A complex square matrix $A$ is **normal**if it commutes with its conjugate transpose $A^{*}$. 

$$
A^{*}A = AA^{*}
$$

## Conjugate transpose

To maintain the consistancy, symbol $H$ has been replaced to $\dagger$.

A square matrix $A$ with entries $a_{ij}$ is called 

1. Hermitian or self-adjoint if $A = A^{\dagger}$; i.e., $a_{ij} = \overline{a_{ji}}$.
2. Skew Hermitian if $A = -A^{\dagger}$; i.e., $a_{ij} = -\overline{a_{ji}}$.
3. Normal if $A^{\dagger}A = AA^{\dagger}$.
4. Unitary if $A^{\dagger} = A^{-1}$; i.e., $AA^{\dagger} = A^{\dagger}A = I$.

## Unitary matrix 

In linear algebra, an invertible complex square matrix $U$ is **unitary** if its inverse matrix $U^{-1}$ equals its conjugate transpose $U^{*}$, that is, if 

$$
U^{*}U = UU^{*} = I
$$

In quantum, we use $\dagger$, that is

$$
U^{\dagger}U = UU^{\dagger} = I
$$

For real numbers, the analogue of a unitary matrix is an orthogonal matrix. Unitary matrices have significant importance in quantum mechanics because they preserve norms, and thus, probability.

## Orthogonal matrix
 
In liner algrbra, an **orthogonal matrix**, or **orthonormal matrix**, is a real square matrix whose columns and rows are orthonormal vectors.

Also, it holds that 

$$
Q^{T}Q = QQ^{T} = I
$$

where $Q^{T}$ is the transpose of $Q$ and $I$ is the identity matrix. As a result, it also holds that

$$
Q^{T} = Q^{-1}.
$$

## References:
1. [Conjugate transpose (Wiki)](https://en.wikipedia.org/wiki/Conjugate_transpose)
2. [Orthogonal matrix (Wiki)](https://en.wikipedia.org/wiki/Orthogonal_matrix)
3. [Hermitian matrix (Wiki)](https://en.wikipedia.org/wiki/Hermitian_matrix)