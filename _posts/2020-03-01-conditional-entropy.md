---
title: Conditional Entropy
layout: post
usemathjax: true
tags: all-posts quantum-information
---

I finally put up [my first paper](https://arxiv.org/abs/2001.11237): 'Witnessing Negative Conditional Entropy as an Operational Resource' on arxiv!

The code for the results in the paper and the graphs can be found [here](https://github.com/Tinkidinki/cvenn-codes).

For the uninitiated, conditional von Neumann entropy is a property of quantum states. Turns out the states for which this quantity is negative are pretty damn useful for a lot of information theoretic tasks, for instance: the famous superdense coding experiment, where quantum states can communicate more information than classical states of the same dimensionality (provided there is some groundwork done beforehand). States with negative conditional von Neumann entropy are the ones that provide an advantage over classical states in this protocol. 

Other lesser known tasks for which this negativity of conditional von Neumann entropy is an advantage are state merging, and reducing uncertainty due to unknown measurements on quantum states. 

I'm actually quite fascinated by how this quantity seems to capture quantum advantage in these scenarios and find it quite unintuitive. In fact, the proof for how it is useful for state merging is 30 pages long.

However, probing at it for a year has taught me that it seems to be useful in protocols that involve a distributed quantum state being merged again at some point.

It is not useful to quantify the advantage in quantum teleportation, for instance, which does not involve merging back the distributed state, but rather using the distributed state to transfer another one. 

In our paper, we characterize the set of all states with non-negative conditional von Neumann entropy (the useless states in this context), and provide a method (_a witness_) to distinguish the useful states from the useless ones in a laboratory. 

What was interesting for me about this research endeavour was not so much the final result as some parts of the process. 

When I first encountered the problem, it seemed so similar to the classification of states that are entangled and separable---after all, negative conditional von Neumann entropy states are but a subset of entangled states, and the sets in question in both these problems are convex---that I had immersed myself in studying about the various entanglement witnesses. 

Only when it hit me how different my problem was---unlike the entanglement scenario, I had a closed form function to decide whether a state was useful or useless---was I able to make progress and start  thinking of different directions in which I could proceed. 

At this point in time, the problem solved here doesn't seem like much, but looking back  I have to marvel at how much better I understand the problem. A year ago, I didn't have a single witness, and now I know three different ways of finding one. And for these small joys I have everyone who helped out along the way to thank---my guides and friends at college, and the kind people who live in the parallel universe where [stack exchange](https://quantumcomputing.stackexchange.com/) is opened for _answering_ questions.
