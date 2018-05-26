---
title: Deep Learning Note -- Vectorization on Logistic Regression
date: 2018-05-26 14:10:57
tags: [DeepLearning, NN, DataScience]
categories: DeepLearning
---
Vectorization, fundamental in all machine learning procedure, is one of the best way to improve efficience during training. In this note, we wil first discuss how to perform vectorization with python when implementing a neural network, and then turn to introduce the most basic framework of deep network.
<!-- more -->

# Intro

Training a network is a iterative proceudre which requires go though the whole training set again and again. Traditionaly, this kind of repeating is implemented as explicit for loop in programming. But this explicit for loop doesn't meet our need when training for a deep network since it is low time-efficience and asychronous. In deep learning, the input volume is much larger than traditional methods, this scenario pushs us to find a faster way in implementation. And vectorization is our solution. 

# Vectorization

## Example 1
We use one single step, $ z=w^Tx+b$, from the previous logistic regression as an example. The $w$ and $x$ will be:

$$
\begin{aligned}
w = \left[ \begin{array}{c} w_1 \\ w_2 \\ \vdots \\\end{array} \right] \in \Re^{n_x} , \; 
x = \left[ \begin{array}{c} x_1 \\ x_2 \\ \vdots \\\end{array}\right] \in \Re^{n_x}
\end{aligned}
$$

If we implement this by explicit for loop, the code will be like this:

```python
z = 0
for i in range(n_x): # n_x is the number of instance in traning set.
    z += w[i]*x[i]
z += b
```

The program will go thought every element in the vectors one by one, multiply them, and add to the output.

But when we vectorize the procedure, the code will be like this:

```python
z = np.dot(w.T, x) + b
```

It turns out that vectorization will be over 300 times faster than the explicit for loop. Also, vectorization is more friendly for parallel computation on both CPU and GPU.

## More examples

Let's say we need to perform this procedure:

$$
v = \left[ \begin{array}{} v_1 \\ v_2 \\ \vdots \\ v_m \end{array}\right]
\Rightarrow \;
u = g(v) = \left[ \begin{array}{} g(v_1) \\ g(v_2) \\ \vdots \\ g(v_m) \end{array}\right]
$$

There are several examples:
1. $g(x)=e^x$:
    ```python
    u = np.exp(v)
    ```
2. $g(x)=log(x)$:
    ```python
    u = np.log(v)
    ```
3. $g(x)=|x|$:
    ```python
    u = np.abs(v)
    ```
4. Others
    ```python
    np.max(v, 0)
    v ** 2
    1 / v
    ```

## Vectorize logistic regression

The logistic regression we use here is as same as the last post:

$$
\begin{aligned}
Given \; x, \;\hat{y}=P(y=1|x), where\; 0\leq\hat{y}\leq1 \\
\hat{y}^{(j)}=\sigma(w^Tx^{j}+b), \;where\;\sigma(Z^{(j)})=\frac{1}{1+e^{-z^{(j)}}}
\end{aligned}
$$

The vectorization wil perform in both forward propagation & backward propagation:

1. Fprop:
    We first define the vector for input X & parameter W:

    $$
    \begin{aligned}
    X =& \left[ \begin{array}{} x^{(1)}, x^{(2)}, \cdots, x^{(m)}\end{array}\right] \in \Re^{n_x \times m}\\
    W^T =& \left[ \begin{array}{} w^{(1)}, w^{(2)}, \cdots, w^{(m)}\end{array}\right] \\
    Y =& \left[ y^{(1)}, y^{(2)}, \cdots, y^{(m)}\right]
    \end{aligned}
    $$

    The ouput will be: 
    $$ 
    \begin{aligned}
    Z =& \left[ \begin{array}{} z^{(1)}, z^{(2)}, \cdots, z^{(m)}\end{array}\right] = W^TX + [b, b, \cdots, b]_{1 \times m} \\
    =&\left[W^Tx^{(1)}+b, \cdots, W^Tx^{(m)}+b\right] \\
    A =&\;\sigma (Z) = \left[ \sigma (z^{(1)}), \sigma (z^{(2)}), \cdots, \sigma (z^{(m)}) \right]
    \end{aligned}
    $$

    By vectorization, the code will be:
    ```python
    # fprop
    Z = np.dot(W.T, X) + b
    A = sigmoid(Z) # predefine function
    ```

2. Bprop:
    The backward propagation is shown below:

    $$
    \begin{aligned}
    dZ &= \left[ dz^{(1)}, dz^{(2)}, \cdots, dz^{(m)}\right] = A-Y \\
    db &= \frac{1}{m} \times \sum^{m}_{i}{dz^{(i)}} \\
    dW &= \frac{1}{m} X dZ^T = \frac{1}{m}\left[ \begin{matrix} x^{(1)},\cdots\end{matrix}\right] \left[\begin{matrix} dz^{(1)} \\ \vdots \end{matrix}\right] \\
        &= \frac{1}{m}\left[\begin{matrix} x^{(1)}dz^{(1)} + \dots + x^{(m)}dz^{(m)}\end{matrix}\right] \\
    W &= W -\alpha \times dW \\
    b &= b- \alpha \times db
    \end{aligned}
    $$

    By vectorization, the code will be:
    ```python
    dZ = A-Y
    db = np.sum(dZ) / m
    dW = np.dot(X, (A-Y).T) / m
    W -= learning_rate * dW
    b -= learning_rate * db
    ```
3. Parameter Initialization
    One thing that needed to keep in mind is that the parameter **weight**, or **W**, can not be initialized as **zero** but some random small numbers. And the **bias**, or **b**, can be initializaed as 0. (Althoght **W** can be initialized as 0 here in this single node logistics regession case, I want it to be consistance to the following example in any other network with more than one node.)
    
    Code like this:

    ```python
    w = np.random.randn(w_shape)
    b = np.zeros(b_shape)
    ```