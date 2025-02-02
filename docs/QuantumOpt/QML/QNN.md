# Quantum Neural Networks

There are the contents of this chapter:

1.  Building and training quantum neural networks
2.  Quantum neural networks in PennyLane
3.  Quantum neural networks in Qiskit


## Building and training a quantum neural network

### From classical neural networks to quantum neural networks


|                     | Convolutional Neurak Networks             | Quantum Neurak Networks                  |
| ------------------- | ----------------------------------------- | ---------------------------------------- |
| Data Pre-processing | Data normalizing or scaling               | Encode classic data via **feature map**, normalize if needed |
| Data processing     | Feeding the data via a sequence of layers | Use **variational form (ansatzs)** to resembles the spirit of a classical neural network |
| Data Output         | Retrun the output via a final layer       | The result of some measurement operation (whichever suits our problem best) |


In fact, both **feature map** and **variational form (ansatzs)** are both examples of **variational circuit**: quantum circuits that are controlled by some classical parameters.

-   **Feature map**: depend on the input data and are used to encode it.
-   **Variational form (ansatzs)**: depend on optimizable parameters and are used to transform a quantum input state.

<div style="text-align: center;">
    <img src="../../images_QML/QNN_flow.png" alt="QNN_flow" style="width: 400px; height: 100px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Quantum neural networks flow. Where \( F \) us a feature map, variational form \( V \) depends on some optimizable parameters \( \overrightarrow{\theta} \). The output of the quantum neural network is the result of a measurement opeartion on the final state.
    </p>
</div>

### Variational forms

In principle, variational forms for QNNs follow a "layered structured," trying to mimic the spirit of classical neural networks.

Let's first define a variational form with $k$ layers, we could consider $k$ vectors of independent parameters $\overrightarrow{\theta_{1}}, \cdots, \overrightarrow{\theta_{k}}$. To define each layer $j$, we take variational circuit $G_j$ depend on the parameters $\overrightarrow{\theta_{j}}$. A common approach is to prepare variational forms by stacking these variational circuits consecutively and separating them by some circuit $U_{ent}^{t}$, inpedendent of any parameters, meant to create entanglement between the qubits.

<div style="text-align: center;">
    <img src="../../images_QML/QNN_Variational_form.png" alt="QNN_Variational_form" style="width: 1200px; height: 100px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        A variational form with \(k\) layers, each defined by a variational circuit \( G_j \) dependent on some parameters \( \overrightarrow{\theta_{j}} \). The circuits \( U_{ent}^{t} \) are used to create entanglement, and the state \( | \psi_{enc} \rangle\) denotes the output of the feature map
    </p>
</div>

#### Two-local

The **two-local form** with $k$ repetitions on $n$ qubits relies on $n \times (k+1)$ optimizable parameters, which we will denote as $\theta_{rj}$ with $r = 0, \cdots, k$ and $j=1,\cdots,n$. The two-local circuit is constructed as

``` py
    # TWOLOCAL (n, k, theta)
        for r in range(k):
            # DO: add the r-th layer.
            for j in range(1, n+1): # for all t = 1,...,n
                # DO: Apply a Ry(theta_{rj}) gate on qubit j
            # DO: create entanglement between layers
            if r < k:
                for t in range(1, n): # for all t = 1,...,n-1
                    # DO: Apply a CNOT gate with control on qubit t and target on qubit t+1
```

Below figure shows $n = 4$ and $k=3$ in the two-local method. The two-local variational form uses the same circuit as the angle encoding feature map for its layers, and then **it relies on a cascade of contrlled-NOT operations in order to create entanglement** (see CNOT gates after each $R_{Y}(\theta_{rj})$ implementation). Also, the tow-local variational form with $k$ repetitions has $k+1$ layers, not $k$. The two-local variational form is very versatile, and it can be used with any measurement operation.

<div style="text-align: center;">
    <img src="../../images_QML/QNN_two_local.png" alt="QNN_two_local" style="width: 650px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Two-local variational form on four qubits and two repetitions.
    </p>
</div>

For a two-local variational form, there are more options for the distribution of gates in entanglement circuit besides the "linear" model we have just covered. See the below figure for **circular** and **full** entangelment circuits diagram.

<div style="text-align: center;">
    <img src="../../images_QML/QNN_entanglement_type.png" alt="QNN_entanglement_type" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Different entanglement circuits
    </p>
</div>

#### Tree tensor

The **tree tensor** variational form with $k+1$ layers can be applied on $n = 2^{k}$ qubits. The variational form relies on $2^{k}+2^{k-1}+2^{k-2}+ \cdots + 1$ optimizable parameters of the form 

$$
\theta_{rs}, \quad r = 0,\cdots,k, \quad s = 0,\cdots,2^{k-r}-1.
$$

The tree tensor circuit is constructed as 

``` py 
    # TREETENSOR (k, theta)
    # on each qubit j, apply a rotation Ry(theta_0j).
    for r in range(1,k+1): # for all r = 1,..., k
        for s in range(2^{k-r}): #for all s = 0,...,2^{k-r}-1 do
            # DO: Apply a CNOT operation with target on qubit 1+s2^{r} and controlled by qubit 1 + s2^{r} + 2^{r-1}
            # DO: Apply a rotation Ry(theta_{r,s}) on qubit 1+s2^{2}.
```
The below image shows an output of the above algorithm for $k=3$.

<div style="text-align: center;">
    <img src="../../images_QML/QNN_variational_tree_tensor.png" alt="QNN_variational_tree_tensor" style="width: 620px; height: 600px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Tree tensor variational form on \( 8 = 2^{3}\) qubits
    </p>
</div>

The tree tensor variational form fits best in quantum neural networks designed to work as binary classifiers. The most natural measurement operation that can be used in conjuction with it is the obtention of the expected value of the first qubit, as measuremented in the computational basis.

#### Strongly entangling layers

The strongly entangling layers variational form acts on $n$ qubits anc can have any number $k$ of layers. Each layer $l$ is given a **range** $r_l$. The variational form uses $3nk$ parameters of the form

$$
\theta_{ljx}, \quad l = 1,\cdots,k, \quad j = 1,\cdots,n, \quad x = 1,2,3.
$$

The strongly entangling layer circuit is constructed as 

``` py 
    # STRONGLYENTANGLINGLAYERS(n,k,r,theta)
    for l in range(1,l+1): # for all l = 1,..., l
        for j in range(1,j+1): # for all j = 1,..., j
            # DO: Apply a rotation RZ(theta_{l,j,1}) on qubit j.
            # DO: Apply a rotation RY(theta_{l,j,2}) on qubit j.
            # DO: Apply a rotation RZ(theta_{l,j,3}) on qubit j.
        for j in range(1,j+1): # for all j = 1,..., j
            # DO: Apply a CNOT operation controlled by qubit j and with target on qubit [(j+r_l - 1 mod N)] + 1.
```

Tge choice to use mostly $Y$ rotations in the previouse examples of variational forms is somewhat arbitrary. We can also use $X$ rotation. And we can also have used a different controlled operation. 

<div style="text-align: center;">
    <img src="../../images_QML/QNN_variational_strongly_entangling_layers.png" alt="QNN_variational_strongly_entangling_layers" style="width: 620px; height: 600px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Strongly entangling layers form on 4 qubits and 2 layers with respective range 1 and 2
    </p>
</div>

### Measurements

From [**VQE**](../QOpt/VQEIntro.md), we know any physical observable can be represented by a Hermitian operator in such a way that all the possible outcomes of the measurement of the observable can be matched to the different eigenvalues of the operator.

When we measure a single qubit in the computational basis, the coordinate matrix with respect to the computational basis of the associated Hermitian operator could well be either of 

$$
M = 
\begin{pmatrix}
1 & 0\\
0 & 0
\end{pmatrix},
\quad
Z = 
\begin{pmatrix}
1 & 0\\
0 & -1
\end{pmatrix}.
$$

Both of these operators represent the measurement of a qubit, but they differ in the eigenvalues that they associate to the distinct outputs.

1.  The M operator associate with eigenvalues 1 and 0 to the qubit's value being 0 and 1 respectively.
2.  The Z operator associate with eigenvalues 1 and -1 to the qubit's value being 0 and 1 respectively.

!!! practice 
    Show that operatoin M and Z can be written as 
    $$
    M = 1|0\rangle \langle 0 | + 0|1\rangle\langle1| = |1\rangle\langle1|, \quad Z = |0\rangle\langle0|-|1\rangle\langle1|
    $$
??? Answer

As we know, PennyLane allow you to work with measurement operations defined by any Hermitian operator.

In an $n$-qubit circuit, you will be able to instruct PennyLane to compute the expectation value of the obervable $M \otimes \cdots \otimes M$, which has as its coordinate repersentation in the copmutational basis the matrix

$$
\begin{pmatrix}
0 & & & \\
  & \ddots & \\
  & 0 & \\
  & & 1 
\end{pmatrix}_{2^{n}\times 2^{n}}
.
$$

You can also consider the observable $Z \otimes \cdots \otimes Z$. This observable will return $+1$ is an even number of qubits are measured as $0$, and $-1$ otherwise. The $Z \otimes \cdots \otimes Z$ operation is also called as a **partiy** observable.

### Gradient computation and the parameter shift rule

After we have all the building blocks, let's see how we can train our QNN model! To do so, we have to bring back our old friend - optimization algorithm.

The optimization algorithm that we shall use for quantum neural networks will be gradient descen algorithm, in particular, the Adam optimizer. The Adam optimizer in quantum optimization needs to obtain the gradient of the expected value of a loss function in terms of the optimizable parameters.

#### Numerical approximation

If we had a real-valued function taking $n$ real inputs $f: \mathbb{R}^{n} \rightarrow \mathbb{R}$, we approximate its partial dervatives as 

$$
\frac{\partial f}{\partial x_j} = \frac{f(x_{1},\cdots,x_{j}+h,\cdots,x_{n}) - f(x_{1},\cdots,x_{n})}{h}
$$

where $h$ is a sufficiently small value.

#### Automatic differentiation

Given the current state of real quantum hardware, odds are that most of the quantum neural networks that you will train will run on simulators.

#### The parameter shift rule

By using the parameter shift rule, you can compute gradients when executing quantum neural networks on real quantum hardware. This technique enables us to compute gradients by using the same circuit in the quantum neural network, yet shifting the values of the optimizable parameteres. This technique cannot always be applied, but it works on many common cases and can be used in conjuction with other techniquesm such as numerical approximation.

For example, if you had a circuit consisting of a single rotation gate $R_{X}(\theta)$  and the measurement of its expectation value $E(\theta)$, you would be able to compute its derivative with respect to $\theta$ as 

$$
\nabla_{\theta}E(\theta) = \frac{1}{2}\bigg(E\bigg(\theta+\frac{\pi}{2}\bigg) -E \bigg( \theta - \frac{\pi}{2}\bigg) \bigg).
$$

This is a analogy to the trigonometric functions such as a derivative of the sine function in terms of shifted values of the same sine function.

!!! Note
    When quantum neural networks are run on simulators, gradients can be computed using automatic differentiation techniques analogous to those of classical machine learning. Alternatively, numerical approximation is always an effective way to compute gradients.


!!! Note 
    Everything looks good and promising, but quantum neural networks also pose some challenges when it comes to training them. 
    
    1.  They are known to be vulnerable to **barren plateaus**: situations in which the **training gradients vanish** and, thus, the training can no longer progress (see the paper by McClean et. al for further explanation). [[2]](../QML/QNN.md#reference)
    2.  It is also known that the kind of measurement operation used and the depth of the QNN play a role in how likely these barren plateaus are to be found. (in a paper by Cerezo and collaborators) [[3]](../QML/QNN.md#reference)
    3.  In any case, you should be vigilant when training your QNNs, and follow the literature for possible solutions should barren plateaus threaten the learning of your models

### Practical usage of quantum neural networks

Here are a collection of ideas that you should keep in mind when designing QNN models and training them.

*   Make a wise choice

    Choose your feature map, variational form, and measurement operation properly. Be intentional about these choices and consider the porblem and the data taht you are working with. Your decision can lead to **barren plateaus**. Try to build a case from a well-established cases in literature.

*   Size matters

    When you use a well-designed variational form, such as two-local, tree tensor, or strongly entanging layers, the power of the resulting quantum neural network will be directly related to the number of optimizable parameters it has

*   Optimize optmization

    For most problems, the Adam optimizer can be your go-to choice for training a quantum neural network

*   Feed your QNN properly

    The data that is fed to a quantum neural network should be normalized according to the requirements of the feature map in use. Also, depends on the complexity of the problem, considering using dimensionality reduction techniques.

If you want to boost the power of your QNN, you amy consider using **data reuploading** technique [[4]](../QML/QNN.md#reference).In QNN, you have a feature map $F$ dependendt on some data $\overrightarrow{x}$, which is then followed by a variational form $V$ dependent on some optimizable parameters $\overrightarrow{\theta_{x}}$ - any number of times you want - before performing the measurement opeartion of the QNN.

<div style="text-align: center;">
    <img src="../../images_QML/QNN_Variational_form_data_reuploading.png" alt="QNN_Variational_form_data_reuploading" style="width: 1200px; height: 100px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        A data reuploading scheme of a QNN
    </p>
</div>

This has been shown, both in practice and in theory [[5]](../QML/QNN.md#reference), to offer some advantages over the simpler, standard approach at the cost of increasing the depth of the circuits that are used.

# Reference 

1.  Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.
2.  M. Cerezo, A. Sone, T. Volkoff, L. Cincio, and P. J. Coles, “Cost function dependent barren plateaus in shallow parametrized quantum circuits,” Nature communications, vol. 12, no. 1, pp. 1–12, 2021.
3.  M. C. Caro, H.-Y. Huang, M. Cerezo, et al., “Generalization in quantum machine learning from few training data,” Nature Communications, vol. 13, no. 1, p. 4919, 2022.
4.  A. Pérez-Salinas, A. Cervera-Lierta, E. Gil-Fuster, and J. I. Latorre, “Data re-uploading
for a universal quantum classifier,” Quantum, vol. 4, p. 226, Feb. 2020.
5.  M. Schuld, R. Sweke, and J. J. Meyer, “Effect of data encoding on the expressive power of variational quantum-machine-learning models,” Phys. Rev. A, vol. 103, p. 032 430, 3 Mar. 2021.