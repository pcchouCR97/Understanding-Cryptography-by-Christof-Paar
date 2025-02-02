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


# Reference 
[1]. Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.