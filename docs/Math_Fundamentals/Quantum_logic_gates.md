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

# 

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