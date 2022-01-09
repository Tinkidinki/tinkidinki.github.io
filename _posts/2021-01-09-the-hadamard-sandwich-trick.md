---
title: The Hadamard Sandwich 
layout: post
tags: all-posts quantum-algorithms  
usemathjax: true
published: false
---

## Questions that remain
- Can $$P$$ be just a projector, or does it have to be an orthogonal projector?
- When do I use \mid and when do I use $$| 1$$ 

---

The Hadamard Sandwich is a neat quantum circuit trick that shows up in several quantum algorithms! In this post, we will cover what the Hadamard Sandwich is, and its application in (1) the swap test, (2) the fixed point quantum search algorithm, and (3) the eigenvalue threshold algorithm.

## What is the Hadamard Sandwich?

Let us first define a type of quantum gate that is our main focus in this post.

**The projector controlled-unitary gate:** Let $$U$$ be a $$2^n \times 2^n$$ unitary matrix (in other words, a quantum gate on $$n$$ qubits), and let $$P$$ be a $$2^m \times 2^m$$ orthogonal projector. Then $$P$$-controlled $$U$$ is the following $$m+n$$ qubit gate:

$$P \otimes U + (I-P) \otimes I,$$

The CNOT gate is an example of a projector-controlled unitary gate, where the projector is $$\mid 1 \rangle \langle 1 \mid$$ and the unitary is $$X$$: 

$$| 1 \rangle \langle 1| \otimes X + | 0 \rangle \langle 0 | \otimes I.$$ 

When the projector $$P$$ is just $$\mid 1\rangle \langle 1 \mid$$---as in the above case---we simply call it a controlled-$$U$$ gate.

Assume you have access to a controlled-$$U$$ gate, and you are promised that $$U$$ only has eigenvalues $$+1$$ and $$-1$$. The Hadamard Sandwich (no, I haven't seen it being called this anywhere) is a simple procedure that can convert this controlled-$$U$$ to a $$P$$-controlled $$X$$ gate, where $$P$$ is a projector onto the "$$-1$$" eigenspace of $$U$$

Assume you have access to the gate on the left, that is, a controlled-$$U$$ acting on $$n+1$$ qubits, where you are promised that $$U$$ has only eigenvalues $$+1$$ and $$-1$$. Let $$\{\mid e_i \rangle\}_i$$ be an orthogonal basis for the eigenspace of $$U$$ with eigenvalue $$-1$$. Let $$P = \sum_i \mid e_i \rangle \langle e_i \mid$$.  

![input-output](/assets/the-hadamard-sandwich/input_output.png)

Then, the Hadamard Sandwich (no, I haven't seen this term used anywhere), is a trick to convert the gate on the left, 

$$\mid 1 \rangle \langle 1 \mid \otimes U + \mid 0 \rangle \langle 0 \mid \otimes I $$

to the gate on the right, 

$$ X \otimes P + I \otimes (I-P).$$

Crucially, notice that on the left, $$U$$ is controlled by the top qubit, and acts on all the bottom qubits. However, for the gate on the right, the $$X$$ gate acts on the top qubit, being controlled by all the bottom qubits.


