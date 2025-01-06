# Variational Quantum Eigensolver (VQE)
The VQE can cover more general settings of the optimization problems sucha as chemistry and physis.

The Variational Quantum Eigensolver (VQE), which can be seen as a generalization of the Quantum Approximate Optimization Algorithm of Quantum Approximate Optimization (QAOA). Or we can said that the QAOA algorithm is a special case of the VQE.

## Hamiltonians, observables, and their expectation values 
### Hamiltonians 

From QUBO, we know that we can start with a function $f$ that assigns real numbers to binary strings of a certain length $n$, and we try to find a binary string $x$ with minimum cost $f(x)$. To perform this algorithm, we then define a Hamiltonian $H_{f}$ such that 

$$
\langle x|H| \rangle = f(x)
$$

holds for every binary string $x$ of length $x$. Then, we can solve the optimal value by finding a ground state of $H_{f}$. That is, a state $\lvert \psi \rangle$ such that the expectation value $\langle x|H_{f}| \rangle = f(x)$ is minimum.

now we introduce a new property of the Hamiltonian, For every computational basis state $\lvert x \rangle$, it holds that 

$$
H_{f}\lvert x \rangle = f(x)\lvert x \rangle.
$$

### Observables


### Expectation values 


## Intoducing the Variational Quantum Eigensolver


## Using VQE with Qiskit


## USing VQE with PennyLane