# Quantum Approximate Optimization Algorithm

Specifically, we have studied how to write problems using the QUBO formalism and how to use quantum annealers to sample approximate solutions.

In this chapter, we are going to showhowthe ideas thatwe have already explored can also be used on digital quantum computers. We will be using our beloved quantum circuits — with all their qubits, quantum gates, and measurements — to solve combinatorial optimization problems formulated in the QUBO framework.

we will be studying the famous Quantum Approximate Optimization Algorithm (QAOA), which is a gate-based algorithm that can be understood to be the counterpart to quantum annealing in the quantum circuit model.

We will start by introducing all the theoretical concepts that are needed in order to understand this algorithm, then we will study the kind of circuits used in its implementation, and finally, we will explain how to run QAOA with both Qiskit and PennyLane.

After reading this chapter, you will understand how QAOA works, you will know how to design the circuits used in the algorithm, and you will be able to solve your own combinatorial optimization problems using QAOA in Qiskit and PennyLane.

## From adiabatic computing to QAOA
### Discretizing adiabatic quantum computing