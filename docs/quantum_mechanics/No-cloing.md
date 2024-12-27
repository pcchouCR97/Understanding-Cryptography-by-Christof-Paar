The **no-cloning theorem** is a fundamental principle in quantum mechanics that states it is {==impossible to create an exact copy of an arbitrary unknown quantum state==}. This is a cornerstone of the security in quantum key distribution protocols like [BB84](../qcryptography/BB84.md).

---
Another interesring property of quantum systems is the no-cloning theorm. Check [No-cloning](../quantum_mechanics/No-cloing.md) for proof. In short, for an example, if we would like to have a {==two-qubit quantum gate $U$ that will be able to copy the first qubit into the second==}. we would need,

$$
U(\lvert\psi\rangle \otimes \lvert0\rangle) = \lvert\psi\rangle \otimes \lvert\psi\rangle
$$

from above, we know that $U\lvert00\rangle = \lvert00\rangle$ and $U\lvert10\rangle = \lvert11\rangle$. Therefore,

$$
U\bigg(\frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert10\rangle)\bigg) = \frac{1}{\sqrt{2}}(U\lvert00\rangle + U\lvert10\rangle) = \frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert11\rangle).
$$

Then, from previous example, we also know that $\frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert10\rangle$ can be written in a product form,

$$
\frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert10\rangle = \frac{1}{\sqrt{2}}\bigg(\lvert0\rangle + \lvert1\rangle\bigg)\lvert0\rangle 
$$

Now, let's apply gate $U$, we should have,

$$
U\frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert10\rangle = U\bigg(\frac{1}{\sqrt{2}}\bigg(\lvert0\rangle + \lvert1\rangle\bigg)\lvert0\rangle\bigg) =  \frac{\lvert0\rangle + \lvert1\rangle}{\sqrt{2}}\frac{\lvert0\rangle + \lvert1\rangle}{\sqrt{2}}
$$

which is a product state, and we also know have $U\bigg(\frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert10\rangle)\bigg) = \frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert11\rangle)$,

$$
\frac{\lvert0\rangle + \lvert1\rangle}{\sqrt{2}}\frac{\lvert0\rangle + \lvert1\rangle}{\sqrt{2}}\neq \frac{1}{\sqrt{2}}(\lvert00\rangle + \lvert11\rangle)
$$

therefore, no such gate $U$ exist.
---

### **What is the No-Cloning Theorem?**
The no-cloning theorem ensures that:

Given a qubit in an unknown quantum state  $\lvert\psi\rangle$ = $\alpha\lvert0\rangle$ + $\beta\lvert1\rangle$, {==it is impossible to produce another qubit in the exact same state  $\lvert\psi\rangle$ without disturbing the original==}.

### Why?
The proof is rooted in the linearity of quantum mechanics:

1.  To clone a quantum state, we would need a **cloning operation** U that works like this:
$$
U(\lvert\psi\rangle \otimes \lvert0\rangle) = \lvert\psi\rangle \otimes \lvert\psi\rangle
$$
where  $\lvert0\rangle$  is a blank state (e.g., an auxiliary system). See [Non-cloing](../Math_Fundamentals/linear_algebr_tensor.md) for tensor.

2.  For two arbitrary quantum states $\lvert\psi\rangle$ and $\lvert\phi\rangle$, linearity requires:
$$
U(\lvert\psi\rangle \otimes \lvert0\rangle) = \lvert\psi\rangle \otimes \lvert\psi\rangle
$$
$$
U(\lvert\phi\rangle \otimes \lvert0\rangle) = \lvert\phi\rangle \otimes \lvert\phi\rangle
$$
3.  If $\lvert\psi\rangle \neq \lvert\phi\rangle$, linearity leads to a contradiction because the superposition of the states cannot preserve the cloning operation:
$$
U(a\lvert\psi\rangle + b\lvert\phi\rangle) \neq a\lvert\psi\rangle \otimes \lvert\psi\rangle + b\lvert\phi\rangle \otimes \lvert\phi\rangle
$$
Thus, no such universal cloning operation $U$ can exist.
---

### **1. The No-Cloning Theorem**
The **no-cloning theorem** states that it is impossible to create an exact copy of an arbitrary quantum state. Given a quantum state :
$$
\lvert \psi \rangle = \alpha \lvert 0 \rangle + \beta \lvert 1 \rangle,
$$
there is no operation that can produce:
$$
U(\lvert \psi \rangle \otimes \lvert 0 \rangle) = \lvert \psi \rangle \otimes \lvert \psi \rangle,
$$
where $ \lvert 0 \rangle $ is an auxiliary "blank" state.

---

### **2. Why Cloning is Impossible**
The proof relies on the **linearity** of quantum mechanics. 

Suppose we have two distinct quantum states $\lvert \psi \rangle$ and $\lvert \phi \rangle$. For a universal cloning operation $U$, we require:
$$
U(\lvert \psi \rangle \otimes \lvert 0 \rangle) = \lvert \psi \rangle \otimes \lvert \psi \rangle,
$$
$$
U(\lvert \phi \rangle \otimes \lvert 0 \rangle) = \lvert \phi \rangle \otimes \lvert \phi \rangle.
$$

Now, consider a superposition of these states:
$$
\lvert \chi \rangle = a \lvert \psi \rangle + b \lvert \phi \rangle,
$$
where $a$ and $b$ are complex coefficients.

By the **linearity** of $U$, applying it to $ \lvert \chi \rangle $ gives:
$$
U(\lvert \chi \rangle \otimes \lvert 0 \rangle) = U((a \lvert \psi \rangle + b \lvert \phi \rangle) \otimes \lvert 0 \rangle).
$$
Expanding this:
$$
U(\lvert \chi \rangle \otimes \lvert 0 \rangle) = a U(\lvert \psi \rangle \otimes \lvert 0 \rangle) + b U(\lvert \phi \rangle \otimes \lvert 0 \rangle).
$$

Substituting the cloning results:
$$
U(\lvert \chi \rangle \otimes \lvert 0 \rangle) = a (\lvert \psi \rangle \otimes \lvert \psi \rangle) + b (\lvert \phi \rangle \otimes \lvert \phi \rangle).
$$

However, the **desired cloning result** would be:
$$
\lvert \chi \rangle \otimes \lvert \chi \rangle = (a \lvert \psi \rangle + b \lvert \phi \rangle) \otimes (a \lvert \psi \rangle + b \lvert \phi \rangle).
$$

Expanding this:
$$
\lvert \chi \rangle \otimes \lvert \chi \rangle = a^2 (\lvert \psi \rangle \otimes \lvert \psi \rangle) + ab (\lvert \psi \rangle \otimes \lvert \phi \rangle) + ab (\lvert \phi \rangle \otimes \lvert \psi \rangle) + b^2 (\lvert \phi \rangle \otimes \lvert \phi \rangle).
$$

---

### **3. The Contradiction**
The two results are **not equal**:
1. The linearity of $U$ only gives:
$$
a (\lvert \psi \rangle \otimes \lvert \psi \rangle) + b (\lvert \phi \rangle \otimes \lvert \phi \rangle).
$$
This result **lacks the cross terms** $ab(\lvert \psi \rangle \otimes \lvert \phi \rangle)$ and $ab (\lvert \phi \rangle \otimes \lvert \psi \rangle)$.

Thus, the desired cloning result requires the cross terms because it represents the tensor product of the superposition with itself.

This mismatch proves that a universal cloning operation $U$ cannot exist.

---

### **4. Key Insights**
- The **no-cloning theorem** arises because quantum mechanics is linear, and linear operations cannot replicate the behavior of classical copying.
- Quantum states cannot be duplicated without violating the mathematical structure of quantum mechanics.

This makes cloning of arbitrary quantum states impossible, providing a foundation for the security of quantum cryptography protocols like BB84. Let me know if you'd like further clarification!