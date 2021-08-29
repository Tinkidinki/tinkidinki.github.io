---
title: Super Density Matrices
layout: post
tags: all-posts quantum-information
usemathjax: true
---


Density matrices describe ensembles of quantum systems and are useful in two ways. Firstly, if you're preparing a state probabilistically, and secondly if you want to describe a subsystem of a quantum state.

The second application I used often while discarding subsystems, but the first description I have never used except for actually saying, 'I'm preparing state A with this probability, and state B with this probability.'

Hence, I was pleasantly amused when I read [this](https://arxiv.org/abs/quant-ph/0407037) paper, where they used it to describe the [superdense coding protocol.](https://en.wikipedia.org/wiki/Superdense_coding)

Consider the best case scenario for superdense coding between two parties Alice and Bob, so they're sharing a Bell state.

Alice wants to send the outcome of two coin tosses to Bob. She will prepare one of the Bell states according to the outcome of the tosses, so each state gets prepared with one-fourth probability.

Now, for Alice, before she prepares it, and for Bob, before he measures it, the shared state can be considered to be:
$$
\frac{1}{4} \psi^- + \frac{1}{4} \psi^+ + \frac{1}{4} \phi^- + \frac{1}{4} \phi^+ = \frac{I}{4}
$$

The entropy of the state is 2, so Alice was able to send 2 bits worth of information.
The main thing I liked is how 'Alice would prepare so and so state if she saw so and so outcome' is all neatly abstracted away, and we can think of it as Alice actually preparing a state $\frac{I}{4}$.

This extends to the cases in which they do not share a maximally entangled state, Alice would be sending most information to Bob, if she manipulated the overall state as best she could locally, such that it becomes a density matrix of maximum entropy.

