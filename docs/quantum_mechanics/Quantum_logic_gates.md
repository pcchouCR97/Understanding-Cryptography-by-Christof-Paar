## Basic

we can write the vector representation for 2 qubits as

$$
\lvert \psi \rangle = v_{00} \lvert 00 \rangle + v_{01}\lvert 01 \rangle + v_{10} \lvert 10 \rangle + v_{11} \lvert 11 \rangle\ \rightarrow 
\begin{bmatrix}
v_{00} \\
v_{01} \\
v_{10} \\
v_{11} \\
\end{bmatrix}
$$

## Unitary Matrices
From the quantum mechanics, the only matrics that we can use are the **unitary** matrices, which are the matrices $U$ such that:
$$
U^{\dagger}U = UU^{\dagger} = I,
$$
where $I$ is the identity matrix and $u^{\dagger}$ is the adjoint of U, that is, the matrix obtained by transposing $U$ and replaing each element by its complex conjugate. {==This means that any unitary matrix $U$ is invertible and its inverse is given by $U^\dagger$==}. in quantum mechanics, the operations represented by these matrices are called **quantum gates**.

## CNOT Gate

Matrix Representation of the CNOT Gate
In the computational basis $\{\lvert 00 \rangle,\lvert 01 \rangle, \lvert 10 \rangle, \lvert 11 \rangle\}$, the CONT gate is represented as a **4x4 matrix**:

$$
\text{CNOT} =
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0
\end{bmatrix}
$$

- The first qubit is the control qubit.
- The second qubit is the target qubit.

## Hadamard Gate

The Hadamard gate is a single-qubit operation that maps the basis state $\lvert 0 \rangle$ to $\frac{\lvert 0 \rangle + \lvert 1 \rangle}{\sqrt{2}}$ and $\lvert 1 \rangle$ to $\frac{\lvert 0 \rangle - \lvert 1 \rangle}{\sqrt{2}}$, which creates an equal superposition of the basis states.

$$
H = \frac{1}{\sqrt{2}} 
\begin{bmatrix}
1 & 1  \\
1 & -1  \\
\end{bmatrix}
$$

if we apply $H$ gate on a qubits in state $\lvert 0 \rangle$ and $\lvert 1 \rangle$, we have:

$$
H\lvert0\rangle = \frac{1}{\sqrt{2}}(\lvert0\rangle+\lvert1\rangle)
$$

which is called **mplus** state and it is denoted as $\lvert+\rangle$; and 

$$
H\lvert1\rangle = \frac{1}{\sqrt{2}}(\lvert0\rangle-\lvert1\rangle)
$$

which is called **minus** state and it is denoted as $\lvert-\rangle$.

## Pauli-X gate
The pauli-X gate is a single-qubis rotation through $\pi$ radians around the X-axis. $X$ gate is also called `NOT` gate, since it performs like a `NOT` gate in classical digital circuits. Here's why:

$$
X = 
\begin{bmatrix}
0 & 1  \\
1 & 0 \\
\end{bmatrix}
$$

Apply $X$ gate to $\lvert 0 \rangle$ and $\lvert 1 \rangle$, we have:

$$
X\lvert0\rangle =  
\begin{array}{cc}
\begin{bmatrix}
0 & 1  \\
1 & 0 \\
\end{bmatrix}
\begin{bmatrix}
1 \\
0 \\
\end{bmatrix}
= \begin{bmatrix}
0 \\
1 \\
\end{bmatrix}
= \lvert 1 \rangle
,& 
X\lvert1\rangle =  
\begin{bmatrix}
0 & 1  \\
1 & 0 \\
\end{bmatrix}
\begin{bmatrix}
0 \\
1 \\
\end{bmatrix}
= \begin{bmatrix}
1 \\
0 \\
\end{bmatrix}
= \lvert 0 \rangle
\end{array}
$$

## Pauli-Y gate 
The Pauli-Y gate is a single-qubit rotation through π radians around the y-axis.

$$ 
Y = \sigma_{y} = \sigma_{2} =
\begin{bmatrix}
0 & -i  \\
i & 0  \\
\end{bmatrix}
$$

## Pauli-Z gate
The Pauli-Z gate is a single-qubit rotation through π radians around the z-axis.

$$ 
Z = \sigma_{z} = \sigma_{3} =
\begin{bmatrix}
1 & 0  \\
0 & -1 \\
\end{bmatrix}
$$

What if we apply an $H$ gate, then an $X$ gate and, finally, another $H$ gate. we have:

$$
Z = 
\begin{bmatrix}
1 & 0\\
0 & -1 \\
\end{bmatrix}
$$

and we have the following properties for when we apply $Z$ on $\lvert 0 \rangle$ and $\lvert 1 \rangle$:

$$
\begin{array}{ccc}
Z \lvert0\rangle = \lvert0\rangle & \text{and} & Z\lvert1\rangle = -\lvert1\rangle
\end{array}
$$

## S gates
S gate is given by:

$$
S = 
\begin{bmatrix}
1 & 0  \\
0 & e^{i\frac{\pi}{2}} \\
\end{bmatrix}
$$

## T gates
T gate is given by:

$$
T = 
\begin{bmatrix}
1 & 0  \\
0 & e^{i\frac{\pi}{4}} \\
\end{bmatrix}
$$





















































---

## Reference
1. https://www.quantum-inspire.com/kbase/pauli-x/

