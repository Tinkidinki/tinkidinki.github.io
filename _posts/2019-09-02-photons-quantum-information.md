---
title: Photons for Quantum Information People
layout: post
usemathjax: true
tags: all-posts quantum-information
---
As someone who studied quantum information for about a year, I was more than happy to assume that photons were natural qubits given to us, completely defined by their polarization state which I would smoothly represent with ket vectors, with which we could do whatever computation we wanted to do.

But of course, a photon is more than a convenient mathematical notation --- firstly, we can't perform any computation (CNOT is still probabilistic on photons) and it comes with its own baggage: the particle-wave duality, which although information theorists might not care about, is a very physical phenomenon.

This post is about the qualitative things I learnt about photons while frequenting the lab at RRI, Bangalore. It is to document whatever I saw and connections I could infer from quantum information theory and high school physics. Note that since I have not formally studied quantum optics, (I did attempt to read _Quantum Optics for the Impatient_, but by the prerequisites given in the book, you'd have to be pretty damn patient to get through it), this post is far from mathematically accurate. 

_____________________

Firstly, there are multiple properties a photon has, a few of which we exploit for quantum information. Polarization of course, is one. Every photon has an inherent spin which sits in a two-dimensional Hilbert space. 

There are also spatial degrees of freedom, which are restricted using slit-based or split-based systems and can also be used to make qudits. The angular momentum of a photon has also been used to prepare qudits.

And of course, there's the energy each photon has---which determines its colour---and the states of a bunch of photons---The Fock states. 

Not that it's a much better model than I had before, but now I think of photon states being represented by some vector in a bunch of tensored Hilbert spaces, one for each property.

This idea is true for other objects as well. For instance, in the Stern Gerlach experiment, the spin of the atom, and the position the atom lands on after passing through the device are two separate properties of the atom described via vectors in their respective Hilbert spaces. 

The magnetic field entangles both these properties such that we have a way of observing spin by looking at the position as we would not be able to know the spin otherwise. 

Note that position and momentum of particles on the other hand,  are two different bases for the same Hilbert space.

--------------------
## Light Sources

There are three types of light sources used in the lab---Thermal sources, laser sources and single photon sources. These have been divided based on the probability of the source emitting a photon within a given time period. Thermal sources follow super Poissonian statistics, laser sources follow Poissonian statistics and single photon sources follow sub Poissonian statistics. 

A neat way to check if a source is a single photon source is to pass light that comes through the source through a beam splitter, place detectors at both the output ports, and check (within a small finite time period) that only one detector clicks at a time. 

Note that the numbers themselves are mind-boggling. In theory, one might talk about preparing 'one' or 'two' copies of a state, or reducing the number of measurements necessary in a protocol, so as to not 'waste' states by collapsing them.  

Well, in the photon implementation atleast, you prepare around $10^9$ copies of a state at a time in a beam that is continuously going to lose copies of the state whether you measure them or not. (To check: Theoretically reducing the measurements, might reduce the time it takes to perform them and directly determine the amount of time the laser source needs to be switched on for, therefore affecting power loss.)

## How light is moved around
So, when the quantum information experiments are performed, light beams have to be moved from preparing devices to detectors and through other manipulating devices. One way this is done is with mirrors, reflecting light into whatever devices. Another way this is done is to beam the light into an optic fibre (this process is not experimentally trivial -- and takes some time to get right and is called fiber-coupling) and use it from the other end.

## Polarization

Photons have an inherent property called spin. Polarization is the degree to which this spin has aligned with a given direciton. The Z basis, Y basis and X basis are represented by left-right polarization, diagonal-antidiagonal polarization, and circular left-right polarization and these directly map to the bloch sphere.

To prepare a beam of light in a particular polarization state, or to measure it, we just pass it through a polarizer, a chemical sheet that allows only light of certain polarization to pass through it. 

To apply a unitary operator on the light beam, we have to pass it through a wave plate. A half wave plate rotated appropriately changes the $\alpha$ and $\beta$, and the quarter wave plate changes $\phi$ in a state written as $\alpha |0 \rangle + e^{i \phi} \beta |1 \rangle$.

Entanglement is done by a process called SPDC that stand for Spontaneous Parametric Down Conversion. The efficiency of the devices used in SPDC is so bad that when visible light is input, a lot of the energy is lost and infrared light gets output. This means of course, you can't see the output beams and single-photon detectors or CMOS cameras are used to detect them.

There are two types of SPDC. Type I SPDC takes vertical polarized light and outputs entangled pairs of horizontal polarized photons (or vice versa). Type II SPDC generates pairs of oppositely polarized entangled photons.

## Split based systems
![](https://lh3.googleusercontent.com/ZaOmcJ-WxUV8m1lygPX-BwZaZS3U5u7y7hjjs62eyLxXxsX8SbYkvR4hru7HRSsxxWXHr_SIYl4n "Beam Splitter")

A beam splitter is a unitary transformation device that rotates the input photon. $\alpha$ and $\beta$ are determined by the splitting ratio of the beam splitter and $\phi$ is determined by a piece of glass kept in the way of one of the beams to introduce required phase.

Note that the two beams together are now the single photon itself -- just in superposition.

If you were to apply a unitary again on this new state $\alpha |0\rangle + e^{i \phi}\beta |1 \rangle$, place another beam splitter after this one, such that they both enter on two faces of the next beam splitter.

This is where the photon physics kicks in. When a photon is in a coherent state like in the middle of the beam splitting process, the "parts" of the photon in either beam interfere with each other and form interference patterns at the detector. This means the final intensity of light seen at the detector, depends on the phase angle between the two "parts" of the photon.

[Hunch - need to confirm: You can either measure the phase $\phi$ or the ($\alpha$ and $\beta$) but not both. To measure $\alpha$ and $\beta$ you would need detectors placed at both outputs of the beam splitter, and measure the ratio of output. To measure $\phi$, you would have to have mirrors (as shown in the figure), and beam them into a common output and study the interference pattern on the output.]

So for the quantum information scientist, the beam splitter is just an innocent unitary, but in the coherent state, we see the effects of the wave-particle duality, and based on the $\phi$, we see different interference patterns at the detector - due to the wave nature of light.

Based on the configuration of the interferometer, interference can be collinear or non-collinear. More often than not, the alignment of the devices and mirrors in the lab is not perfect (non-collinear), so the beams from the transmitted and reflected parts do not land at the same position on the detector, but land as two Gaussians on separate points which have another phase factor (apart from the original one introduced in the interferometer). If it is perfect (collinear), there is only one Gaussian. (The intensity of the light beam as a function of position from the centre is described by a Gaussian.)

## Slit Based Systems
This is another way of introducing spatial dimensions in photons. Not as effective as a splitter as all the light that does not go through the slits is lost.

## Entangling both these spaces
The polarization beam spitter uses birefringent material to split photon beams based on polarization, entangling the polarization and the spatial properties of a photon. Birefringent materials are those that have a refractive index dependent on the polarization of the incident light.

## Other Stuff
Note that the various polarization states are all equivalent, just as we assume in quantum information, one is not more "stable" than the other. However, this is not the case in other implementations of qubits, especially in atomic systems. States constantly evolve to go to the more stable ground state.

Since it is convenient to simulate quantum experiments that occur in other systems using optics, photon beams are artificially evolved to mimic the way atomic quantum systems work, and then experiments are performed. This was done in the [Entanglement Sudden Death](http://www.rri.res.in/quic/esdactivities.php) experiment at RRI.

The [Chershire cat](https://www.nature.com/scitable/blog/pop/the_quantum_cheshire_cat/) experiment attempts to separate the spin of a particle from its other properties. Yes, very much like the one in _Alice and Wonderland._

## Further reading?
- Fock states are a basis states for beams of photons. Read about Fock operators and spectral filters
- Understand everything in this post with the math now? 
- Storage: how exactly would a state be preserved in quantum optics, if everything is just shooting beams?

_Thank you to everyone at RRI for patiently explaining everything to me! Special thanks to [Ashutosh Singh]([https://sites.google.com/view/ashutoshsingh](https://sites.google.com/view/ashutoshsingh)) for reading through an older version of the post and suggesting edits._
