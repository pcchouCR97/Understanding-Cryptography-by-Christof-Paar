# Quantum Algorithm


Q:  As pointed out earlier, the reason quantum circuits cannot be used to directly simulate classical circuits is because unitary quantum logic gates are inherently reversible, whereas many classical logic gates such as the NAND gate are inherently irreversible.

## Toffoli gate
Two of the bits are control bits that are unaffected by the action of the Toffoli gate. The third bit is a target bit that is flipped if both control bits are set to 1, and otherwise is left alone.

Toffoli gate twice to a set of bits has the effect (a, b, c) → (a, b, c ⊕ ab) →
(a, b, c), and thus the Toffoli gate is a reversible gate, since it has an inverse – itself.

The Toffoli gate can be used to simulate `NAND` gates, as shown in Figure 1.15, and
can also be used to do `FANOUT`, as shown in Figure 1.16. With these two operations it
becomes possible to simulate all other elements in a classical circuit, and thus an arbitrary
classical circuit can be simulated by an equivalent reversible circuit.


{==what is a Unitary matrix??==}

## Quantum Parallelism
``` 
    Quantum parallelism is a fundamental feature of many quantum algorithms.
```
Quantum parallelism allows quantum computers to evaluate a function f(x) for many different values of x simultaneously.

Quantum parallel evaluation of a function with an n bit input x and 1 bit output, f(x), can thus be performed in the following manner.

qubit quantum computer which starts in the state $\lvert x,y \rangle$, with and proper squence of logic gates to transfrom to state $\lvert x, y\oplus f(x) \rangle$, where $\oplus$ indicates addition modulo 2. The first register is called `data` register, and the second register is the `target` register.

**Quantum circuit for evaluating $f(0)$ and $f(1)$ simultaneously**

![quantum_circuit_uf](../quantum/images/quantum_circuit_Uf.png)

Consider the figure, which applies $U_f$ to and input not in the computational basis. The data register is prepared in the superposition $\frac{\lvert 0 \rangle + \lvert 1 \rangle}{\sqrt{2}}$, which can be created with Hadamard gate acting on $\lvert 0 \rangle$. Then we apply $U_f$, resulting in the state:
$$
\frac{\lvert 0, f(0)\rangle + \lvert 1, f(1)\rangle}{\sqrt{2}}
$$
The different terms contain information about both $f(0)$ and $f(1)$; it is almost as if we have evaluated $f(x)$ for two values of x simultaneously, a feature known as ‘quantum parallelism’.

---
Proof: $\frac{\lvert 0, f(0)\rangle + \lvert 1, f(1)\rangle}{\sqrt{2}}$

The operation $U_f$ is a unitary operation that maps:
$$
\lvert x\rangle \lvert y\rangle \rightarrow \lvert x\rangle \lvert y\oplus f(x)\rangle 
$$
where $\oplus$ is XOR (addition modulo 2)

1. $x$ is the value of the data register.
2. $y$ is the value of the ancilla qubit.
3. $f(x)$ is the output of the function being evaluated.

**We know that the inital state can be written as**:
$$
\big( \frac{\lvert 0\rangle + \lvert 1\rangle}{\sqrt{2}}\big)\lvert 0 \rangle = \frac{\lvert 0\rangle\lvert 0\rangle + \lvert 1\rangle\lvert 0\rangle}{\sqrt{2}}
$$

**Apply $U_f$ to each term from the initial state**:

1. For the first term $\lvert 0\rangle\lvert 0\rangle$:
$$
\lvert 0\rangle\lvert 0\rangle \rightarrow \lvert 0\rangle\lvert 0\oplus f(0)\rangle = \lvert 0\rangle \lvert f(0) \rangle
$$
2. For the second term $\lvert 1\rangle\lvert 0\rangle$:
$$
\lvert 1\rangle\lvert 0\rangle \rightarrow \lvert 1\rangle\lvert 0\oplus f(1)\rangle = \lvert 1\rangle \lvert f(1) \rangle
$$
Thus, the new state:
$$
\frac{\lvert 0, f(0)\rangle + \lvert 1, f(1)\rangle}{\sqrt{2}}
$$

---

Unlike classical parallelism, where multiple circuits each built to compute f(x) are executed simultaneously, here a single f(x) circuit is employed to evaluate the function for multiple values of x simultaneously

Prepare the n + 1 qubit state $\lvert 0 \rangle^{\otimes n} \lvert 0 \rangle$,
then apply the Hadamard transform to the first n qubits, followed by the quantum circuit
implementing $U_f$ . This produces the state
$$
\frac{1}{\sqrt{2^{n}}}\sum_{x} \lvert x \rangle \lvert f(x) \rangle
$$

In some sense, quantum parallelism enables all possible values of the function f to be
evaluated simultaneously, even though we apparently only evaluated f once. However,
this parallelism is not immediately useful.

in the general case, measurement of the state $\sum_{x} \lvert x \rangle \lvert f(x) \rangle$ would give only $f(x)$ for a single value of $x$. Of course, a classical
computer can do this easily!

Quantum computation it requires the ability to extract information about more
than one value of $f(x)$ from superposition states like $\sum_{x} \lvert x. f(x)\rangle$.

## Deutsch's algorithm
Let's see how quantum circuits can outperform classical ones by implementing *Deutsch’s algorithm*. Deutsch’s algorithm combines quantum parallelism with a property of quantum mechanics known as interference

Build on the previous case, we apply Hadamard gate to the state $\lvert 1 \rangle$. Thus, we have $\frac{\lvert 0 \rangle + \lvert 1 \rangle}{\sqrt{2}}$ for the first qubit and $\frac{\lvert 0 \rangle - \lvert 1 \rangle}{\sqrt{2}}$ for the second qubit. 
The input state:
$$
\lvert \psi_0 \rangle = \lvert 01 \rangle
$$
Therefore we have the state:
$$
\lvert \psi_1 \rangle = \bigg[\frac{\lvert 0 \rangle + \lvert 1 \rangle}{\sqrt{2}}\bigg]\bigg[\frac{\lvert 0 \rangle - \lvert 1 \rangle}{\sqrt{2}}\bigg]
$$
Then, we apply $U_f$ on the state $\lvert x\rangle(\frac{\lvert 0 \rangle - \lvert 1 \rangle}{\sqrt{2}})$, we will have:

$$
\lvert\psi_2\rangle =
\begin{cases}
\pm \frac{\lvert0\rangle + \lvert1\rangle}{\sqrt{2}} \frac{\lvert0\rangle - \lvert1\rangle}{\sqrt{2}} & \text{if } f(0) = f(1), \\
\pm \frac{\lvert0\rangle - \lvert1\rangle}{\sqrt{2}} \frac{\lvert0\rangle - \lvert1\rangle}{\sqrt{2}} & \text{if } f(0) \neq f(1).
\end{cases}
$$

Then we apply one Hadamard gate on the first qubit thus gives us:

$$
\lvert\psi_2\rangle =
\begin{cases}
\pm \lvert0\rangle \frac{\lvert0\rangle - \lvert1\rangle}{\sqrt{2}} & \text{if} f(0) = f(1), \\
\pm \lvert1\rangle \frac{\lvert0\rangle - \lvert1\rangle}{\sqrt{2}} & \text{if} f(0) \neq f(1).
\end{cases}
$$

Realizing that $f(0) \oplus f(1)$ is $0$ if $f(0) = f(1)$ and $1$ otherwise, we can rewrite this result as:

$$
\lvert \psi_{3} \rangle = \pm\lvert f(0)\oplus f(1)\rangle \bigg[ \frac{\lvert0 \rangle - \lvert 1 \rangle}{\sqrt{2}} \bigg]
$$

By measurign the first qubit we may determin $f(0) \oplus f(1)$. This is very interesting: the quantum circuit has given us the ability to determine a *global property* of $f(x)$, namely $f(0)\oplus f(1)$, using only *one* evaluation of $f(x)$. {==This is faster than a classical apparatus, which would require at least two evaluations==}.

This example highlights the difference between quantum parallelism and classical
randomized algorithms.

The essence of the design of many quantum algorithms is that a clever choice of function and final transformation allows efficient determination of useful global information about the function – information which cannot be attained quickly on a classical computer.

## The Deutsch-Jozsa Algorithm

Deutsch Algorithm is a special case for Deutsch-Jozsa Algorithm, at which $n=1$ for $2^{n}-1$.
Deutsch’s algorithm is a simple case of a more general quantum algorithm, which we shall refer to as the Deutsch–Jozsa algorithm. 

## Algorithm Summary
TheDeutsch–Jozsa algorithm suggests that quantum computers may be capable of solving
some computational problems much more efficiently than classical computers. Unfortunately,
the problem it solves is of little practical interest. Are there more interesting problems whose solution may be obtained more efficiently using quantum algorithms? What are the principles underlying such algorithms? What are the ultimate limits of a quantum computer’s computational power?

Broadly speaking, there are three classes of quantum algorithms which provide an
advantage over known classical algorithms:

1. The class of algorithms based upon quantum versions of the Fourier transform
2. Quantum search algorithm
3. Quantum simulation



### Quantum Algorithm based upon the Fourier transform

### Quantum search algorithm

### Quantum simulation

### The power of quantum computation
---
Ref: quantum-computation-and-quantum-information-nielsen-chuang