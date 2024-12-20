# What is a Tensor Product $\otimes$ ?

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



Reference:

1. [Tensor product](https://en.wikipedia.org/wiki/Tensor_product)