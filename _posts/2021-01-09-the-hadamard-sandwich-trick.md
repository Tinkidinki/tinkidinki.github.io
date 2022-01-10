---
title: The Hadamard Sandwich 
layout: post
tags: all-posts quantum-algorithms  
usemathjax: true
published: false
---

[//]: # Questions that remain
[//]: # Can $$P$$ be just a projector, or does it have to be an orthogonal projector?
[//]: #- When do I use \mid and when do I use|
The Hadamard Sandwich is a neat quantum circuit trick that shows up in several quantum algorithms! In this post, we will cover what the Hadamard Sandwich is, and its application in (1) the swap test, (2) the fixed point quantum search algorithm, and (3) the eigenvalue threshold algorithm.

## What is the Hadamard Sandwich?

Let us first define a type of quantum gate that is our main focus in this post.

**The projector controlled-unitary gate:** Let $$U$$ be a $$2^n \times 2^n$$ unitary matrix (in other words, a quantum gate on $$n$$ qubits), and let $$P$$ be a $$2^m \times 2^m$$ orthogonal projector. Then $$P$$-controlled $$U$$ is the following $$m+n$$ qubit gate:

$$P \otimes U + (I-P) \otimes I.$$

The CNOT gate is an example of a projector-controlled unitary gate, where the projector is $$\mid 1 \rangle \langle 1 \mid$$ and the unitary is $$X$$: 

$$| 1 \rangle \langle 1| \otimes X + | 0 \rangle \langle 0 | \otimes I.$$ 

When the projector $$P$$ is just $$\mid 1\rangle \langle 1 \mid$$---as in the above case---we simply call it a controlled-$$U$$ gate.


Assume you have access to the gate on the left, that is, a controlled-$$U$$ acting on $$n+1$$ qubits, where you are promised that $$U$$ has only eigenvalues $$+1$$ and $$-1$$. Let $$\{\mid e_i \rangle\}_i$$ be an orthogonal basis for the eigenspace of $$U$$ with eigenvalue $$-1$$, and let $$P = \sum_i \mid e_i \rangle \langle e_i \mid$$. That is, $$P$$ is a projector into the $$-1$$ eigenspace of $$U$$. 

![input-output](/assets/the-hadamard-sandwich/input_output.png)

Then, the Hadamard Sandwich (no, I haven't seen this term used anywhere), is a trick to convert the gate on the left (a controlled-$$U$$), 


$$| 1 \rangle \langle 1| \otimes U + | 0 \rangle \langle 0 | \otimes I.$$ 


to the gate on the right (a $$P$$-controlled $$X$$), 

$$ X \otimes P + I \otimes (I-P).$$

Crucially, notice that on the left, $$U$$ is controlled by the top qubit, and acts on all the bottom qubits. However, for the gate on the right, the $$X$$ gate is controlled by all the bottom qubits but acts on the top qubit.

How is this accomplished? By simply sandwiching the top qubit of the gate on the left with Hadamards!

![the-sandwich](/assets/the-hadamard-sandwich/the-sandwich.png)

To prove this, let us write $$U$$ in terms of its $$+1$$ and $$-1$$ eigenvectors:

$$U = \sum_j |a_j \rangle \langle a_j | - \sum_i |e_i \rangle \langle e_i | = (I-P) - P = I - 2P$$

Applying the Hadamard Sandwich to gate on the left, we have, 

$$H| 1 \rangle \langle 1|H \otimes U + H| 0 \rangle \langle 0 |H \otimes I.$$ 

$$ = \frac 1 2 (|0\rangle \langle 0| + |1\rangle \langle 1| - |0 \rangle \langle 1| - |1 \rangle \langle 0 |)\otimes U + \frac 1 2(|0\rangle \langle 0| + |1\rangle \langle 1| + |0 \rangle \langle 1| + |1 \rangle \langle 0 |) \otimes I.$$ 

$$ = (|0 \rangle \langle 1| + |1 \rangle \langle 0 |)\otimes \Big(\frac{I-U}2 \Big) + (|0\rangle \langle 0| + |1\rangle \langle 1|) \otimes \Big(\frac{I+U}2\Big)$$ 

$$ = X \otimes P + I \otimes (I-P)$$ 

Cool! Now let's look at where this idea shows up!

## The Swap Test

The Swap Test is a circuit that can be used to compute the inner product of two vectors given as quantum states. Both states, along with an ancilla qubit in $$ \mid 0\rangle$$ state are given as input to a Hadamard-sandwiched controlled-SWAP:

![swap-test](/assets/the-hadamard-sandwich/swap-test.png)

The probability of measuring the ancilla as $$\mid 1 \rangle $$ is then $$\frac{1 - \Vert \langle \psi \mid \phi \rangle \Vert ^2}2$$, which can be used to compute the inner product.

We first prove this directly. The initial state is

$$ |0\rangle |\psi \rangle |\phi \rangle $$

Upon applying Hadamard,


$$ \frac 1 {\sqrt 2} \big(|0\rangle + |1\rangle \big) |\psi \rangle |\phi \rangle $$

After the controlled swap,

$$ \frac 1 {\sqrt 2} \big(|0\rangle |\psi \rangle |\phi \rangle  + |1\rangle |\phi \rangle |\psi \rangle  \big) $$


Hadamard again,


$$ | \alpha \rangle = \frac 1 2 \bigg(|0\rangle \big(|\psi \rangle |\phi \rangle    + |\phi \rangle |\psi \rangle \big) +  |1\rangle \big(|\psi \rangle |\phi \rangle - |\phi \rangle |\psi \rangle \big)  \bigg) $$

The probability of measuring the ancilla as $$\mid 1 \rangle $$ is

$$ \langle \alpha | \big(|1\rangle \langle 1| \otimes I \big) |\alpha \rangle $$

$$= \frac 1 4 \big(\langle \psi | \langle \phi | - \langle \phi | \langle \psi | \big) \big(|\psi \rangle |\phi \rangle - |\phi \rangle |\psi \rangle \big) $$

$$ = \frac {1 - \Vert \langle \psi \vert \phi \rangle \Vert^2} 2$$

Thus, the inner product can be computed from the measurement probability. We can also analyse the swap test through the lens of the Hadamard Sandwich. We do it here for the case where $$\vert \psi\rangle$$ and $$\vert \phi\rangle$$ are both $$1$$-qubit states. An eigenbasis for the $$2$$-qubit SWAP gate is the Bell basis, where the state $$\vert \beta \rangle = \frac 1 {\sqrt 2} \big (\vert 01 \rangle - \vert 10 \rangle \big )$$ has an eigenvalue $$-1$$ and the rest have eigenvalues $$+1$$.

Now, we know that the Hadamard Sandwich has converted the controlled SWAP into a projector-controlled $$X$$ gate. And since $$\vert \beta \rangle$$ is the only eigenvector with $$-1$$ eigenvalue, the projector is essentially $$\vert \beta \rangle \langle \beta \vert $$. 

Thus, we need to compute the probability of measuring the ancilla as $$\vert 1 \rangle$$ in the following state:

$$ \Big (X \otimes |\beta \rangle \langle \beta | + I \otimes (I- |\beta \rangle \langle \beta |) \Big )|0 \rangle | \psi \phi \rangle$$

which is


$$ \Bigg \Vert \Big ( |1\rangle \langle 1 | \otimes I \Big )\Big (X \otimes |\beta \rangle \langle \beta | + I \otimes (I- |\beta \rangle \langle \beta |) \Big )|0 \rangle | \psi \phi \rangle \Bigg \Vert^2$$


$$ = \Bigg \Vert \Big ( |1\rangle \langle 1 | \otimes I \Big ) \Big (|1\rangle \otimes |\beta \rangle \langle \beta | \psi \phi \rangle \Big ) \Bigg \Vert^2$$

$$ = \Bigg \Vert |1\rangle \otimes |\beta \rangle \langle \beta | \psi \phi \rangle \Bigg \Vert^2$$

$$= \frac 1 2\langle \psi \phi \vert \big ( |01 \rangle - |10 \rangle\big) \big( \langle 01 | - \langle 10 | \big) | \psi \phi \rangle$$

$$ \frac 1 2 \big(\psi_0^* \phi_1^* - \psi_1^*\phi_0^*) \big(\psi_0 \phi_1 - \psi_1\phi_0)$$

$$= \frac 1 2 \big( \psi_0^*\phi_1^*\psi_0\phi_1 + \psi_1^*\phi_0^*\psi_1\phi_0 - \psi_0^*\phi_1^*\psi_1\phi_0 - \psi_1^*\phi_0^*\psi_0\phi_1 \big ) \tag{1}$$  

However, notice that we have $$ \langle \psi \vert \psi \rangle \langle \phi \vert \phi \rangle = 1 $$ 

Thus, 

$$( \psi_0^*\psi_0 + \psi_1^* \psi_1 ) (\phi_0^* \phi_0 + \phi_1^* \phi_1) = 1$$

$$\psi_0^*\psi_0\phi_0^*\phi_0 + \psi_0^*\psi_0\phi_1^*\phi_1 + \psi_1^*\psi_1\phi_0^*\phi_0 + \psi_1^* \psi_1 \phi_1^*\phi_1 = 1$$

and 

$$ \psi_0^*\psi_0\phi_1^*\phi_1 + \psi_1^*\psi_1\phi_0^*\phi_0  = 1 - \psi_0^*\psi_0\phi_0^*\phi_0 - \psi_1^* \psi_1 \phi_1^*\phi_1 \tag{2}$$

Thus, substituting Eq$$(2)$$ in $$(1)$$, we have

$$= \frac 1 2 \bigg( 1 - \big ( \psi_0^*\psi_0\phi_0^*\phi_0 + \psi_1^* \psi_1 \phi_1^*\phi_1 + \psi_0^*\phi_1^*\psi_1\phi_0 + \psi_1^*\phi_0^*\psi_0\phi_1 \big ) \bigg ) $$  

$$= \frac 1 2 \bigg( 1 - \big ( \psi_0\phi_0^* + \psi_1\phi_1^* \big ) \big ( \psi_0^*\phi_0 + \psi_1^*\phi_1 ) \bigg )$$


$$ = \frac {1 - \Vert \langle \psi \vert \phi \rangle \Vert^2} 2$$

which is the original expression obtained in the first method! Thus, another way of thinking about the swap test is that the ancilla qubit is flipped based on how big a component of $$\vert \psi \phi \rangle$$ lies along the state $$\vert \beta \rangle$$, and the inner product between $$\vert \phi \rangle$$ and $$\vert \psi \rangle$$ can be computed from this value.
