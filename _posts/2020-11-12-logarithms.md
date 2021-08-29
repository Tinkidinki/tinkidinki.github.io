---
title: Logarithms
layout: post
tags: all-posts maths
usemathjax: true
---

The following is a nice way to think about logarithms: 
$$\log_b a \equiv$$ The number of times $$a$$ is divided into $$b$$ parts such that each part becomes $$1$$. 

For example, $$1000$$ needs to be divided into 10 parts $$3$$ times to get to 1, so $$\log_{10}1000 = 3$$.

### Reaching arbitrary numbers by repeated division

Now, we may ask---how many times should $$a$$ be divided into $$b$$ parts to reach an arbitrary number $$x$$? We can split $$a$$ reaching $$1$$ to $$a$$ reaching $$x$$ and then $$x$$ reaching $$1$$.

Let $$k \equiv$$ The number of times $$a$$ is divided into $$b$$ parts to reach $$x$$. 

We have, 
$$
\log_b a = k + \log_b x \\
\implies k = \log_b a - \log_b x = \log_b \left (\frac a x \right )
$$

### Intuition for the change of base rule
We will use this to build intuition for the following identity: 
$$ \log_qa = \log_pa . \log_qp$$

Informally, the above asks, 
how do we relate the number of times we divide $$a$$ in $$q$$ parts to reach $$1$$ and the number of times we divide $$a$$ in $$p$$ parts to reach $$1$$?

Let us look at the first step of calculating $$\log_a p$$. We divide $$a$$ into $$p$$ parts, and each part is of size $$a/p$$. Just to perform this first step, if I was only allowed to divide by $$q$$, how many divisions would I need to reach $$a/p$$ ?

Well, this is the question we asked in the previous section!
The number of divisions is equal to $$\log_q \frac{a}{a/p} = \log_q p$$. Notice this is independent of $$a$$, so this holds true to reach any $$x/p$$ from $$x$$, in other words, it holds for every step of the calculation of $$\log_a p$$. 

So, we have $$\log_q p$$ divisions [in the process where you only divide by $$q$$ to reach $$1$$] for each of the $$\log_p a$$ divisions [in the process where you only divide by $$p$$ to reach $$1$$].

Which gives us:
$$\log_q a =  \log_p a. \log_q p$$

Yayyy!

I would love to be able to formalize the intuition I have here, and also extend it for other logarithm identities. 
