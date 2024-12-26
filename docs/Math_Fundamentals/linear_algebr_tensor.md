# What is a Tensor Product $\otimes$ ?

The tensor product of two vectors is defined as:

$$
\begin{pmatrix}
a_1 \\
a_2 \\
\vdots \\
a_n
\end{pmatrix}
\otimes
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix}
=
\begin{pmatrix}
a_1
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix} \\
a_2
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix} \\
\vdots \\
a_n
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix}
\end{pmatrix}
=
\begin{pmatrix}
a_1 b_1 \\
a_1 b_2 \\
\vdots \\
a_1 b_m \\
a_2 b_1 \\
a_2 b_2 \\
\vdots \\
a_2 b_m \\
\vdots \\
a_n b_1 \\
a_n b_2 \\
\vdots \\
a_n b_m
\end{pmatrix}
$$

The tensor product is a mathematical operation that combines two quantum states into a single state describing a composite system. If you have two systems, $A$ and $B$, with states $\lvert 0 \rangle$ and $\lvert 0 \rangle$, their combined state is written as:
$$
\lvert a \rangle \otimes \lvert b \rangle
$$
For a single qubit has a state of dimension 2 ($\lvert 0\rangle$,$\lvert 1\rangle$), for two qubis together forma a state space of dimension $2 \times 2 = 4 \ (\lvert 00 \rangle, \lvert 01 \rangle, \lvert 10 \rangle, \lvert 11 \rangle)$. 

1. **Linearity**: 
$$
(a\lvert u \rangle + b\lvert v \rangle) \otimes \lvert w \rangle = a(\lvert u \rangle \otimes \lvert w \rangle)+ b(\lvert v \rangle \otimes \lvert w \rangle)  
$$

2. **Dimensionality**:
If $\lvert u \rangle$ is in a $d_{1}$-dimensional space and $\lvert v \rangle$ is in a $d_{2}$-dimensional space, the the combination state,
$$
\lvert d_{1} \rangle \otimes \lvert d_{2} \rangle 
$$
is in a $d_{1} \times d_{2}$-dimensional space.

3. **Associativity**
$$
(\lvert u \rangle \otimes \lvert v \rangle) \otimes \lvert w \rangle \approxeq \lvert u \rangle \otimes (\lvert v \rangle\otimes \lvert w \rangle)
$$

4. **Independence**:
The tensor prodict assumes that the two systems are **independent** and their joint state is simply the product of their individual states. For example: 
$$
\lvert 0 \rangle \otimes \lvert 1 \rangle = \lvert 01 \rangle
$$

Here are some properties of the tensor product operation:
5. **Basis States**
If $\lvert u \rangle$ and $\lvert v \rangle$ are basis states, their tensor product $\lvert 0 \rangle \otimes \lvert 0 \rangle$ forms a basis for the composite system. For example, the basis states $\lvert 0 \rangle$, $\lvert 1 \rangle$ for a sinagle qubi combine to form the two-qubi basis $\{ \lvert 00 \rangle, \lvert 01 \rangle, \lvert 10 \rangle, \lvert 11 \rangle \}$.

# What $\lvert 00 \rangle = \lvert 0 \rangle \otimes \lvert 0 \rangle$ implies
This representation emphasizes:

1. Individuality of Qubits: 
Each qubit has its own state space, but when combined, they form a composite system.
2. **Non-Entangled States**: The tensor product $\lvert 0 \rangle \otimes \lvert 0 \rangle$ imples a product state, meaning two qubits are independent and not entangled.


# Non-cloing in quantum mechanics:
The expression $\lvert \psi \rangle \otimes \lvert \psi \rangle$ represents the **tensor product** of a quantum state $\lvert \psi \rangle$ with itself. In the context of cloning, this means having **two identical copies** of the quantum state $\lvert \psi \rangle$.

---

### **Understanding Tensor Products**
In quantum mechanics:
1. The **tensor product** combines two quantum systems into a single composite system.
2. If two systems are in identical states $\lvert \psi \rangle$, the tensor product $\lvert \psi \rangle \otimes \lvert \psi \rangle$ describes a system where both subsystems are in the same state $\lvert \psi \rangle$.

---

### **Why $\lvert \psi \rangle \otimes \lvert \psi \rangle$ Represents Cloning**
1. **Single Qubit**:
    - A single qubit in the state $\lvert \psi \rangle$ has amplitudes $\alpha$ and $\beta$ for the basis states $\lvert 0 \rangle$ and $\lvert 1 \rangle$, respectively:
    $$
     \lvert \psi \rangle = \alpha \lvert 0 \rangle + \beta \lvert 1 \rangle
    $$

2. **Cloning**:
    - To clone $\lvert \psi \rangle$, we want to produce a second qubit in the exact same state, so the final system contains two qubits, both in state $\lvert \psi \rangle$.

3. **Mathematical Representation**:
    - If the first qubit is $\lvert \psi \rangle$ and the second (blank) qubit starts in $\lvert 0 \rangle$, then after cloning, the system would be described as:
    $$
     \lvert \psi \rangle \otimes \lvert \psi \rangle = (\alpha \lvert 0 \rangle + \beta \lvert 1 \rangle) \otimes (\alpha \lvert 0 \rangle + \beta \lvert 1 \rangle)
    $$

4. **Expansion**:
    - Expanding the tensor product:
    $$
     \lvert \psi \rangle \otimes \lvert \psi \rangle = \alpha^2 \lvert 00 \rangle + \alpha \beta \lvert 01 \rangle + \beta \alpha \lvert 10 \rangle + \beta^2 \lvert 11 \rangle
    $$
   - This describes a **two-qubit system** where both qubits are in the same state $\lvert \psi \rangle$.

---

### **Why Cloning is Different from Classical Duplication**
In classical systems:
1.  Information can be copied exactly without any loss or disturbance.

In quantum systems:
2.  A state like $\lvert \psi \rangle = \alpha \lvert 0 \rangle + \beta \lvert 1 \rangle$ cannot be perfectly duplicated into $\lvert \psi \rangle \otimes \lvert \psi \rangle$ because of the **no-cloning theorem**.

---

### **Summary**
1.  $\lvert \psi \rangle \otimes \lvert \psi \rangle$ represents a quantum system where two qubits are in the same state $\lvert \psi \rangle$, which corresponds to a cloned system.
2. Achieving this for an arbitrary $\lvert \psi \rangle$ is impossible because cloning would violate the linearity of quantum mechanics, as shown by the no-cloning theorem. 

Reference:

1. [Tensor product](https://en.wikipedia.org/wiki/Tensor_product)