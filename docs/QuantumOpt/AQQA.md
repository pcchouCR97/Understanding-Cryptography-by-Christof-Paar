# Adiabatic Quantum Computing & Quantum Annealing (AQQA)
Quantum annealers is a special type of quantum computer which is designed to find the approcimate solutions to combinatorial optimization problems.

## Adiabatic Quantum Computing

When using quantum circuits, we apply operations through discrete, sequential steps. However, adiabatic quantum computing relies on the use of continuous transformations.

That's is, we will use a $H(t)$ that will vary with time and that will be the driving force to change the state of our qubits according to time-dependent Schrödinger equation:

$$
\hat{H}(t) \lvert\psi(t)\rangle = i \hbar \frac{\partial \lvert\psi(t)\rangle}{\partial t}
$$

where:

1. $\hbar$ is the reduced Planck's constant,
2. $\lvert\psi(t)\rangle = \psi(x,t)$ is the wave function,
3. $\hat{H}(t)$ is the Hamiltonian operator, time-dependent.
4. $i$ is the imaginary unit.

This means that the energy can varies with time.

In addition to using time-dependent Hamiltonians, **adiabatic evolution**, the other important ingredient in our new quantum algorithm model. 

!!! tip
    The adiabatic process is one in which the ***energy configuration*** of the system changes ***very gently***.

{==The key observation is that we will be considering problem whose optimal solutions will correspond to minimum-energy or ground states of some Hamiltonian of an Ising model.==} If we make our model so that the final Hamiltonian of the system is the one whose ground state will yield the solution to our problem, then we only need to measure the system to get the solution we are looking for.


!!! tip
    the idea behind adiabatic quantum computing is to start with a simple Hamiltonian, one for which we can easily obtain — and prepare! — the ground state, and evolve it “carefully.”

Of course, the crucial thing here is how to perform the evolution to ensure that it is, indeed,
adiabatic. But don’t worry, the adiabatic theorem from Max Born and Vladimir Fock, two of the fathers of quantum mechanics, says that for your process to be adiabatic, it should be slow enough.

The tips is that the total time should be *inversely proportional* to the square of the **spectral gap**, which is the minimum difference in energy between the ground state and the first excited state of the Hamiltonian during the whole evolution. That is:

1. If the difference is big, you’d better be faster!
2. If the difference is small, you’d better be careful! -> slower

In a more formal way, suppose that you have a problem for which $H_1$ is the Hamiltonian whose ground state encodes the result that you want to find. For instance, $H_1$ could be an Ising Hamiltonian that you obtained from transforming a QUBO problem. {==Now, imagine that your system is in the ground state of some initial Hamiltonian $H_0$.==} We will soon discuss how to choose $H_0$, but for now just think that you can prepare its ground state easily enough so that it is a natural choice for you. Suppose that we run the process for total time $T$. The time-dependent Hamiltonian that we consider will be of the form 

$$
H(t) = A(t)H_{0} + B(t)H_{1}
$$
where $A$ and $B$ are real-valued functions that accept inputs over the interval $[0, T]$ such that $A(0) = B(T) = 1$ and $A(T) = B(0) = 0$. {==Notice that it holds that $H(0) = H_{0}$ and $H(T) = H_{1}$, exactly as we desired==}. however, a more common choose is to set $A(t) = 1-t/T$ and $B(t) = t/T$. 

## Quantum annealing
Quantum annealing relies on the same core idea as adiabatic quantum computing: it takes an initial Hamiltonian $H_0$, a final Hamiltonian $H_1$ whose ground state encodes the solution to the problem of interest, and it {==gradually changes the acting Hamiltonian from the initial to the final==} one by using some functions $A$ and $A$ (as described in the previous section) to decrease the action of $H_0$ and to increase the action of $H_1$. However, there are two difference between the quantum annealing and adiabatic quantum computing.

### 1. Difference in Hamiltonian
The final Hamiltonian $H_1$ that can be realized, in the practial implementations of quantum annealing, must be selected from a certain way. 

The initial Hamiltonian in the quantum annealing setup is also usually fixed to be $H_{0} = -\sum_{j=0}^{n-1} X_{j}$, where $n$ is the number of qubits, and $X_{j}$ stands for the tensor product in which the $X$ matrix is acting on qubit $j$ with the rest of positions occupied by $I$ (yes, the identity matrix). The Ground state of $H_{0}$ is easily seen to be $\bigotimes_{i=0}^{n-1} \lvert+\rangle$, the tensor product of $n$ copies of the plus state, which is relatively easy to prepare because it is completely unentangled. 

Thus, the Hamiltonian used in quantum annealing is given by:

$$
H(t) = -A(t)\sum_{j=0}^{n-1}X_{j}-B(t)\sum_{j,k}J_{jk}Z_{j}Z_{k}-B(t)\sum_{j}h_{j}Z_{j},
$$

where $J_{j,k}$ and $h_{j}$ are some adjustable coefficicents, and $A$ and $B$ are functions such that $A(0) = B(T) = 1$ and $A(T) = B(0) = 0$, with T being the total **annealing time**, $A$ and $B$ are called the **annealing schudle**.

### 2. Difference in spectral gap
In the Quantum annealing, evolution is no longer guaranteed to be adiabatic. Here are the two main reasons:

1. The spectral gap is the minimum of the difference between the ground sate and the first state of $H(t)$ for $t\in[0,T]$, which can be very difficult to compute, sometimes, it can be more difficult to compute than find the ground state that we are looking for. Cubitt et al. [38].
2. The time we need to compute the adiabatic process (evolution) could be too large.

Thus, we only run the evolution for a certain amount og time that need not satisfy the conditions for adiabaticity, and hope to still be able to find good approximations of the optimal solution to our problem. {==In fact, remaining in the ground state of $H(t)$  is not always needed==}. Since, at the end, we are going to measure the state, it would be enough if the amplitude of an optimal or sufficiently good solution in our final state were big enough. That’s because, then, the probability of obtaining a useful result will still be high.

!!! tip
    And, of course, we can always repeat the process several times and keep the best of all measurements!

### Working with D-wave
D-Wave, the first company commercialize a quantum device that implemented quantum annealing as we have just covered. We need to keep in mind that, with these quantum computers, {==the evolution process will not be adiabatic in general, so there is no guarantee that the exact solution will be found in all cases.==}

Using D-wave is easier than you think. You need to install Ocean, which is D-Wave's quantum annealing Python library, and to create a free account on D-Wave Laep, a cloud service where you can get one minute per month of free computing time on D-Wave's quantum annealers. 

Once you have everything set up, you can access quantum annealers to find an approximation of a solution to any combinatiorial optimiazation problem that you may have written as either an instance of finding the groud state of an Ising model or as a QUBO problem. Let's try with the MaxCut problem from [QUBO](../QuantumOpt/QUBO.md) first! 

![MaxCut0](../QuantumOpt/images/MaxCut0.png)

ok let's do some review first, for the Ising model from [QUBO](../QuantumOpt/QUBO.md), we have

$$
\begin{array}{ll}
\text{Minimize} & -\sum_{(j,k)\in E} J_{jk}\langle \psi \lvert Z_{j}Z_{k}\lvert \psi \rangle -\sum_{j} h_{j}\langle \psi \lvert Z_{j}\lvert \psi \rangle \\
\text{where} & \lvert \psi \rangle \text{is taken from the set of quantum states on n qubis}
\end{array}
$$

and we can reduce to a 3 vertices problem to the following:

$$
\begin{array}{ll}
\text{Minimize} & \langle \psi\lvert(Z_{0}Z_{1}+Z_{0}Z_{2})\lvert \psi\rangle = \langle \psi \lvert Z_{0}Z_{1}\lvert \psi \rangle + \langle \psi \lvert Z_{0}Z_{2}\lvert \psi \rangle,\\
\text{where} & \lvert \psi \rangle \text{is taken from the set of quantum states on 3 qubis}
\end{array}
$$

we can write the ground state as,

$$
Z_{0}Z_{1} + Z_{0}Z_{2}
$$

which is, of course, an Ising Hamiltonian in which $J_{01} = J_{02} = 1$ and the rest of the coefficients are $0$.

Next, all we need is to tell the quantum annealer is that those are the coefficients we want to use, and then we can perfrom the annealing multiple times to obtain some resutls that will hopefully wolve our problem. To specift the problem, we can use the `dimod` package inclided in the Ocean library.

Next, let's move to our Jupyter file to work on this!


## Takeaways
1. **Adiabatic Quantum Computing**:
   - Adiabatic quantum computing (AQC) is equivalent to the quantum circuit model but uses a time-dependent Hamiltonian for continuous evolution instead of discrete quantum gates.
   - The adiabatic theorem ensures that, with slow enough evolution, the ground state can be measured at the end.

2. **Quantum Annealing**:
   - In practice, quantum annealing is used over AQC because adiabatic evolution can be too slow for practical applications.
   - D-Wave Leap is a platform that enables finding approximate solutions to combinatorial optimization problems using quantum annealers.

3. **Parameter Control**:
   - Understanding and controlling annealing parameters improves solution quality with quantum annealers.

4. **Hybrid Solvers**:
   - Hybrid solvers divide large problems into smaller pieces and combine classical and quantum methods to find global solutions.

5. **Future Topic**:
   - The next focus will be on discretizing quantum annealing to implement it using quantum gates, integrating it into the quantum circuit model.