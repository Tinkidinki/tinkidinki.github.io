---
title: Change of Basis
layout: post
tags: all-posts maths quantum-information
usemathjax: true
---


In quantum information, there are two common notations to represent vectors and linear transformations between them: the bra-ket notation, and the matrix notation. 

In the bra-ket notation, it is possible to explicitly write the basis that is being used to represent a vector, whereas, in the matrix notation, this is implicit.

Consider a vector $$v \in \mathbb{R}^3$$. In an orthonormal basis $$\{\vert a_i \rangle\}$$, we have:

$$v = \sum_{i=1}^3\alpha_i \vert a_i \rangle$$

$$ v = \begin{bmatrix} \alpha_1 \\  \alpha_2 \\ \alpha_3 \end{bmatrix} $$

In the second notation, the fact that the $$\{\vert a_i \rangle\}$$ basis is being used is implicit. 

A linear transformation on the other hand, is associated with two bases rather than one. A basis for the input vector space and one for the output vector space. 

This is extremely important. A lot of times, if we are writing a linear transformation from a vector space into the same vector space, we may use the same basis---however, as we will see below, this is not necessary. 

### Change of basis
A change of basis transformation is a linear transformation that takes an input vector in a particular basis and represents the *same* vector in another basis. Here, obviously, the output basis is different from the input one. Let us do this with an example. 

In quantum information, there are two bases that are usually used for the space $$\mathbb{H}^2$$. There is the computational basis: $$\{ \vert0 \rangle, \vert1\rangle \}$$ and the $$\sigma_x$$ basis: $$\{ \vert+ \rangle, \vert-\rangle\}$$.

They are related as follows:
$$
\tag{1}
\vert0\rangle = \frac{\vert+\rangle + \vert-\rangle}{\sqrt 2} \\
\vert1\rangle = \frac{\vert+\rangle  - \vert-\rangle}{\sqrt 2} 
$$

Now that we know how the bases are related, we will write a change-of-basis linear transformation in both the notations. We will transform from the $$\{ \vert0 \rangle, \vert1\rangle \}$$ basis to the  $$\{ \vert+ \rangle, \vert-\rangle\}$$ basis. 

We are transforming from a $$2$$ dimensional space back to the same space. Therefore, we require a $$2 \times 2$$ matrix in the matrix notation: 
$$
\tag{2}
\begin{bmatrix}
? & ? \\
? & ? \\
\end{bmatrix} 
\begin{bmatrix}
x \\
y \\
\end{bmatrix}  = 
\begin{bmatrix}
a \\
b\\
\end{bmatrix} 
$$

Unfortunately, because we do not explicitly mention the bases, the above statement becomes confusing, and one might not even know it is a change of basis operation. We have $$\begin{bmatrix} x \\ y \end{bmatrix}$$ in the $$\{ \vert0 \rangle, \vert1\rangle \}$$ basis and $$\begin{bmatrix}
a \\
b\\
\end{bmatrix}$$ in the $$\{ \vert+ \rangle, \vert-\rangle\}$$ basis. What elements do we put in the matrix above such that 
$$\begin{bmatrix} x \\ y \end{bmatrix}_{\{0, 1\}} = \begin{bmatrix}a \\ b\end{bmatrix}_{\{+, -\}}$$ ? 

[We mentioned the vector's current basis in the subscript although in practice this is never done.]

We can substitute the vectors $$\begin{bmatrix} 1 \\ 0 \end{bmatrix}_{\{0, 1\}}$$ and $$\begin{bmatrix} 0 \\ 1 \end{bmatrix}_{\{0, 1\}}$$ for $$\begin{bmatrix} x \\ y \end{bmatrix}_{\{0, 1\}}$$ in equation $$(2)$$ and use the relations in $$(1)$$ to write the elements of the change of basis matrix. 
$$
\tag{3}
\begin{bmatrix}
\frac{1}{\sqrt 2} &  \frac{1}{\sqrt 2}\\
\frac{1}{\sqrt 2} & -\frac{1}{\sqrt 2} \\
\end{bmatrix} 
\begin{bmatrix}
x \\
y \\
\end{bmatrix}  = 
\begin{bmatrix}
a \\
b\\
\end{bmatrix} 
$$

How do we do the same in bra-ket notation? Well, here this is much more straightforward. We directly use the relations in $$(1)$$ to write the transformation as:
$$\left( \frac{\vert+\rangle + \vert-\rangle}{\sqrt 2} \right) \langle 0 \vert + \left( \frac{\vert+\rangle - \vert-\rangle}{\sqrt 2} \right) \langle 1 \vert $$

A vector of the form $$x\vert0\rangle + y\vert1\rangle$$ left multiplied by the above transformation will become $$a\vert+\rangle + b\vert-\rangle$$ such that:
$$x\vert0\rangle + y\vert1\rangle = a\vert+\rangle + b\vert-\rangle$$

The inner product notation allows us to concisely express the same matrix multiplication as above. The basis being explicit allows us to see the transformation clearly. However, the basis being explicit may also cause us to incorrectly simplify the above transformation as: 
$$\vert0\rangle \langle 0\vert + \vert1\rangle \langle 1\vert$$

This is just the identity, you say! Yes, if you transform a vector from one basis to the same basis while ensuring it is the same vector, you get the identity transformation. Even the matrix transformation  above in $$(3)$$ will be equal to the identity if you go from one basis to the same basis. 

### Other transformations --- actually changing the input vector
Rotations and other linear transformations, unlike the change of basis transformations above, may deal with the same input and output basis. These transformations change the vector itself and not just the representation. This time, one may get confused in the bra-ket notation. For example, a rotation may be written as:
$$\vert+\rangle \langle 0\vert + \vert-\rangle \langle 1\vert$$

This is not a change of basis matrix! You may rewrite the above in the same input and output basis to see that it is not equal to the identity:
$$\left( \frac{\vert0\rangle + \vert1\rangle}{\sqrt 2} \right) \langle 0 \vert + \left( \frac{\vert0\rangle - \vert1\rangle}{\sqrt 2} \right) \langle 1 \vert $$.
