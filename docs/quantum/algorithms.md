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


















---


Ref: quantum-computation-and-quantum-information-nielsen-chuang