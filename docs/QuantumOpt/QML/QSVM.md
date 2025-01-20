# Quantum Vector Support Machine 

## Support vector machine
A support vector machines takes inputs in an $n$-dimentional Euclidean space $(R^{n})$ and classifies them according to which side of a hyperplane they are on. This hyperplane fully defines the behavior of the SVM, which can be defined by adjustable parameteres $\overrightarrow{w}$ and the constant $b$.

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/SVM_margin.png" alt="QSVM_1" style="width: 400px; height: 400px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        SVM Margin
    </p>
</div>

### Training support vector mahcines
Let's say that we have a binary classification problem, and we are given some training data consisting of datapoints in $R^{n}$ together with their corresponding lables. In SVM, we want to look for the hyperplane that best seperates the two categories in the training dataset.

Let the datapoints in our training dataset be $\overrightarrow{w} \in R^{n}$ and their expected label by $y_{j} = 1, -1$. Also, we assume these datapoints cam be perfectly separated by a hyperplane.

When we train a classifier, we are interested in getting a low generalization error. In our case, we acheve this by looking into a hyperplane that can maximize the distance from itself to the training datapoints. For example, if our separating hyperplane is too close to one of the training datapoints, we risk another datapoint of the same class crossing to the other side of the hyperplane and being misclassified.

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/QSVM_1.png" alt="QSVM_1" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Both hyperplanes separate the two categories, but the continuous line is closer to the datapoints than the dashed line.
    </p>
</div>

!!! Note 
    We seek to find not just any separating hyperplanem, but the one that is far away from the training points as possible.

There are two ways that could help us achieve:

1. We can consider the distance from a separating hyperplane $H$ to all the points in the training dataset, and then try to find a way to tweak $H$ to maximize that distance while making sure that $H$ still separates the data properly.
2. Instead, we can associate to each data point a unique hyperplane that is parallel to $H$ and contains that datapoint. The parallel hyperplane that goes through the point that is closest to $H$ will itself be a separating hyperplane - and so will be its reflection over $H$.

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/QSVM_2.png" alt="QSVM_2" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. The solid black line represents a separating hyperplane \( H \). The red dashed line is parallel hyperplane which goes through the closest point to \( H \), and it's reflection over \( H \) is the other dashed line.
    </p>
</div>

This pair of hyperplanes — the parallel plane that goes through the closest point and its reflection — will be the two equidistant parallel hyperplanes, which are the furthest apart from each other while still separating the data. The distance between them is also know as the **margin** and it is what we aim to maximize.

Since any separating hyperplane $H$ can be written as $\overrightarrow{w} \cdot \overrightarrow{x} + b = 0$. And any hyperplane that is parallel to $H$ can be described as $\overrightarrow{w} \cdot \overrightarrow{x} + b = C$ for some constant $C$. And the reflection of this plane is $\overrightarrow{w} \cdot \overrightarrow{x} + b = -C$. That is, for some constant $C$, the hyperplane and its reflection can be characterized as 

$$
\overrightarrow{w} \cdot \overrightarrow{x} + b = \pm C.
$$

If we set $\tilde{w} = \overrightarrow{w}/C$ and $\tilde{b} = b/C$, the equation can be rewritten as 

$$
\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \pm 1,
$$

for some values of $\overrightarrow{w}$ and $b$. Also, we know that the distance between these two hyperplanes is $2/||w||$. Therefore, we can describe our problem as maximizing $2/||w||$ while subjecting to the constraint of $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \pm 1$.

Let's consider an arbitrary element $\overrightarrow{p} \in R^{N}$ and a hyperplane $H$ characterized by $\overrightarrow{w} \cdot \overrightarrow{x} + b = 0$. We know that $\overrightarrow{p}$ is in the hyperplane when $\overrightarrow{w} \cdot \overrightarrow{p} + b = 0$. As the result increases, point $\overrightarrow{p}$ gets further away from the hyperplane. The point is in this latter hyperplane when this value reaches 1. And this point moves beyond both hyperplanes as the value is greater than 1.

Since we assume that there are no points inside the margin, the hyperplane $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = 0$ will properly separate the data if, for all the positive entries, $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \geq 1$, while all the negative ones will satisfy $\tilde{w} \cdot \overrightarrow{x} + \tilde{b} = \leq -1$. We conclude this as 

$$
y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}}) \leq 1,
$$

since we are considering $y_{j} = 1$ when the $j$-th example belongs to the positive class and $y_{j} = -1$ when it belongs to the negative one.

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/QSVM_3.png" alt="QSVM_3" style="width: 400px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. The hyperplane that could have been returned by an SVM is represented by a black solid line, and the lines in dashed lines are the equidistant parallel hyperplanes that are the furthest apart from each other while still separating the data. The margin is thus half of the thickness of the colored region
    </p>
</div>

There we can write our optimization problem as 

$$
\begin{array}{ll}
\text{Minimize} & ||w||,\\
\text{Subject to} & y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}} + b) \geq 1,
\end{array}
$$

where each $j$ defines an individual constraint. We can also write our problem as 

$$
\begin{array}{ll}
\text{Minimize} & \frac{1}{2}||w||^{2},\\
\text{Subject to} & y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}} + b) \geq 1,
\end{array}
$$

this is also called as **hard-margin training** since we are allowing no elements in the training dataset to be misclassified or even to be inside the margin. This expression can save us from troubles due to Euclidean norm in most of the optimization algorithms.

With hard-margin training, we need our training data to be perfectly separable by a hyperplane because, otherwise, we will not find any feasible solutions to the optimization problem that we have just defined. Alternatively, we can use the technique called **soft-margin training**

### Soft-margin training
In contrast to the **hard-margin training**, which does not allow any points being misclassified, **Soft-margin** classification is more flexible, allowing for some misclassification through the use of slack variables ($\xi$). we will use 

$$
y_{i}({w} \cdot {x_{j}} + b) \geq 1 - \xi_{j}.
$$

When $\xi_{j} > 0$, we will allow $\overrightarrow{x_{j}}$ to be close to the hyperplane or even on the wrong side of the space. The larger the value of $\xi_{j}$ is, the further into the wrong side $\overrightarrow{x_{j}}$ will be.

Since we would these $\xi_{j}$ to be as small as possible, we need to include them into the cost function. Thus,

$$
\begin{array}{ll}
\text{Minimize} & \frac{1}{2}||w||^{2} + C\sum_{j} \xi_{j}, \\
\text{Subject to} & y_{i}(\overrightarrow{w} \cdot \overrightarrow{x_{j}} + b) \geq 1 - \xi_{j},\\
& \xi_{j} \geq 0,
\end{array}
$$

where $C>0$, a hyperparameter for a penalty term, is a hyperparameter that can be chosen by us. The larger the value of $C$ is, the less misclassification is allowed. Uf there is a hyperplane that can perfectly separate the data, setting $C$ to a huge value would be equivalent to doing hard-margin training. You may think to make $C$ as larger as possible, but this can make our model prone to overfitting.

The soft-margin training problem can be equivalently written in terms of some optimizable parameters $\alpha_{j}$ as follows:

$$
\begin{array}{ll}
\text{Maximize} & \sum_{j}\alpha_{j} - \frac{1}{2}\sum_{j,k}y_{i}y_{k}\alpha_{j}\alpha_{k}(\overrightarrow{x_{j}} \cdot \overrightarrow{x_{k}}),\\
\text{Subject to} & 0 \leq \alpha_{j} \leq C,\\
& \sum_{j}\alpha_{j}y_{j} = 0.
\end{array}
$$

This formulation of the SVM soft-margin training problem is, most of the time, easier to solve in practice. Once we get the $\alpha_{j}$ values, it is also possible to go back to the original formulation. For instance, it holds that 

$$
\overrightarrow{w} = \sum_{j}\alpha_{j}y_{j}\overrightarrow{x_{j}}.
$$

Notice that $\overrightarrow{w}$ only depends on the training point $\overrightarrow{x_{j}}$, for which $\alpha_{j} \neq 0$. These vectors are call **support vectors**.

### The kernel trick

Sometimes, it's hard to separat effectively a model by any SVM linearly. For example, the figure (a) shown below.

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/QSVM_4.png" alt="QSVM_4" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. The original data cannot be separated by a hyperplane linearly. The separating hyperplane is represented by a dashed line in (b) after applying the kernel trick
    </p>
</div>

Luckly, we can apply **kernel trick** technique, which maps the original space $R^{n}$ into a higher dimensional space $R^{N}$. This higher dimenstional space is also called **feature space** and we refer to the function $\phi : R^{n} \rightarrow R^{N}$ as a **feature map**.

For example, figure above shows that a kernel trick is implemented to map 1-dimensional real line into a 2-dimensional plane with the function 

$$
F(x) = (x,x^{2})
$$

To use the kernel trick, the single and only computation that we need to perform in the feature space is 

$$
k(x,y) = \phi(\overrightarrow{x})\phi(\overrightarrow{y}).
$$

This is also know as a **kernel function**. The kernel functions are functions that can be represented as inner product in some space.

!!! note
    The kernel trick allows computations involving a high-dimensional feature space through inner products $(\phi(x),\phi(y))$ without explicitly transforming the data, enabling efficient classification.

## Going quantum

A Quantum Support Vector machines is a special case of SVMs which rely on the [**kernel trick**](./QSVM.md#the-kernel-trick).

### General idea behind quantum support vector machines

In quantum support vector machiens, we only need to use our quantum algorithm for the kernel function. This function will have to rely on a quantum computer to d othe following:

1. Take as input two vectors in the original space of data.
2. Map each of tehm to a quantum state through a [**feature map**](./QSVM.md#the-kernel-trick).
3. Compute the inner product of the quantum states and return it.

Let's say we ahve a feature map $\phi$ such that for each $\overrightarrow{x}$, we will have a circuit $\Phi(\overrightarrow{x})$ wuch that the output of the feature map will be quantum state $\phi(\overrightarrow{x}) = \Phi{\overrightarrow{x}}|0\rangle$. With a feature map ready, we can then take our kernel function to be

$$
k(\overrightarrow{a},\overrightarrow{b}) = |\langle\phi(a)|\phi\rangle(c)|^{2} = |\langle 0|\Phi^{\dagger}(a)\Phi(b)|0\rangle|^{2}.
$$

This is nothing then the **probability** of measuring all zeros after preparing the state $\Phi^{\dagger}(a)\Phi(b)$. This follows from the fact that the computational basis is [**orthonormal**](/docs/Math_Fundamentals/matrices.md#orthonormal-basis).

Since quantum circuits are always represented by unitary operations, $\Phi^{\dagger}$ is just a inverse of $\Phi$. But $\Phi$ will be given by a series of quantum gates. Therefore, all you have to do to calculate $\Phi^{\dagger}$ is to apply these gate inversely then and $\Phi$.

Let's review how to implement a quantum kernel function.

1. Take a feature map that will return a circuit $\Phi(\overrightarrow{x})$ for any input $\overrightarrow{x}$.
2. You prepare the state $\Phi^{\dagger}(a)\Phi(b)$ for the pair of vectors on which you want to compute the kernel.
3. Return the **probability** of measuring zero on all the qubits.

!!! Excercise 
    One of the conditions for a function $k$ to be a kernel is that it be symmetric. Prove that, for any quantum kernel, is symmetric. ($k(\overrightarrow{a},\overrightarrow{b}) = k(\overrightarrow{b},\overrightarrow{a}))$ for any inputs.)

### Feature maps

!!! Note
    A feature map is a parameterized circuit $\Phi(\overrightarrow{x})$ that depends on the original data and thus can be used to prepare a state depends on it.

#### Angle encoding
The Angle encoding consists in the application of rotation gate on each qubit $j$ parameterized by the value $x_{j}$. In angle encoding, we are using the $x_{j}$ values as angles in the rotations hence the name of the encoding.

We are free to use any rotation gate of our choice. However, if we use $R_{Z}$ gates and take $|0\rangle$ to be our initial state. **The action of our feature map will have no effects**, as a result, we have to apply Hadamard gates on each qubit.

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/QSVM_5_angle_encoding.png" alt="QSVM_5_angle_encoding" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. Angle encoding of an input \( (x_{1}, \cdots, x_{n}) \) using different rotation gates.
    </p>
</div>

These parameters should be properly normaliezd within a certian interval before feeding into angle encoding. For example, if they are normalized between $0$ and $4\pi$, then the data will be mapped to a wider region of the feature space then if they were normalized with in $0$ to $1$ interval.

#### Amplitude encoding

Alghouth the Angle encoding can take $n$ inputs on $n$ qubits, the Amplitude encoding can take $2^{x}$ inputs when implemnted on an $n$-qubit circuit. If the amplitude encoding feature map is given an input $x_{0}, \cdots, x_{2^{n}-1}$, it simply prepares the state

$$
| \phi(\overrightarrow{a}\rangle) = \frac{1}{\sqrt{\sum_{k}x_{k}^{2}}}\sum_{k=0}^{2^{n}-1}x_{k} |k\rangle.
$$

As we were told in the basic quantum state, all quantum states need to be normalized vectors. And that's what we have done for this equation to make sure the output is, obviously, a quantum state. Also, we can tell from the formula, the amplititude encoding works for any inputs except for the zero vector since nothing can be divided by $0$.

Normally, we don't use all the $2^{n}$ parameters that amplititude encoding provides. Instead, we use some of them and fill the rest with zeros. If you want to use all $2^{n}$ inputs for $n$-qubits, you have to make sure that $n$ is big enough to avoid loss information in $2^{n}-1$ degree of freedom.

#### ZZ feature map

**ZZ feature map** can take $n$ inputs $a_{1}, \cdots, a_{n}$ on $n$ qubits, just like angle embedding. Its parameterized circuit is constructed following these steps:

1.  Apply a Hadamard gate on each qubit.
2.  Apply a rotation $R_{Z}$ on each qubit $j$.
3.  For each pair of elements $\{j,k\}\subseteq\{1,\cdots,n\}$ with $j < k$, do
    -   Apply a CNOT gate targeting qubit $k$ and controlled by qubit $j$. 
    -   Apply a rotation $R_{Z} (2(\pi - x_{j})(\pi - x_{k}))$ on each qubit.
    -   Apply a CNOT gate targeting qubit $k$ and controlled by qubit $j$. 

<div style="text-align: center;">
    <img src="/QuantumOpt/images_QML/QSVM_6_ZZ_feature.png" alt="QSVM_6_ZZ_feature" style="width: 800px; height: 300px;">
    <p style="font-size: 16px; font-style: italic; color: gray; margin-top: 5px;">
        Figure. \( ZZ \) feature map three qubits with inputs \(x_{1}, x_{2}, x_{3} \).
    </p>
</div>

To guarantee a healthy balance between separating the extrema of the dataset and using as big a region a possible in the feature space, the variables could be normalized to [0,1].

## Quantum support vector machines in PennyLane
    
## Quantum support vector machines in Qiskit

# Reference 
[1]. Combarro, E. F., & González-Castillo, S. (2023). A practical guide to quantum machine learning and quantum optimisation: Hands-on approach to modern quantum algorithms. Packt Publishing.

[2]. What are support vector machines (SVMs)? https://www.ibm.com/think/topics/support-vector-machine.

[3]. [Support vector machine](https://en.wikipedia.org/wiki/Support_vector_machine)By <a href="//commons.wikimedia.org/w/index.php?title=User:Larhmam&amp;action=edit&amp;redlink=1" class="new" title="User:Larhmam (page does not exist)">Larhmam</a> - <span class="int-own-work" lang="en">Own work</span>, <a href="https://creativecommons.org/licenses/by-sa/4.0" title="Creative Commons Attribution-Share Alike 4.0">CC BY-SA 4.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=73710028">Link</a>