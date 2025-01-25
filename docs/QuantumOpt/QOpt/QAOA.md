# Quantum Approximate Optimization Algorithm

Specifically, we have studied how to write problems using the QUBO formalism and how to use quantum annealers to sample approximate solutions.

In this chapter, we are going to showhowthe ideas thatwe have already explored can also be used on digital quantum computers. We will be using our beloved quantum circuits — with all their qubits, quantum gates, and measurements — to solve combinatorial optimization problems formulated in the QUBO framework.

we will be studying the famous Quantum Approximate Optimization Algorithm (QAOA), which is a gate-based algorithm that can be understood to be the counterpart to quantum annealing in the quantum circuit model.

We will start by introducing all the theoretical concepts that are needed in order to understand this algorithm, then we will study the kind of circuits used in its implementation, and finally, we will explain how to run QAOA with both Qiskit and PennyLane.

After reading this chapter, you will understand how QAOA works, you will know how to design the circuits used in the algorithm, and you will be able to solve your own combinatorial optimization problems using QAOA in Qiskit and PennyLane.

## From adiabatic computing to QAOA
### Discretizing adiabatic quantum computing
In the previous setion, we've learn how to use adiabatic quantum computing and the quantum annearling to calculate combinatorial optimization problems. Both methods relied on the adiabatic theorem.

Also, in the adiabatic quantum computing, we used a time-dependent Hamiltonian that induced a continuous transformation of the state of a quantum system: from an initial state to a final state that - hopefully- has a big overlap with the solution to our problem.

A natural question to ask is whether there is any sort of analog to this way of solving optimization problems for circuit-based quantum computers. However, you might already wonder how can we resolve the difference between the discrete opertaions in the quantum circuit and continuous adiabatic quantum computing.

To answer this question, we use a method call **Trotterization**, which **discretize** any continuous evolution, approximating it with a sequenca of small, discrete changes.

QAOA was initially proposed as a **discretization** of adiabatic quantum computing with the goal of approximating the optimal solution to combinatorial optimization problems. The Hamiltonian used in adiabatic quantum computing and quantum annealing is of the form:

$$
H(t) = A(t)H_{0} + B(t)H_{1},
$$

with $H_{0}$ and $H_{1}$ two fixed Hamiltonians and $A(t)$ and $B(t)$ functions satifying $A(0) = B(T) = 1$ and $A(T) = B(0) = 0$, where $T$ is the total time of the process. It turns out that the evolution of the quantum system is governed by the famous time-depent Schrödinger equation. If you can solve this equation, you will get the state vector at any moment $t$ between $0$ and $T$. 

For QAOA, all we need to know is that, applying discretization, we can express the solution as a production of operators of the form

$$
e^{i\Delta t(A(t_{c})H_{0} + B (t_{c})H_{1})}
$$

applied to the initial state. Where,

1. $i$ is the imaginary unit.
2. $t_{c}$ is a fixed time point in $[0,T]$
3. $\Delta t$ is a small amount of time.

The key idea is that in the inteval $[t_{c}, t_{c} + \Delta]$ we assume that the Hamiltonian is constant and equal to $H(t_{c}) = A(t_c)H_{0} + B(t_{c})H_{1}$. Of course, intuitively, the smaller $\Delta t$ is, the better this approximation will be. It is also critical to know that $e^{i\Delta t(A(t_{c})H_{0} + B (t_{c})H_{1})}$ is a **unitary transformation** (A operation satisfies $U^{\dagger}U = UU^{\dagger} = I, \text{where} \ U^{\dagger} \text{is the adjoint of}\ U$. You may remember, some of the quantum gates such as $R_X$, $R_Y$, and $R_Z$ are also exponentials of some matirces. By using the descretiation technique, if $\lvert \psi \rangle$ is the initial state, then the final state can be approximated by

$$
\bigg( \prod_{m = 0}^{p}e^{i\Delta t(A(t_{m})H_{0} + B (t_{m})H_{1})} \bigg) \lvert \psi_{0} \rangle
$$

where $t_{m} = m\frac{\Delta t}{T}$, and $p = \frac{T}{\Delta t}$.

To compute this state with a quantum circuit, we just need an additional approximation. You know that $e^{a+b} = e^{a}e^{b}$ for any real numbers. However, this identity doesn't hold for a matrices, unless the matrices **commute**. However, if $\Delta t$ is small, then 

$$
e^{i\Delta t(A(t_{m})H_{0} + B (t_{m})H_{1})}  \approx e^{i\Delta tA(t_{c})H_{0}}\ e^{i\Delta t B (t_{c})H_{1}},
$$

which is known as the **Lie-Trotter formula**.

Putting the Lie-Trotter formula with the discretization we mentioned above, we can obtain,

$$
\prod_{m = 0}^{p} e^{i\Delta tA(t_{c})H_{0}}\ e^{i\Delta t B (t_{c})H_{1}} \lvert \psi_{0} \rangle,
$$

which is the inspiration for QAOA.

## QAOA Algorithm
Again, let's start with a combinatorial optimization problem that we want to solve, and we encode it into an Ising Hamiltonian $H_{1}$. To find its round state and solve our problem, we seek to apply a quantum state evolution similar to that of quantum annealing, but using a quantum circuit instead of a quantum annealer.

To simulate with a quantum circuit the evolution of a state under a time-dependent Hamiltonian, you only need to take an initial state $\lvert \psi \rangle$ and the alternate for $p$ times the application of the operators $e^{i\gamma H_{1}}$ and $e^{i\beta H_{0}}$ for some values of $\gamma$ and $\beta$. We will see that the unitary transformations $e^{i\gamma H_{1}}$ and $e^{i\beta H_{0}}$ can be implemented with just one-qubit and two-qubit quantum gates.

What we are doing is using a quantum circuit to prepare a state of the form

$$
e^{i\beta_{p}H_{0}}e^{i\gamma_{p}H_{1}} \dots e^{i\beta_{2}H_{0}}e^{i\gamma_{2}H_{1}} e^{i\beta_{1}H_{0}}e^{i\gamma_{1}H_{1}}  \lvert \psi_{0} \rangle,
$$

where $p \geq 1.$ Usually, we collect all the coefficients in the exponents in two tuples $\beta = (\beta_{1}, \cdots, \beta_{p})$ and $\gamma = (\gamma_{1}, \cdots, \gamma_{p})$ and we denote the whole state by $\lvert \beta , \gamma \rangle$.

In QAOA, we choose a fixed value of $p$ and we have some values for $\beta$ and $\gamma$. We'll just consider these values to be plain real numbers. So how to choose the *best* possible values? Remember that we are just trying to find the ground state of $H_{1}$, the lower the value of the energy $\langle \beta, \gamma \lvert H_{1} \lvert \beta, \gamma \rangle$, the better. In this way, we have transformed our optimization problem into finding the values $\beta$ and $\gamma$ that minimize 

$$
E(\beta, \gamma) = \langle \beta, \gamma \lvert H_{1} \lvert \beta, \gamma \rangle.
$$

Since $\beta$, $\gamma$, and energy $E(\beta, \gamma)$ are real numbers, we are just having a problem of finding a minimum for a real-valued function with real inputs. Therefore, we can just apply some optimization algorith such as the gradient descent algorithm. However, as we know, the number of amplitudes needed to describe a state like $\lvert \beta , \gamma \rangle$ is {==exponential==} in the number of qubits that we are using. Thus, computing $E(\beta, \gamma)$ may be difficult with just a classical computer.

Luckly, estimating value of $E(\beta, \gamma)$ is something that we can do very efficiently with a quantum computer - when the number of terms in $H_1$ is polynomial in the number of qubits.

{==Then we can use the classical optimization algorithm for function monimization and use a quantum computer to estimate $E$ value. Then we give that value back to the classical algorithm until it needs another of value $E$.==} This is what we call a **hybrid algorithm**.

Once we obtained the optimal values $\beta^{*}$ and $\gamma^{*}$ for $\beta$ and $\gamma$, we can use the quantum computer once more in order to prepare the state $\lvert \beta^{*}, \gamma^{*}\rangle$. This state should have a sizeable overlap with the ground state of $H_{1}$, so when we measure it in the computational basis, we will have a good chance of obtaining a string of zeros and ones that is a good solution to our original problem.

---

Let's put everything together! 

1. The input to QAOA is an Ising Hamiltonian $H_{1}$, the ground state of which we wich to approximate becasuse it encodes the solution to a certain combinatorial optimization problem. 
2. We consider the energy function $E(\beta, \gamma)$ as defined before and we proceed to minimize it.
3. Pick $p \geq 1$ and some initial values $\beta_{0}$ and $\gamma_{0}$ for some optimization algorithm.
4. Run the optimization, use quantum computer $E$ to prepare the state $\lvert \beta, \gamma \rangle$ and to estimate energy, retrun the value back to the optmization algorithm.
5. Continue this porcess until the classical optmization algorithm finds the optimal values of $\beta^{*}$ and $\gamma^{*}$.
6. Using qunautm copmuter to prepare $\lvert \beta^{*}, \gamma^{*}\rangle$. 

---

Here is the pseudocode in the following algorithm:

Algorithm 5.1 (QAOA).

1. **Choose a value for** `p`.
2. **Choose a starting set of values**:
    - `β = (β₁, ..., βₚ)`
    - `γ = (γ₁, ..., γₚ)`
3. **While the stopping criteria are not met**, do the following:
    - Prepare state `|β, γ⟩`. *This is done on the quantum computer!*
    - From measurements of `|β, γ⟩`, estimate `E(β, γ)`.
    - Update `β` and `γ` according to the minimization algorithm.
4. Obtain the optimal values:
    - `β*` and `γ*` returned by the minimization algorithm.
5. Prepare state `|β*, γ*⟩`. *This is done on the quantum computer!*
6. Measure the state to obtain an approximate solution.

---

## Circuits of QAOA
For QAOA, it always involves with preparing state of the form 

$$
\lvert \beta, \gamma \rangle = e^{i\beta_{p}H_{0}}e^{i\gamma_{p}H_{1}} \dots e^{i\beta_{2}H_{0}}e^{i\gamma_{2}H_{1}} e^{i\beta_{1}H_{0}}e^{i\gamma_{1}H_{1}}  \lvert \psi_{0} \rangle,
$$

where $\lvert \psi_{0} \rangle$ is the groudn state of $H_{0}$. As we know, $H_{0}$ is usually taken to be $-\sum_{j=0}^{n-1} X_{j}$, while $H_{1}$ is an Ising Hamiltonian of the form 

$$
-\sum_{j,k} J_{jk}Z_{j}Z_{k} - \sum_{j} h_{j}Z_{j}
$$

where the coefficients $J_{jk}$ and $h_{j}$ are real numbers. The ground state of $H_{0}$ is $\bigotimes_{i=1}^{n-1} \lvert + \rangle$. This state can be esaily prepared, you just need to use Hadamard gate on each qubit of the circuit starting from $\lvert 0 \rangle$.

Let's focus on the operations of the form $e^{i \beta_{k}H_{0}}$, with $\beta_{j}$ a real number. We also know that $H_{0} = -\sum_{j=0}^{n-1} X_{j}$ and that all $X_{j}$ matrices commute with each other. Thus, we can say

$$
e^{i \beta_{k}H_{0}} = e^{-i \beta_{k} \sum_{j=0}^{n-1}X_{j}} = \prod_{j=0}^{n-1} e^{-i \beta_{k}X_{j}}
$$

{==But $e^{i \beta X_{j}}$ is the expression for the rotation gate $R_{X}(2\beta)$, so this means that we just need to apply this gate to each of the qubits in our circuit.==}

Next, we will take care of the $e^{i \gamma_{l}H_{1}}$ for any real coefficient $\gamma_{l}$. We know that $H_{1}$ is a sum of terms of the form $J_{jk}Z_{j}Z_{k}$ and $h_{j}Z_{j}$. Since these matrices commute with each other, we get

$$
e^{i \gamma_{l}H_{1}} = e^{i\gamma_{l}(\sum_{j,k} J_{jk}Z_{j}Z_{k} + \sum_{j}h_{j}Z_{j})} = \prod_{j,k}e^{-i\gamma_{l}J_{jk}Z_{j}Z_{k}} \prod_{j}e^{-i \gamma_{l} h_{j} Z_{j}}
$$

Same, the operations of the form $e^{-i\gamma_{l}h_{j}Z_{j}}$ can be carried out with rotation gate $R_{Z}$. Thus, we only need to figure out how to represent $e^{-i \gamma_{l} h_{j} Z_{j}}$ using quantum gates. 

1. First, let's denote the real number $\gamma_{l}J_{jk}$ by $a$. 
2. Notice that $e^{iaZ_{j}Z_{k}}$ is the exponential of a diagonal matrix, since $Z_{j}Z_{k}$ is the tensor product of diagonal matrices.
3. If $\lvert x \rangle$ is a computational basis, we have the following properties

$$
    \begin{array}{ll}
        e^{-iaZ_{j}Z_{k}}\lvert x \rangle = e^{-ia}\lvert x \rangle & \text{if qubit j and k have the same values} \\
        e^{-iaZ_{j}Z_{k}}\lvert x \rangle = e^{ia}\lvert x \rangle & \text{if qubit j and k have the different values}
    \end{array}
$$

This unitary action is implemented by the circuit below, where we have only depicted qubits $j$ and $k$.

<div style="text-align: center;">
    <img src="../../images_QOpt/circuit_implementation_QAOA_1.png" alt="circuit_implementation_QAOA_1" style="width: 500px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Circuit implementation QAOA 1
    </p>
</div>

Figure. Circuit Implementation of $e^{-iaZ_{j}Z_{k}}$

Imagine that the Ising Hamiltonian of your problem is $3Z_{0}Z_{2} - Z_{1}Z_{2} + 2Z_{0}$ Then, the circuit used by QAOA to prepare $\beta, \gamma$

<div style="text-align: center;">
    <img src="../../images_QOpt/circuit_implementation_QAOA_2.png" alt="circuit_implementation_QAOA_2" style="width: 850px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Circuit implementation QAOA 2
    </p>
</div>

Figure. QAOA circuit with $p=1$

Don't worry, let's go through this together!

1. First, we prepare the ground state of $H_0$ with a column of Hadamard gates.
2. Remember that we set $a = \gamma_{l}J_{jk}$ and $e^{-iaZ_{j}Z_{k}}\lvert x \rangle = e^{ia}\lvert x \rangle$ if qubit j and k have the different values. Since $J = 3$, we implemented $R_{Z}(2a = 2 \times 3\gamma_{l} = 6 \gamma_{1})$ with CNOT gate between qubit 0 and 2. 
3. Next, we perform the same operation between qubit 1 and 2 with $R_{Z}(2a = 2 \times -1\gamma_{l} = -2 \gamma_{1})$ and CNOT gates.
4. Then we use an $R_Z$ gate on qubit $0$ to implement $e^{-i2\gamma_{1}Z_{0}}$.
5. Finally, a column of $R_X(2\beta_1)$ gates implements $e^{-i\beta_{1}\sum_{j}X_{j}}$

If we increase the number of **layers** $p$, the circuit would grow by repeating for another $p-1$ times the very same circuit structure shown above except for the inintal Hadamard gates. More, we have to replace $\beta$ and $\gamma$ with corresponding $p$.

## Estimating the energy
After knowing how to construct a quantum circuit, let's see how to estimate the energy for the state $\lvert \beta, \gamma \rangle$.

Here, we are more interested in the energy of the $\lvert \beta, \gamma \rangle$ since this is the quantity that we want to minimize. That's beeing say, we need to evaluate $\langle \beta, \gamma \lvert H_{1} \lvert \beta, \gamma \rangle$. Of course, we don't have to access to the state vector since we re preparing the state with a quantum computer.

Since we already know how to evaluate efficiently $\langle x\lvert H_{1} \lvert x \rangle$ for any basis state $\lvert x \rangle$/. In fact, $\langle x\lvert H_{1} \lvert x \rangle$ is the value of $x$ in the cost function of our combinatorial optimization problem, because we derived $H_{1}$ from it. We only need to notice that $\langle x\lvert Z_{j} \lvert x \rangle = 1$ if the $j$-th bit of $x$ is 0 and that $\langle x\lvert Z_{j} \lvert x \rangle = -1$ otherwise. In the same fashion, $\langle x\lvert Z_{j}Z_{k} \lvert x \rangle = 1$ if $j$-th and $k$=th buts of x are equal and $\langle x\lvert Z_{j}Z_{k} \lvert x \rangle = -1$ if they are different.

Let's look into this problem, for instance, try to evaluate $\langle x \lvert H_{1} \lvert x \rangle$ if $H_{1} = 3Z_{0}Z_{2}-Z_{1}Z_{2}+2Z_{0}$, 

$$
\langle 101 \lvert H_{1} \lvert 101\rangle = 3\langle 101 \lvert Z_{0}Z_{2} \lvert 101\rangle - \langle 101 \lvert Z_{1}Z_{2} \lvert 101\rangle + 2\langle 101 \lvert Z_{0} \lvert 101\rangle = 3 + 1 - 2 = 4
$$

!!! note 
    Try to evaluate $\langle 100 \lvert H_{1} \lvert 100\rangle$ with $H_{1} = 3Z_{0}Z_{2}-Z_{1}Z_{2}+2Z_{0}$ = ?

We also know that the can always write $\lvert \beta, \gamma \rangle$ as a linear combination of basis state,

$$
\lvert \beta, \gamma \rangle = \sum_{x} a_{x} \lvert x \rangle
$$

for certain amplitudes $a_{x}$ such that $\sum_{x}|a_{x}|^{2} = 1$. Therefore, we can have,

$$
\langle \beta, \gamma \lvert H_{1} \lvert \beta, \gamma \rangle = \bigg( \sum_{y} a_{y}^{*} \langle y \lvert \bigg) H_{1} \bigg( \sum_{y} a_{x}^{*} \lvert x \rangle \bigg) = \sum_{y}\sum_{x} a_{y}^{*} a_{x} \langle y \lvert H_{1} \lvert x \rangle = \sum_{x} \lvert a_{x} \lvert ^{2} \langle x \lvert H_{1} \lvert x \rangle.
$$

because $H_{1} \lvert c \rangle$ is always a multiple of $\lvert x \rangle$ ($H_{1}$ is a diagonal matrix since it is a sum of diagonal matrices), because $\langle y | x \rangle = 0$ when $y\neq x$, and because $a_{y}^{*} a_{x} = | a_{x} |^{2}$.

From this, we obtain

$$
\langle \beta, \gamma \lvert H_{1} \lvert \beta, \gamma \rangle \approx \sum_{x} \frac{m_{x}}{M} \langle x | H_{1} | x \rangle,
$$

where $m_{x}$ is the number of times that $x$ was measured. The higher the value of $M$, the better this approximation will be.

## QUBO and HOBO
The Higher Order Binary Optimization (HOBO) or Polynomial Unconstrained Binary Optimization (PUBO), are the optimization problems that deals with minimizing a binary polynomial - of any degree - with no additional restrictions.

### Solving HOBO by tansforming it to QUBO problems
One way to solve HOBO problems is by transforming them into QUBO problems. For an example, you can substitute prodcuts $xy$ by a new binary variables $z$ as long as you introduce a penalty term $xy - 2xz - 2yz +3z$, which is $0$ if and only if $xy = z$. This kind of transformations can be access in D-Wave via `BinaryPolynomial` objects that you can reduce polynomials of degree $2$ with the `make_quadratic` function.

### Solving QUBO via QAOA
We can consider a binary polynomial of any degree and transform it using the techniques we have covered in [QUBO section](./QUBO.md). We will end up having a Hamiltonian that is a sum of tensor products of $Z_{j}$ matrices. Luckly, we can now deal with more than just one or two $Z_{j}$ matrices.

In the same fashion, If $\lvert x \rangle$ is a basis state, we have 

$$
e^{-iaZ_{j1}Z_{j2}\cdots Z_{jm}} \lvert x \rangle = e^{-ia} \lvert x \rangle
$$

if the sum of the bits of $x$ in positions $j_{1}, j_{2}, \cdots, j_{m}$ is even, and 

$$
e^{-iaZ_{j1}Z_{j2}\cdots Z_{jm}} \lvert x \rangle = e^{ia} \lvert x \rangle
$$
if the sum is odd.

This unitary action can be implemented by using consecutive CNOT gates with control qubits in $j_{1}, j_{1}, \cdots, j_{m-1}$ and targets in $j_{m}$ then a $R_Z$ gate with parameter $2a$ on qubit $j_{m}$ and again, consecutive CNOT gates with control qubits $j_{m-1}, j_{m-2}, \cdots, j_{1}$ and targets in $j_{m}. You can see the implementation in the image down below for case $e^{-iaZ_{0}Z_{1}Z_{3}}$.


We can also expand the same concept to help us evaluate the Hamiltonian $H_{1}$ that includes tensor products of $Z$ matrices. We have 

$$
\langle x | Z_{j_1}Z_{j_2} \cdots Z_{m} | x \rangle = 1
$$

if the sum of the bits of $x$ positions $j_{1}, j_{1}, \cdots, j_{m}$ is {==**even**==}. And

$$
\langle x | Z_{j_1}Z_{j_2} \cdots Z_{m} | x \rangle = -1
$$

if the sum of the bits of $x$ positions $j_{1}, j_{1}, \cdots, j_{m}$ is {==**odd**==}.

!!! example
    Please evaluate $\langle 100| H_{1} |100 \rangle$ with $H_{1} = Z_{0}Z_{1}Z_{2} + 3Z_{0}Z_{1}Z_{2} - Z_{1}Z_{2} + 2Z_{0}$    

    ---
    Answer: = $1 + 3 \times (-1) - 1 + 2 \times 2 = 0$



# Using QAOA with Qiskit

# Using QAOA with PennyLane

# Summary
In this chapter, you've learn the most popular quantum algorithms used to solve optimization problmes with gate-based quantum computers. You also learned that QAOA is a "discretization version" of a quantum annealing and it is implemented in hybrid way such that we run the classical optimization tool to update parameters such as $\beta$ and $\gamma$ and then use the powerful qunatum computer to prepare its energy state. And you surely know how to use these circuits to estimate expectation values in an efficient way.