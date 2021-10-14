---
layout: post
title:  "Quantum Signal Processing and Grover's Search"
usemathjax: true
tags: all-posts quantum-information quantum-algorithms quantum-singular-value-transform 
---

On May 6th, 2021, Chuang et al. posted the paper [_A Grand Unification of Quantum Algorithms_](https://arxiv.org/abs/2105.02859) which claims that arguably the three most important algorithms in quantum computing: search, factoring and simulation, all have a common underlying structure: the quantum singular value transform. In this pedagogical paper, based heavily on work by [Gilyen et al.](https://arxiv.org/abs/1806.01838), they explain this structure, starting out with a primitive called _quantum signal processing_, which leads to the _quantum eigenvalue transformation_, and finally, the coveted _quantum singular value transformation_.

This post covers my take on the first part of this review: quantum signal processing, and how Grover's search emerges from it. It is also littered with questions about what I would like to understand better. 

We'll organise this into three sections:
1. **Quantum signal processing (QSP)**: This is a transformation that is defined for single qubit operations. 
2. **Generalising QSP**: How the above transformation is defined for any operation (even those that act on more than one qubit), as long as you care about their action on a small space. Chuang et al. call this portion _amplitude amplification_.
3. **Grover's search from the generalised QSP**: How the Grover's search is basically the QSP, with some constraints on it.

## Quantum Signal Processing (QSP)
As the name suggests, quantum signal processing is a transformation that takes a signal and processes it and either the signal or the processing or both are quantum things.

Alright, let's get precise.
By _signal_, we mean a rotation about the $$X$$ axis of the Bloch sphere. Just so you and I have the exact same picture in our heads, here is the Bloch sphere that's in mine. 

![bloch sphere](/assets/qsp-and-grovers/bloch-sphere.png)

The unitary representing this rotation is given by $$e^{-i\frac{\theta}{2}\sigma_x}$$ where $$\sigma_x$$ is the Pauli matrix 
$$\begin{bmatrix} 0 & 1 \\ 1 & 0 \\ \end{bmatrix}$$. It turns out to be convenient to consider this angle to be $$2 \operatorname{cos}^{-1} a$$, and write this rotation matrix in terms of $$a$$:

$$
W(a) = 
\begin{bmatrix}
a & i\sqrt{1 - a^2} \\ i\sqrt{1 - a^2} & a \\
\end{bmatrix}
$$

The processing of the signal refers to manipulation of the signal by multiplying it with other matrices. The precise way in which this is done is we interleave the signal with rotation matrices about the $$Z$$ axis of the Bloch sphere. That is, matrices of the form $$e^{-i \frac{\phi}{2} \sigma_z}$$, where $$\sigma_z$$ is $$\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$$. We'll denote these rotations by $$R_z(\phi)$$.

Thus, the circuit obtained upon processing the signal looks like
![qsp_circuit](/assets/qsp-and-grovers/qsp_circuit.png)

Let's call a circuit of the above form $$Q$$, and let the number of applications of $$W(a)$$ in $$Q$$ be $$d$$. Then, the quantum signal processing theorem states:
1. For any $$Q$$, we have $$\langle 0 \vert Q \vert 0\rangle$$ is a polynomial in $$a$$ with degree $$\leq d$$. 
2. It is possible to obtain, for any polynomial $$P(a)$$ with degree $$\leq d$$, a set of d+1 phases $$\{\phi_i\}$$ such that a $$Q$$ prepared using this phases as the $$Z$$-rotations will have $$\langle 0 \vert Q \vert 0 \rangle = P(a)$$ [^1].

[^1]: There are some more constraints on $$P(a)$$, but they are not too important in the context of this post.

Moreover, for any polynomial $$P$$, there exist efficient classical algorithms to obtain these phases 

... Chuang's paper comes with a package that computes these phases for you!... 

Now, why might it be useful to do this? At first glance, before reading about QSP's application in Grover's, I felt it might be useful to _gain information about an unknown signal_. 

... Explain using a random function ...

... Show example given in the paper ...




## Generalising the QSP

... Now, let us assume that we have a unitary in some larger Hilbert space ...

... ability to give a phase to any two vectors $$A_0$$ and $$B_0$$ ... 

... A promise (no clue why this is important) ...

... Write the two vectors down ...
... Action of the unitary ...
... This looks familiar ...

In fact it is, blah blah

Now, flipping about the two vectors can be treated as rotation in their respective Bloch spheres

... Basically recover the qsp, but this time notice that we take <A_0 \vert Q \vert B_0> to any polynomial 

## Grover's search from the generalised QSP

... you get only a matrix that flips over some A_0 that you don't know, but you know that it is a computational basis vector
... you pick B_0 to be 0
... you pick U to be one that takes 0 to the uniform superposed vector, so that you know irrespective of what A_0 is, you have fixed a to be 1/sqrt(N)
... Now, the issue is that you don't get to manipulate the rotations about A_0, but you do get to manipulate the rotations about B_0 
... turns out, this is sufficient
... with a Q of size sqrt(N), if you manipulate the rotations about B_0, then, you obtain <A_0 \vert Q \vert B_0 > as 1. 


What if you change the question of Grover's a bit? Can you obtain an O(1) search?
