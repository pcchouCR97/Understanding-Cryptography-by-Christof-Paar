# GAS: Grover Adpative Search

Grover's algorithm is used for searching elements that satisfy certain conditions. more formally, the algorithm assumes that we have a collection of elements indexed by strings of $n$ bits, and a Boolean function $f$ that takes those binary strings and returns "true" (or 1) if the element indexed by the string satisfies the condition and "false" (or 0) otherwise. For example, we have 8 different elements and the function return $f(x) = 1$ is and only if $010$ or $100$, and $f(x)= 0$ otherwise. 

It is worth to notice that we have no access to the inner working of $f$, which means, its a black box. The only thing we have access to is to call function $f$ on input and observe the outpurs. since we do not have any information about the indices of the elements that satisfy the condition, we cannot favour any position over any other. That's being said, if we are using a classical algorithm, and we are searching among $N$ elements and only one of them satisfies the condition we are interested in, we will need to call $f$ about $N/2$ times on average to find it. Since the one element could be anywhere, in the worst case, we have to call $N-1$ times to find it.

However, in Grover's algorithm, it is possible to find the hidden element with high probability by calling f around $\sqrt{N}$ times! Let's say, we need to call only 1000 times on a problem with 1,000,000 elements instead of 500,000 times with a classical algorithm. To understand how this works, we need to know what **quantum oracle** are and how they can be used.

## Quantum oracles

In the classical case, we had $n$ inputs and just one output. In contrast, in quantum world, $n$-inputs with one output cannot work since we need unitary operation to be **reversible**. As you must recall, every quantum gate has the same number of inputs and outputs!

The Quantum oracles, $O_{f}$, is an quantum gate (operation) of $f$ that take any input of $\lvert x\rangle \lvert y \rangle$, where $x$ is an $n$-bit string nd $y$ is a single bit, and the output of the $O_{f}$ gate will be 

$$
O_{f} = \lvert x \rangle \lvert y \oplus f(x) \rangle
$$

where $\oplus$ denotes addition modulo 2. 

It may be intuitive to think about the form of the output to be something like $\lvert x\rangle \lvert f(x)\rangle$. However, this could let the operation become irreversible since we would obtain the same output over the inputs $\lvert x\rangle \lvert1\rangle$. In fact, if we apply our choice of $O_{f}$, which is a reversiable operation , twice, we can obtain $\lvert x \rangle \lvert y \oplus f(x) \oplus f(x) \rangle$, which is equal to $\lvert x \rangle \lvert y \rangle$ because $f(x)\oplus f(x) = 0$ for addition modulo 2.

Usually, $O_{f}$ is said to be a quantum oracle for $f$ since we can consult it to get the value of $f$ on any input $x$ without having to worry about its internal workings. 

For any $f$, it is always possible to construct $O_{f}$ by using just NOT and multi-controlled NOT gates. For example, if $f$ is a Boolean function on 3-bit strings such that $f$ takes value 1 just on 101 an 011, then we can use the circuit show below.

<div style="text-align: center;">
    <img src="../../images_QOpt/oracle_of_the_boolean_function.png" alt="oracle_of_the_boolean_function" style="width: 600px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Oracle for the Boolean function \(f\) that takes value 1 on 101 and 011, and value 0 on the rest of the 3-bit strings
    </p>
</div>


In this example, we only used NOT gates before and after the multi-controlled gates to select those qubits that should be 0 in the input and to restore them to their original values.

!!! excercise
    Construct a circuit for $O_f$ where $f$ is a 4-bit Boolean function that takes value 1 on 0111, 1110, and 0101, and value 0 on any other input.

## Grover's circuits
Now, let's say that we want to apply Grover's algorithm to a Boolean function $f$ which receives binary strings of leangth $n$. Besides the quantum oracle $O_f$ we mentioned above, we still need two more elements to complete our circuit.

<div style="text-align: center;">
    <img src="../../images_QOpt/circuit_of_grover_algorithm.png" alt="circuit_of_grover_algorithm" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
       Figure. Circuit for Grover’s algorithm in the case in which \(f\) receives strings of length 3 as input. The oracle \(O_f\) and Grover’s diffusion operator are repeated, in that order, a number of times before the final measurements
    </p>
</div>

Figure. Circuit for Grover’s algorithm in the case in which $f$ receives strings of length 3 as input. The oracle $O_f$ and Grover’s diffusion operator are repeated, in that order, a number of times before the final measurements

1. **Phase kickback**:
    
    The first block is composed of one-qubit gates that are applied to the initial state $\lvert0 \cdots 0\rangle \lvert 0 \rangle$, where the first integer is of length $n$ and the second one if of length 1. Thus the state before applying the oracle is 

    $$
    \begin{array}{lll}
    H^{\otimes n+1} \lvert 0 \rangle ^{\otimes n}\lvert 1 \rangle 1 & = & \lvert + \rangle^{\otimes n} \lvert - \rangle \\
    & = & \frac{1}{\sqrt{2^{n}}} \ ((\lvert 0 \rangle + \lvert 1 \rangle) \cdots (\lvert 0 \rangle + \lvert 1 \rangle)) \ \lvert + \rangle \\
    & = & \frac{1}{\sqrt{2^{n}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle \lvert - \rangle
    \end{array}
    $$

    because we apply yhe first $X$ gate to $\lvert 0 \rangle$ to obtain $\lvert 1 \rangle$.

    The first register of this state is a **superposition** of all basis state $\lvert x \rangle$. This is exacatly what we will to evaluate $f$ "in superposition" with our application of the $O_{f}$ oracle.

    To apply the quantum oracle, we have

    $$
    \begin{array}{lll}
    O_f \bigg(\frac{1}{\sqrt{2^{n}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle \lvert - \rangle \bigg) & = & O_f \bigg(\frac{1}{\sqrt{2^{n+1}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle (\lvert 0 \rangle - \lvert 1 \rangle)\bigg) \\ 
    & = & O_f \frac{1}{\sqrt{2^{n+1}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle (\lvert 0 \rangle - \lvert 1 \rangle) \\
    & = & \frac{1}{\sqrt{2^{n+1}}}\sum_{x=0}^{2^{n}-1}O_{f}\lvert x \rangle (\lvert 0 \rangle - \lvert 1 \rangle) \\
    & = & \frac{1}{\sqrt{2^{n+1}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle (\lvert 0 \oplus f(x) \rangle - \lvert 1 \oplus f(x) \rangle) 
    \end{array}
    $$

    Let's focus on $\lvert 0 \oplus f(x) \rangle - \lvert 1 \oplus f(x) \rangle$.

    $$
    \begin{array}{llll}
    \lvert 0 \oplus f(x) \rangle - \lvert 1 \oplus f(x) \rangle & = & \lvert 0 \rangle - \lvert 1 \rangle, & \text{if} \ f(x) = 0 \\
    \lvert 0 \oplus f(x) \rangle - \lvert 1 \oplus f(x) \rangle & = & -(\lvert 0 \rangle - \lvert 1 \rangle), & \text{if} \ f(x) = 1
    \end{array}
    $$

    Thus, we can write it in a generalized form,

    $$
    \lvert 0 \oplus f(x) \rangle - \lvert 1 \oplus f(x) \rangle = (-1)^{f(x)}(\lvert 0 \rangle - \lvert 1 \rangle)
    $$

    As you may observe, there is information about the value $f(x)$ coded in the amplitude of the state now. Now let's put everything together!

    $$
    \begin{array}{lll}
    O_f \bigg(\frac{1}{\sqrt{2^{n}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle \lvert - \rangle \bigg) & = & \frac{1}{\sqrt{2^{n+1}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle (-1)^{f(x)}(\lvert 0 \rangle - \lvert 1 \rangle) \\
     & = & \frac{1}{\sqrt{2^{n}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle (-1)^{f(x)}\frac{1}{\sqrt{2}}(\lvert 0 \rangle - \lvert 1 \rangle)\\
     & = & \frac{1}{\sqrt{2^{n}}}\sum_{x=0}^{2^{n}-1}\lvert x \rangle (-1)^{f(x)}\lvert - \rangle
    \end{array}
    $$

    Notice how the application of $O_{f}$ has introduced a *relative phase* in some of the states $\lvert x \rangle$ of the superposition. This technique is called **phase kickback**, because we have only used the register in state $\lvert - \rangle$ to create the phase but it ends up affecting the whole state.

    As you can see, the phase that goes with the basis state $\lvert x \rangle$ depends on only $f(x)$ and it is 1 if $f(x) = 1$ and -1 if $f(x) = 1$. in this waym we say that we have **marked** those elements that we are interested in, that is, $f(x) = 1$.

    However, although we can seperate elements $x$ that satisfy $f(x) = 1$ from the list, we do not seems to be closer to find one of them. In fact, we will get the same probability before and after applying $O_{f}$. So we need a second block to help us identify these elements that match the criteria.

2. **Grover's diffusion operator**:

    The Grover's diffusion operator are used to increase the probability of measuring the marked states. The Grover's diffusion operator implements and opeartion called **inversion about the mean**. First, the average value $m$ of all the amplitudes of the states is computed. Second, every amplitude $a$ is replaced with $2m-a$. Then, the positive amplitudes will be a bit smaller, but the negative ones will be a little bit bigger. This is also called **amplitude amplification**. In short, the amplitudes of the elements that we are insterested in will be a little bit larger. However, this will still not big enough to guarantee a high probaility of measuring on of them. Thus, in gernal, we need to apply quantum oracle $O_{f}$ with Grover's difussion operator several times until the probability ofmeasuring one of the state we are looking for is high enough (close to 1), then we can perform a measurement. 

You may ask, so how many times should we apply $O_{f}$ with Grover's diffusiont operator? Let's see the following sebsection.

## Probability of finding a marked element
There's oen very important obervation about the $O_{f}$ and the Grover's diffusion is that the combination of these two acts just like a rotation in a two-dimensional space. Let's say if we have $n$-but strings and there is only one marked element $x_{1}$, it can be proved (skip here) that the state that we reach after $m$ times of combination of $O_{f}$ and the Grover's diffusion is 

$$
\cos(2m+1)\theta \lvert x_{0} \rangle + \sin(2m+1)\theta \lvert x_{1} \rangle,
$$

where 

$$
\lvert x_{0} \rangle = \sum_{x \in \{ 0, 1 \}^{n}, \ x \neq x_{1}}\sqrt{\frac{1}{2^{n}-1}}\lvert x \rangle
$$

and $\theta \in (0, \pi/2)$ is such that 

$$
\begin{array}{ll}
\cos \theta =  \sqrt{\frac{2^{n}-1}{2^{n}}}, & \sin \theta = \sqrt{\frac{1}{2^{n}}}.
\end{array}
$$

Notice that $\lvert x_{0} \rangle$ is just the **uniform superposition** of the state $\lvert x \rangle$ such that $f(x) = 0$. Therefore, as you know, we want to obtain the state where $\sin (2m +1) \theta$ is close to 1. That is,

$$
(2m + 1) \theta \approx \frac{\pi}{2}
$$

Solving for $m$

$$
m \approx \frac{\pi}{4\theta} - \frac{1}{2}.
$$

More, by plugging in $\sin \theta = \sqrt{1/2^{n}}$ and we asigning a big enough of $n$, we then have 

$$
\theta \approx \sqrt{\frac{1}{2^{n}}}
$$

After plugging it into $m$, 

$$
m = \frac{\pi}{4}\sqrt{2^{n}}.
$$

that is, the biggest integer that is less than or equal to $(\pi/4)\sqrt{2^{n}}$

Notice that there are exactly $2^{n}$ elements but only one of them satisfies the conditions we are interested in. for the classical algorithm, we need $2^{n}/2$ calls to $f$ on average to find $x$. However, Graver's algorithm only needs about $\sqrt{2^{n}}$.

There's one thing that worth ot notice is that the classical algorithm increases the probability of find the solution as we use $f$ more times. In contrast, if $m$ is not selected wisely, we can overshoot results and decrease the probability of getting it.

Don't forget, the probability of measuring $x_{1}$ is $(\sin (2m+1))^{2}$, which is a periodic function! After visiting values close to 1, the function goes back down to 0.

What if there is more than one marked element? If there are $k$ marked elements, we can repeat our previous reasoning and show that a good value for $m$ is 

$$
m = \frac{\pi}{4}\sqrt{\frac{2^{n}}{k}},
$$

where $k$ is samll compared to $2^{n}$.


## Finding minima with Grover's algorithm
As you can imagine, finding a minima is some action that find a value with a special property. Suppose we want to find a minimum of a function $g$ that is computed over binary strings of length $n$. We select one such string $x_0$ at random and we compute $g(x_0)$. Now we apply Grover's algorithm with an oracle that, on input $x$, returns 1 if $g(x) < g(x_{0})$ and 0 otherwise. If the element $x_1$ that we measure after applying Grover's search is lower than $g(x_0)$, we replace $x_0$ with it and repeat the process but now with an oracle that checks the condition $g(x) < g(x_1)$. If not, we keep using $x_0$. We repeat this process several times until find the lowest value.


## Quantum oracles for combinatorial optimization

After knowing the basic knowedge of the Dürr-Høyer algorithm, the next will be how to find a quantum oracle, which uses $x$ and $y$ to check if $g(x) < g(y)$, for us to use in order to find the minimum of function $g$.

We will start our jounery by considering the QUBO and HOBO cases with the coefficients of the polynomial are integer numbers and, then, we will extend our study to the most general when the coefficients are real numbers. However, before that, we need to understand one of the most important subroutines in all of quantumn computing: the **quantum Fourier transform**.

## The quantum Fourier transform
The quantum Fourier transform (QFT) is one of the most important and useful tool in quantum computing. It is an essential part of Shor's algorith for integer factorization and other algorithm such as HHL.

We will use the QFT to help us implement the arithmetical operations that we need to compute the values of the polynomial function of our QUBO and HOBO problems. The QFT on $m$ qubits is defined as the unitary transformation that takes the basis states $\lvert j \rangle$ to 

$$
\frac{1}{\sqrt{2^{m}}} \sum_{k=0}^{2^{m}-1} e^{\frac{2\pi ijk}{2^{m}}} \vert k \rangle,
$$

where $i$ is the imaginary unit.

The QFT can be implemented with a number of one- and two- qubit gates that is quadratic in $m$.

For instance, the circuit for the QFT on three qubits is shown below. As you can see, the rightmost gate, which acts on the top and bottom qibits, is the SWAP gate. Moreover, this QFT circuit uses the **phase gate**, denote by $P(\theta)$. This is a parameterized gate that depends on an angle $\theta$ and whose coordinate matrix is 

$$
\begin{pmatrix}
1 & 0\\
0 & e^{i\theta}
\end{pmatrix}
$$

<div style="text-align: center;">
    <img src="../../images_QOpt/quantum_Fourier_transform_on_3_qubits.png" alt="quantum_Fourier_transform_on_3_qubits" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure: Circuit for the quantum Fourier transform on 3 qubits.
    </p>
</div>

!!! note 
    The phase gate is very similar to the $R_z$ gate. 

The QFT acts by introducing phases of the form $e^{2\pi ijk/2^{m}}$ when it is applied on basis state $\lvert j \rangle$ and we insterested in recovering the values $j$ from those phases. Therefore, as you can imagine, we need to perform the **inverse quantum Fourier transform** ($\text{QFT}^\dagger$) 

$$
\frac{1}{\sqrt{2^{m}}}\sum_{k=0}^{2^{m}-1} e^{\frac{2\pi ijk}{2^m}}\lvert k \rangle
$$

to tha basis state $\lvert j \rangle$.

The circuit for the inverse QFT can be obtained from that of the QFT by reading the circuit backwards and using the inverse of each gate we find. 

1. The inverse of $P(\theta)$ is $P(-\theta)$
2. The inverse of H and SWAP are its own.

<div style="text-align: center;">
    <img src="../../images_QOpt/quantum_inverse_Fourier_transform_on_3_qubits.png" alt="quantum_inverse_Fourier_transform_on_3_qubits" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Circuit for the inverse quantum Fourier transform on 3 qubits.
    </p>
</div>

When designing a quantum oracle to minimize a function $g$, our goal will be to perform the computation in such a way that the $g(x)$ values appear as exponents in the amplitudes of our states so that we can later recover them by means of the inverse QFT.

## Encoding and adding integer numbers

When we deal with the integer numbers, use their **two's complement** representation is the most convenient way. We encode numbers from $-2^{m-1}$ to $2^{m-1}-1$ by using $m$-bit strings. positive numbers are represented in the usual way for binary numbers, but a negative number $x$ is represented by $2^{m}-x$. For example, if $m =$ 4, we represent 3 by 0011, and $-5$ by 1011. (binary representation of 11 = 16 - 5). This can keep positive numbers always start with 0 and negative numbers start with 1. 

Also this can be very helpful when we do addition involving both positive and negative numbers by simply do regular binary addition and discarding the last carry-out. For example, if we add 0011 (3) and 1101 (-5), we obtain 1110 (-2) ( the result is 11110, which is 5 bits. However, in a 4-bit system, only the lower 4 bits are kept). Same thing with adding 0110 (6) and 1100 (-4) you obtain 0010 (The result is 10010, but in a 4-bit system, only the lower 4 bits are retained).

!!! exercise 
    Using two’s complement with 5 qubits, represent 10 and −7 and perform their addition.


When computing $g(x)$ with an oracle, we are intersted in obtaining the state

$$
\frac{1}{\sqrt{2^{m}}} \sum_{k=0}^{2^{m}-1} e^{\frac{2\pi i g(x) k}{2^{m}}} \vert k \rangle,
$$

so that we can apply the inverse QFT to get $\lvert g(x) \rangle$. Notice that $g(x)$ is always a sum of products of integer values. 

The state 

$$
\frac{1}{\sqrt{2^{m}}}\sum_{k=0}^{2^{m}-1} e^{\frac{2 \pi i j k}{2^{m}}} \lvert k \rangle
$$

is called the **phase endoing** of $j$. Let's start with phase encoding of 0. To achieve this, we just apply the Hadamard gate to each and every qubit that we are using to represent the integer values. In this way, we will obtain the state

$$
\frac{1}{\sqrt{2^{m}}}\sum_{k=0}^{2^{m}-1}\lvert k \rangle = \frac{1}{\sqrt{2^{m}}}\sum_{k=0}^{2^{m}-1} e^{\frac{2 \pi i 0 k}{2^{m}}} \lvert k \rangle
$$

which is the phase encoding of $0$.

Suppose that we havea state that phase-encodes $j$ and we want to add $l$ to it. We first assum that $l$ is non-negative and deal with negative numbers later. To add $l$ in phase encoding, we just need to apply the gates shown below

<div style="text-align: center;">
    <img src="../../images_QOpt/phase_encoding_adding_l.png" alt="phase_encoding_adding_l" style="width: 200px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Circuit for adding \(l\) to a state in phase encoding when we have \(m\) qubits.
    </p>
</div>

When we apply those gates to a basis state $k$, we obtain $e^{\frac{2 \pi i l k}{2^{m}}} \lvert k \rangle$. Thus, by linearity, when we apply the circuit to the phase encoding of $j$, we get

$$
\frac{1}{\sqrt{2^{m}}}\sum_{k=0}^{2^{m}-1} e^{\frac{2 \pi i j k}{2^{m}}}e^{\frac{2 \pi i l k}{2^{m}}} \lvert k \rangle = \frac{1}{\sqrt{2^{m}}}\sum_{k=0}^{2^{m}-1} e^{\frac{2 \pi i (j+l) k}{2^{m}}} \lvert k \rangle
$$

which is the phase encoding of $j+l$. 

For negative numbers, we can still apply figure above without additional adjustment. The key observation is that, for any integer $0\leq h \leq m-1$, it holds that 

$$
e^{\frac{\pi i(2^{m}+l)}{2^h}} = e^{\frac{\pi il}{2^h}}e^{\frac{\pi i2^{m}}{2^h}} = e^{\frac{\pi il}{2^h}}e^{\pi i 2^{m-h}} = e^{\frac{\pi il}{2^h}}
$$

since $m-h > 0$, making $2^{m-h}$ even and implying $e^{\pi i 2^{m-h}} = 1$. This means that if we plug in $l$ or $2^{m}+l$ in the gates of [figure](../images_QOpt/phase_encoding_adding_l.png) above, we obtain exactly the same circuit. Thus, we can work with the two's complement representation of $l$.

<div style="text-align: center;">
    <img src="../../images_QOpt/circuit_for_preparing_the_phase_rep_of_0.png" alt="circuit_for_preparing_the_phase_rep_of_0" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure presents a circuit that prepares the phase representation of 0, adds 3 to it and then subtracts 5. Of course, we can simplified the circuit such as \(P(-5\frac{\pi}{2})P(3\frac{\pi}{2}) = P(\pi)\) and \(P(3\pi) = P(\pi)\). Since the phase \(\phi\) is a modulo \(2\pi\) quantity becasue \(e^{i(\pi +2\pi)}\). That is, adding \(2\pi\) to the phase does not change the effect of the gate.
    </p>
</div>

$$
3 \pi \ \text{mod} \ 2\pi = \pi.
$$

## Computing the whole polynomial

Let's show the example of circuit that copmutes $3x_{0}x_{1} - 2x_{1}x_{2} + 1$. The first column of gates prepares the phase encoding of $0$. The second one adds the independent term of the polynomial. The next one adds 3, but only if $x_{0}=x_{1}=1$ (that is why all the gates are controlled by the $\lvert x \rangle$ and $\lvert x_{1} \rangle$ qubits). Similarly, the last column substracts 2, but only when$x_{1} = x_{2} = 1$. As you should remember, controlled gates execute a quantum operation (such as adding a phase) only when the control qubits are in a specific state, typically $\lvert 1 \rangle$. Or, we can say that $3x_{0}x_{1}$ is nonzero only when $x_{0}=x_{1}=1$. These condition ensure that the polynomial terms contribute to the computation only when they are logically valid. Controlled gates enforce these conditions by activating only when the control qubits match the required state.

<div style="text-align: center;">
    <img src="../../images_QOpt/circuit_computing_example_in_pahse_encoding.png" alt="circuit_computing_example_in_pahse_encoding" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Circuit for computing \(3x_{0}x_{1} - 2x_{1}x_{2} + 1\) in phase encoding. 
    </p>
</div>

From the [figure](../images_QOpt/circuit_computing_example_in_pahse_encoding.png), we have adopted the usual convention of setting all the one-qubit gates that are controlled by the same qubits in ta single column. This technique is also called as a **single multi-qubit gate**. Also, you may notice that these gates are multi-controlled, but you can always decompose them into a combination of one and two-qubit gates with Toffoli gates.

There are two methods that we can use to deal with the real numbers in phase encoding.

1. **Approximation Using Fractions:**
    - Represent real coefficients as fractions (e.g., $0.25 = 25/100$, $-1.17 = -117/100$).
    - Multiply the polynomial by the denominator ($100$) to convert coefficients to integers while preserving structure.
    - Example: $0.25x_0 - 1.17x_1 \to 25x_0 - 117x_1$.

2. **Direct Encoding:**
    - Encode real coefficients directly by creating a **superposition of approximations** with the largest amplitude for the best approximation.
    - Example: Coefficient $0.73$ encoded as a superposition of $0.7, 0.73, 0.75$.

## Constructing the oracle

This diagram represents an **oracle** to determine whether $g(x) < g(y)$, utilizing quantum operations. Here’s a breakdown of its components and functionality:

<div style="text-align: center;">
    <img src="../../images_QOpt/oracle_to_determine_g.png" alt="oracle_to_determine_g" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure of a oracle to determine \( g \).
    </p>
</div>

### Key elements in the Circuit:
1. **Input Qubits:**
    - $\lvert x\rangle$ and $\lvert y\rangle$:https://www.youtube.com/ Encodes the inputs $x$ and $y$ as $n$-qubit registers.
    - $\lvert 0^m\rangle$: A work register initialized to $\lvert 0\rangle$ with $m$ qubits, used for intermediate computations (store all the intermediate results).

2. **Phase Encoding of Differences:**
    - The circuit calculates the differences $g(x) - g(y)$ and $g(y) - g(x)$ using phase encoding. These are crucial for comparing the functions' values.

3. **Quantum Fourier Transform (QFT):**
    - A **Quantum Fourier Transform (QFT)** and its inverse ($\text{QFT}^\dagger$) are applied to the work register ($\lvert 0^m\rangle$) to prepare and analyze the relative phases. These operations help determine the magnitude of the difference between $g(x)$ and $g(y)$.
    
4. **Controlled Comparison:**
    - A controlled operation (centered between the $\text{QFT}^\dagger$ and $\text{QFT}$) flips a target qubit if $g(x) < g(y)$. This comparison is achieved through interference effects from the encoded differences.

5. **Uncomputation:**
    - After the comparison, the circuit reverses the transformations applied earlier (like QFT and the $g(x) - g(y)$ and $g(y) - g(x)$ encodings), returning all ancillary qubits to their initial state.
    - We need to set the $m$ auxiliary qubits back to $\lvert 0 \rangle$ to disentangle them from the rest of the qubits in the circuit. If they remain entangled, they may prevent the rest of the circuit from working correctly.

6. **Output:**
    - The bottom qubit will store the result of checking whether $g(x) < g(y)$.
    - The target qubit $\lvert z\rangle$ will be flipped ($\lvert 1\rangle$) if $g(x) < g(y)$, otherwise, it remains $\lvert 0\rangle$.
    - We have already computed the result that we needed: $z$ will be 1 if $g(x) < g(y)$ and it will be 0 otherwise.

More, we can also create oracles to check whether polynomial constraints are met or not, like $3x_{0} - 2x_{0}x_{1} < 3$. This allows constraints to be incorporated into the optimization process without transforming the problem entirely into a QUBO form. This approach can sometimes be more convenient than using penalty terms for constraints.

# Using GAS with Qiskit