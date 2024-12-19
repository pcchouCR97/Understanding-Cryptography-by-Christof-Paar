# Basic

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

# CNOT Gate

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

# Hadamard Gate

The Hadamard gate is a single-qubit operation that maps the basis state $\lvert 0 \rangle$ to $\frac{\lvert 0 \rangle + \lvert 1 \rangle}{\sqrt{2}}$ and $\lvert 1 \rangle$ to $\frac{\lvert 0 \rangle - \lvert 1 \rangle}{\sqrt{2}}$, which creates an equal superposition of the basis states.

$$
H = \frac{1}{\sqrt{2}} 
\begin{bmatrix}
1 & 1  \\
1 & -1  \\
\end{bmatrix}
$$

# Pauli-X gate
The pauli-X gate is a single-qubis rotation through $\pi$ radians around the X-axis.

$$ 
X = \sigma_{x} = \sigma_{1} =
\begin{bmatrix}
1 & 1  \\
1 & -1  \\
\end{bmatrix}
$$

# Pauli-Y gate 
The Pauli-Y gate is a single-qubit rotation through π radians around the y-axis.

$$ 
Y = \sigma_{y} = \sigma_{2} =
\begin{bmatrix}
0 & -i  \\
i & 0  \\
\end{bmatrix}
$$

# Pauli-Z gate
The Pauli-Z gate is a single-qubit rotation through π radians around the z-axis.

$$ 
Z = \sigma_{z} = \sigma_{3} =
\begin{bmatrix}
1 & 0  \\
0 & -1  \\
\end{bmatrix}
$$

### Reference
1. https://www.quantum-inspire.com/kbase/pauli-x/

