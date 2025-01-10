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

-   A quantum system cannot simultaneously process two incompatible properties. For example, a spin-half particle cannot have both $S_{x} = +1/2$ and $S_{z} = +1/2$. There is nothing in the Hilbert space that could be used to represent such a combined property.

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
    (\color{red}{|x\rangle} \color{blue}{\langle w|})|\psi\rangle = \color{red}{|x\rangle} \color{blue}{\langle w|} \psi \rangle = (\color{blue}{\langle w|} \psi \rangle)\color{red}{|x\rangle}.
    $$

    !!! note 
        The middle term, formed by removing the parentheses and replacing two vertivle bars $||$ with one bar $|$.

-   The completness relation

    $$
    I = \sum_{j}|j\rangle \langle j|,
    $$

    where $I$ is the identity operator, $I|\psi\rangle = |\psi\rangle$ for any $|\psi\rangle$. The sum on the right is dyads $|j\rangle \langle j|$ corresponding to the elements $|j\rangle$ of an orthonormal basis.

    -   Theus, we can rewrite a state $|\psi\rangle$
        $$
        \psi = (\sum_{j} \color{red}{|j\rangle}\color{blue}{\langle j|})|\psi\rangle = \sum \color{red}{|j\rangle}\color{blue}{\langle j|}\psi\rangle = \sum_{j}\color{blue}{\langle j |}\psi\rangle \color{red}{|j\rangle}.
        $$

## Matrices
-   Given an operator $A$ and a bisis $\{ |\beta_{j} \rangle\}$ , which need not to be orthonormal, the *matrix* associated with $A$ is the square array of numbers $A_{jk}$ defined by
    
    $$
    A|\beta_{k}\rangle = \sum{j}|\beta_{j}\rangle A_{jk} = \sum_{j}A_{jk}|\beta_{j}\rangle.
    $$

-    For an orthonormal basis, we can rewrite the [completness relation](../Math_Fundamentals/hilbert_space.md#dyads-and-completness)

    $$
    A|k\rangle = I \cdot A|k\rangle = (\sum_{j}\color{red}{|j\rangle} \langle j|)A|k\rangle = \sum_{j}\color{red}{|j\rangle} \langle j| A|k\rangle = \sum_{j}\langle j|A|k\rangle\color{red}{|j\rangle}.
    $$

    In $\langle j|A|k\rangle$, the inner product of $|j\rangle$ with $A|k\rangle$ is just the $A_{jk}$ above. Thus, $\langle j|A|k\rangle$, can be written as $\langle \psi|A|\omega\rangle$, is referred to as a "matrix element" when using Dirac notation.

-   In a similar way the expression

    $$
    A = I \cdot A \cdot I = \sum_{j}|j\rangle \langle j| \cdot A \cdot \sum_{k}|k\rangle \langle k| = \sum{jk}\langle j|A|k\rangle\cdot|j\rangle\langle k|
    $$

    allows us to express *operator $A$* as a sum of dyads, with coefficients given by its matrix elements.

-   When $A$ refers to a qubit the usual wawy of writing the matrix in the standard basis is 

    $$
    \begin{pmatrix}
    \langle 0|A|0\rangle & \langle 0|A|1\rangle \\
    \langle 1|A|0\rangle & \langle 1|A|1\rangle
    \end{pmatrix}
    $$

-   We can also write the matrix element of the product of two operators in terms of the individual matrix elements:

    $$
    \langle j|AB|k\rangle = \langle j|A \cdot I \cdot B|k\rangle = \sum_{m}\langle j|A|m\rangle\langle m|b|k\rangle.
    $$

    If we write this equation in subscripts form,

    $$
    (AB)_{jk} = \sum_{m}A_{jm}B_{mk}.
    $$

## Dagger or adjoint
-   Here are some dagger $^\dagger$ properties:

    $$
    \begin{array}{c}
    (|\psi\rangle)^{\dagger} = \langle\psi|,\\
    (\langle \psi|)^{\dagger} = |\psi \rangle,\\
    (a|\psi\rangle+b|\psi\rangle)^{\dagger} = a^{*}\langle\psi|+b^{*}\langle\psi|,\\
    (|\psi\rangle \langle \omega|)^{\dagger} = |\omega\rangle \langle \psi|,\\
    \langle j|A^{\dagger}|k\rangle = (\langle k|A|j\rangle)^{*},\\
    (aA+bB)^{\dagger} = a^{*}A + b^{*}B,\\
    (AB)^{\dagger} = B^{\dagger}A^{\dagger},
    \end{array}
    $$

    where $a$ and $b$ are complex numbers; $a^{*}$ and $b^{*}$ are its complex conjugate. The operator $A^{\dagger}$ is called the ***adjoint*** of the operator $A$. The matrix of $A^{\dagger}$ is the complex conjugate of the transpose of the matrix of $A$.

## Normal operators
-   A ***normal*** operator $A$ on a Hilbert space in one that commutes with its adjoint, that is, $AA^{\dagger} = A^{\dagger}A$. 
-   A normal operators can be diagonalized using an orthonormal basis, that is 

    $$
    A = \sum_{j} \alpha_{j} |\alpha_{j}\rangle \langle a_{j}|
    $$

    where the basis vectors $|a_{j}\rangle$ are ***eigenvectors*** of $A$ and the complex numbers $\alpha_{j}$ are its ***eigenvalues***. Equivalently, the matrix of $A$ in this basis is diagonal

    $$
    \langle a_{j} | A | a_{k} \rangle = \alpha_{j}\delta_{jk}.
    $$

## Hermitian operators
-   A ***hermitian*** or ***self-adjoint*** operator $A$ is defined by the property that $A = A^{\dagger}$, so it is a normal operator. It is the qunatum analog of a real (not complex) number. Its eigenvalues $\lambda_{j}$ are real numbers.
    -   The term "Hermitian" and "self-adjoint" mean the same thing for a finite-dimensional Hilber space.
    -   Hermitian operators in quantum mechanics are used to represent ***physical variables***, quantities such as energy, momentum, angular momentum, position, etc. The operator representing the energy is the {==***Hamiltonian $H$***==}.
-   There is no always has a well-defined value for a physical system in a particulate in quantum mechanics. Let's say a quantum state $|\psi\rangle$, the physical variable corresponding to the operator $A$ has a well-defined value **if and only if** $| \psi\rangle$ is an eigenvector of $A$, $A|\psi\rangle = \alpha|\psi\rangle$, where $\alpha$m must be a real number since $A^{\dagger} = A$.
    -   The eigenstates of $S_{z}$ for a spin-half particle are $|z^{+}\rangle$ and $|z^{-}\rangle$, with eigenvalues of +1/2 and -1/2, respectively.
    -   If $|\psi\rangle$ is ***not*** an eigenstate of $A$, then in this state the physical quantity $A$ is ***undefined*** , or ***meaningless*** in the sense that quantum theory can assign it no meaning.
    -   The state $|x^{+}\rangle$ is an eigenstate of $S_{x}$ but not of $S_{z}$. hence in this state $S_{x}$ has well-defined value (1/2), where as $S_{z}$ is undefined.

## Projectors 
-   A ***projectors***, short for ***orthogonal projection operator***, is a Hermitian operator $P = P^{\dagger}$ which is idempotent in the sense that $P^{2} = P$. It is a Hermitian operator all or whose eigenvalues are either 0 or 1. {==Therefore, there is always a basis in which its matrix is diagonal $\langle a_{j}|A|a_{k}\rangle = \alpha_{j} \delta_{jk}$, with only 0 or 1 on the diagonal.==} Such a matrix always represents a projector.
    -   

## Positive operators
-   The ***positive*** operators is useful when dealing with probabilities. They are defined as that for every ket $|\psi\rangle$

    $$
    \langle \psi |A|\psi\rangle \geq 0.
    $$

    or 

    $$
    A = \sum_{j} \alpha_{j} |\alpha_{j}\rangle \langle a_{j}|,
    $$

    where $\alpha_{j} \geq 0$. Both representations should be normalized.    

## Unitary operators 
-    A unitary operator $U$ has the property that 

    $$
    U^{\dagger}U = I = UU^{\dagger}.
    $$

    -   Since $U$ commutes with its adjoints, it is a normal operator and can diagonalized using an orthonormal basis. Aboove equation also implies that all the eigenvalues of $U$ are complex numbers of magnitude 1. That is, they lie on the unit circuile in the complex plane.

    - In a finite-dimentional Hilbert space, with $U$ mapping the space into itself, thus, we only have to check one of the above equation to see if $U$ is unitary.

    - In quantum mechnics, unitary operators are used to change from one orthonormal basis to another, to represent ***symmetries***, such as rotational symmetry, and to describe some aspects of the ***dynamics*** or ***time development*** of a quantum system.

# Bloch sphere
-   Any physical state $\psi$ of a qubit (ray or normalized vector in the two-dimensional Hilbert space) can be associated in this way with a direction $w = (w_{x}, w_{y}, w_{z})$ in space for which $S_{w} = 1/2$, i.e., the $w$ component of angular momentum is positive. There is therefore a one-to-one coorespondence bewteen directions, or the correspondence points on the unit shpere, with rays of a two-dimensional Hilber space. This is also known as the ***Bloch sphere representation *** of qubit states.
    -   We often write a state 

    $$
    \begin{array}{c}
    \cos{(\theta/2)}|0\rangle + e^{i\phi}\sin{(\theta/2)}|1\rangle \leftrightarrow S_{w} = +1/2, \\
    \sin{(\theta/2)}|0\rangle - e^{i\phi}\cos{(\theta/2)}|1\rangle \leftrightarrow S_{w} = -1/2,
    \end{array}
    $$

    where the ket are normalized.

    -   Note that it is the ***surface*** of the Bloch sphere - vector $w$ of unit length - that correspond to different rays.

-   Two states of a qubit are orthogonal, physically distinct ot distinguishable, if they are antipodes, two points at opposite ends of a diagonal. For example, $|0\rangle$ $|1\rangle$ are the north and south pole of the Bloch sphere.
    -   Any orthonormal basis of a qubit is associated with a pair or antipodes of that Bloch sphere.

-   A linear operator maps a ray onto a ray, or onto a zero vector. A linear operator on a qubit maps the Bloch sphere onto itself, or in the case of a noninvertible operator, onto a single point on the sphere.

-   Of particular importance are rotations of 180${^\circ}$ about the $x$, $y$, and $z$ axes, obtained using the unitary operators $X = \sigma_{x}$, $Y = \sigma_{y}$, and $Z = \sigma_{z}$, respectively. In the standard basis the corresponding matrices are the Pauli matrices:

$$
X = 
\begin{pmatrix}
0 & 1\\
1 & 0
\end{pmatrix}
, \
Y = 
\begin{pmatrix}
0 & -i\\
i & 0
\end{pmatrix}
, \
Z = 
\begin{pmatrix}
1 & 0\\
0 & -1
\end{pmatrix}
.
$$

# Composite systems and tensor products

## Reference
1. [Hilbert Space Quantum Mechanics](https://quantum.phys.cmu.edu/QCQI/qitd114.pdf)