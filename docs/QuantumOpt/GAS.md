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

![oracle_of_the_boolean_function](../QuantumOpt/images/oracle_of_the_boolean_function.png)

Figure. Oracle for the Boolean function $f$ that takes value 1 on 101 and 011, and value 0 on the rest of the 3-bit strings

In this example, we only used NOT gates before and after the multi-controlled gates to select those qubits that should be 0 in the input and to restore them to their original values.

!!! excercise
    Construct a circuit for $O_f$ where $f$ is a 4-bit Boolean function that takes value 1 on 0111, 1110, and 0101, and value 0 on any other input.

## Grover's circuits
Now, let's say that we want to apply Grover's algorithm to a Boolean function $f$ which receives binary strings of leangth $n$. Besides the quantum oracle $O_f$ we mentioned above, we still need two more elements to complete our circuit.

![circuit_of_grover_algorithm](../QuantumOpt/images/circuit_of_grover_algorithm.png)

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
As you can imagine, finding a minima is some action that find a value with a special property. Suppose we want to find a minimum of a function $g$ that is computed over binary strings of length $n$. We select one such string $x_0$ at random and we compute $g(x_0)$. Now we apply Grover's algorithm with an oracle that, on input $x$, returns 1 if $g(x) < g(x_{0})$ and 0 otherwise.


# Quantum oracles for combinatorial optimization

## The quantum Fourier transform

## Encoding and adding integer numbers

## Computing the whole polynomial

## Constructing the oracle

# Using GAS with Qiskit