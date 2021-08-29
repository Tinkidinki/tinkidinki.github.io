---
title: Fake Complex Derivatives
layout: post
usemathjax: true
tags: maths
---

Having reduced (a part of) the research problem I was working on to finding a gradient, I was just about to be relieved when I realised I had no clue how to find "gradients" in the space of Hermitian matrices I was working in. And the function I wanted to find the gradient for wasn't too simple looking either.

I turned to [math.stackexchange](https://math.stackexchange.com/a/3321634/287775) in despair, and turns out the gradient to my function was super simple and elegant! 

Now I owed it to complex matrix differentiation to go study that shit. Till the excitement lasts, atleast.

The answerer of the question suggested the book 'Complex-Valued Matrix Differentiation' which I started reading - hoping to cover up holes in complex differentiation and linear algebra on the way. 

So, first things first. The word for "differentiable" for complex valued functions of complex numbers is analytic. Before we get into finding derivatives, we need to check whether the function is analytic in the domain we're interested in. 

Taking our function $f(x + iy) = u + iv$, and ensuring that the derivative is the same when a point is approached from all directions gives us the Cauchy-Reimann equations:

$$\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y} \text{ , } \frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}$$

which happen to be a necessary and sufficient condition for a function to be analytic at a given point. 

### Aside: Changing independent variables changes derivatives - and we get to choose these!

Consider the following functions:
$$p(x,y) = x + y \\
q(x,y) = y$$

We can now write $x$ and $y$ in terms of $p$ and $q$:
$$x (p,q)= p - q \\
y(p,q) = q$$

Now, consider $\frac{\partial p}{\partial q}$:
$$ = \frac{\partial{(x + y)}}{\partial y} = 1$$
orrrr,
$$ = \frac{\partial p}{\partial x}\frac{\partial x}{\partial q} + \frac{\partial p}{\partial y}\frac{\partial y}{\partial q} = 0$$

So, which is the right partial derivative?

It turns out both are correct. In the first case, $x$ and $y$ are our independent variables, so $p$ turns out to be dependent on $q$. In the second case, $p$ and $q$ are considered independent variables, so the partial derivative is 0. 

So, these really depend on our choice of variables. Math, unlike CS sadly has no implication just because something is on the left or right of the $=$ sign, so we're left to specify this clearly when we're working in a system. 

### Real valued complex functions aren't differentiable :(

When we do matrix calculus, we want to be able to differentiate everything. Scalar functions of vectors, matrix functions of scalars, scalar functions of matrices -- everything.

However,  just take the simple case of a real valued complex function. This means $v$ is always 0, and the first Cauchy-Reimann equation itself is not satisfied unless $u$ is constant with respect to $x$. So, $\frac{d f}{d z}$ can't be found for arbitrary real valued function.

Perfect. The first thing we learn is we can't find derivatives of real valued functions, let alone the other grandiose plans involving matrices and vectors. 

### So, what do we do?

This is where we cheat. If we change the independent variables from $x$ and $y$ to $z$ and $z^*$, where now, 
$$x = \frac{z + z^*}{2} ,y = \frac{z - z^*}{2i}$$

Then we can take the partial derivative with respect to $z$ !
For instance for the real valued function $f = z^*z$, $\frac{\partial f}{\partial z}$ is just $z^*$.

The partial derivatives with respect to $z$ and $z^*$ are called formal derivatives -- which we have to satisfy ourselves with.

Tldr; we couldn't find the derivative with respect to $z$, so we changed the system such that $z$ became an independent variable, and made do with taking the partial derivative with respect to $z$.

### Aside: Derivatives for multivariable real valued functions?
One might ask, aren't real valued complex functions analogous to having a real 2-variable function? What exactly happens there?

Firstly, there is no one-big-derivative defined for such functions. We can take the partial derivative with respect to each variable, and put them together in a gradient vector to find the direction in which the function slopes the most, but there is no one 'number' that is the derivative.

And the gradient exists if all the partial derivatives exist.

Which is the fundamental difference between complex differentiation and the above - the atomic unit we're dealing with.

(You could still argue that the gradient is actually one complex number, I guess - still need to think about that!)

_Thanks to Siddharth Bhat for the intuition on changing independent variables and Jayitha for the reasoning about real valued functions._

