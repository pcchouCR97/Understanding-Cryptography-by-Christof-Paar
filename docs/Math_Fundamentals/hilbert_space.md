## Hilbert space

-   Hilbert space, a complex vector space with an inner product. In Quantum     mechanics, the term "Hilber space" is often reserved for an intinite-dimentional inner product space having the property that it is copmlete or closed. Here we will use it in finite-dimensional spaces.

-   We use $Dirac$ notation $| v \rangle$ called $ket$. where $v$ is a symbol of a vector. You can analogous to **$v$** or $\vec{v}$. 

-   The inner product of the vector $| v \rangle$ with $| w \rangle$ is written as $\langle v | w \rangle$. Think of this as a $\vec{v}\cdot\vec{w}$ in a familiar vector form.

## Qubit

-   Let's start with the 2-dimensional case. Every vector in the 2-dimensional Hilbert can be written as a linear combination of two vectors which form a basis for the space. The computational basis for quantum information are denoted as $|0\rangle$ and $|0\rangle$, and it is assumed that of them are *normalized* and that they are mutually *orthogonal*

    $$
    \langle 0 | 0 \rangle = 1 = \langle w | w \rangle, \ \langle 0 | 1 \rangle = 1 = \langle 1 | 0 \rangle.
    $$

-   In quantum mechanics a 2-dimensional complex Hilbert space $H$ is used for describing the angular momentum or "spin of a spin-half particle (electron, proton, neutron, silver atom), which then provides a physical representation of a qubit. 
-   A polarization of a photon (particle of light) is also described by $d=2$ (2-dimensional).
-   A state vector $|\psi\rangle$ says something about one component of the spin of the spin half particle. The usual convention is 

    $$
    |0\rangle = |z^{+}\rangle \leftrightarrow S_{z} = + 1/2, \ |1\rangle = |z^{-}\rangle \leftrightarrow S_{z} = - 1/2,
    $$
    where $S_{z}$ is the $z$ component of angular momentum is measured in units of $\hbar$.

-   The general rule is that $w$ is a direction in space correspnding to the angle $\theta$ and $\phi$ in polar coordinates,

    $$
    |0\rangle + e^{i\phi} \tan(\phi/2) |1\rangle \leftrightarrow S_{w} = +1/2, \ |0\rangle - e^{i\phi} \cot(\phi/2) |1\rangle \leftrightarrow S_{w} = -1/2, \
    $$

    but see the comments below on normalization.

## General $d$

-   A collection of linearly independent vectors ${B_{j}}$ form a *basis* of $H$ provided any $| \psi \rangle$ in $H$ can be written as a linear combination 

    $$
    | \psi \rangle = \sum_{j} c_{j} | B_{j} \rangle,
    $$

    where $c_{j}$ is a copmlex number.

-   A [*orthonormal basis*](../Math_Fundamentals/matrices.md#orthonormal-basis) $\{|j\rangle \}$ for $j = 1, 2, \cdots, d$ with the property that 

    $$
    \langle j | k \rangle = \delta_{jk}.
    $$

    The inner product of two basis vectors is 0 for $j \neq k$ if they are *orthogonal*, and 1 if they are *normalized*. $\delta_{jk}$ is also called as a [Kronecker delta](https://en.wikipedia.org/wiki/Kronecker_delta).

-   If we write a vector as a combination of a [*orthonormal basis*](../Math_Fundamentals/matrices.md#orthonormal-basis) $\{|j\rangle \}$ for $j = 1, 2, \cdots, d$

    $$
    |v\rangle = \sum_{j} v_{j}|j\rangle, \ |w\rangle = \sum_{j} w_{j}|j\rangle
    $$

    where the coefficients $v_{j}$ and $w_{j}$ are given by

    $$
    v_{j} = \langle j | v \rangle, \ w_{j} = \langle j | w \rangle,
    $$

    the inner product $\langle v | w \rangle$ can be written as 

    $$
    (|v\rangle^{\dagger})|w\rangle = \langle v | w \rangle = \sum_{j}v_{j}^{*}w_{j},
    $$

    which can be thought of as the product of a "bra" vector

    $$
    \langle v | = (|v\rangle)^{\dagger} = \sum_{j}^{*}w_{j}\langle j |
    $$

    with the "ket vector $| w \rangle$.

-   It's convenient to think of $| w \rangle$ in a column vector 

    $$
    | w \rangle 
    \begin{pmatrix}
    w_{1} \\
    w_{2} \\
    \cdots \\
    w_{d} 
    \end{pmatrix}
    ,
    $$

    and $\langle v |$ by a row vector 

    $$
    \langle v | = (v_{1}^{*},v_{2}^{*},\cdots, v_{d}^{*}).
    $$

    where d is a diemsnion.

## Kets as physical properties

-   In quantum mechanics, two vectors $|\psi\rangle$ and $c|\psi$, where $c$ is any *nonzero* complex number have *exactly the same* physical significance. For this reason, it is sometimes helpful to say that the physical state corresponds not to a particular vector in the Hilbert space, but the *$ray$*, or one-dimensional subspace, defined by the collection of all the complex multiples of a particular vector.
    -   One can always choose $c$ in such way that the $| \psi \rangle$ corresponding to a particular physical situation is *normalized*, $\langle \psi | \psi \rangle = 1$ or $||\psi|| = 1$, where the *norm* $||\psi||$ of a state $|\psi\rangle$ is the positive square root of 

    $$
    || \psi||^{2} = \langle \psi | \psi \rangle,
    $$

    and is 0 if and only if $\lvert \psi \rangle$ is the unique zero vector, which will be written as 0 instead of $| \psi \rangle$.
    
    -   Normalized vectors can always be multiplied by a *phase factor*, a complex number of the form $e^{i\phi}$ where $\phi$ is real, without changing the normalization or the physical interpretation.

-   The state of a single qubit is always a linear combination of the basis vectors $|0\rangle$ and $|1\rangle$,

    $$
    |\psi\rangle = \alpha |0\rangle + \beta |1\rangle,
    $$

    where $\alpha$ and $\beta$ are complex numbers.

-   Two nonzero vectors $|\psi \rangle$ and $|\phi\rangle$ which are *orthogonal* if $\langle \phi | \psi \rangle = 0$, represent *distinct* physical properties:
    -   If one corresponds to a property, such as $S_{z} = + 1/2$, which is a correct description of a physical system at a particular time, then the other corresponds to a physical property which is not true for this system at this time. We call this *mutaully exclusive*.

-   There are cases where $|\psi \rangle$ is neither a multiple of $| \phi \rangle$, nor is it orthogonal to $|\phi\rangle$. For example

    $$
    |0\rangle = |z^{+}\rangle \leftrightarrow S_{z} = +1/2 \ \text{and} \ \sqrt{2}|x^{+}\rangle = |0\rangle + |1\rangle \leftrightarrow S_{x} = +1/2
    $$

     These represent neither the same physical situation, nor do they represent distinct physical situations.

-   A quantum system cannot simultaneously process two incompatible properties. For example, a spin-half particle cannot have both $S_{x} = 1/2$ and $S_{z} = 1/2$. There is nothing in the Hilbert space that could be used to represent such a combined property.

# Operators
## Definition

-   *Operators* are linear maps of the Hilbert space $H$ onto itself. If $A$ is an operator, then for any $|\psi\rangle$ in H, $A|\psi\rangle$ is another element in $H$, and linearity means that 

    $$
    A(b|\psi\rangle + c|\phi\rangle) = bA|\psi\rangle + cA|\phi\rangle
    $$
    
    for any pair $|\psi\rangle$ and $|\phi\rangle$, and any two complex number $b$ and $c$.

    - The product $AB$ of two operators $A$ and $B$ is defined by

    $$
    (AB)|\psi\rangle = A(B|\psi\rangle) = AB|\psi\rangle.
    $$

    In general $AB \neq BA$.

## Dyads and completness

-   The simplest operator is a *dyad*, $|x\rangle \langle w|$. This action is defined by 

    $$
    \color{red}{(|x\rangle \langle w|)}|\psi\rangle = |x \rangle \langle w | \psi \rangle = \color{blue}{(\langle w | \psi \rangle)}|x\rangle.
    $$

    remember the *operators* properties

    $$
    \color{red}{(AB)}|\psi\rangle = A(B|\psi\rangle) = \color{blue}{AB}|\psi\rangle.
    $$

    !!! note 
        The middle term, formed by removing the parentheses and replacing two vertivle bars $||$ with one bar $|$.

## Matrices

## Dagger or adjoint

## Normal operators

## Hermitian operators

## Projectors 

## positive operators

## Unitary operators 

# Bloch sphere


## Reference
1. [Hilbert Space Quantum Mechanics](https://quantum.phys.cmu.edu/QCQI/qitd114.pdf)