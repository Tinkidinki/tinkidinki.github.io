---
title: Yet Another Backpropagation Blog Post
layout: post
tags: all-posts machine-learning
usemathjax: true
---

At this point, there are way too many backprop posts on the net, including another article titled [Yet Another Introduction to Backpropagation](https://www.kamperh.com/notes/kamper_backprop17.pdf). I add another one to the pile as proof to myself that I have understood this reasonably well, and perhaps something in here may help out someone who was stuck with backprop, _and_ wandered onto this blog for help. (So, yes, just the former reason.)

 Backpropagation is a more general process, and I cover here only backpropagation for a feedforward neural network.  Note that the [Wikipedia](https://en.wikipedia.org/wiki/Backpropagation) page on backpropagation was the clearest to me, and this post is largely derived from that one. This post assumes you have already attempted to grok backprop at least once and covers only the nuances of the operation, and is not meant for a high level overview. 

## Prerequisites
* Matrix differentiation: I recommend [this](https://explained.ai/matrix-calculus/) post which is long but worthwhile.  
* Numerator and denominator convention: This is part of matrix differentiation, but especially important. The [Wikipedia page](https://en.wikipedia.org/wiki/Matrix_calculus) covers it well, and also succintly states an issue with the conventions:  _Authors of both groups (numerator and denominator convention) often write as though their specific convention were standard._
* High-level overview of how a feed-forward neural network works. 
* Understanding of why the negative of the gradient of a function at a point is the direction of its steepest descent.


## Notation
Having sensible notation clears out a lot of issues in the understanding of backprop (or a lot of other concepts, but more so here because of the oh-so-many vectors and indices and matrices)

For the rest of this article, consider a neural network with $$n+1$$ layers, labelled $$0$$ to $$n$$. Layer $$0$$ is the input layer, and layer $$n$$ is the output layer. So, excluding the input and output layers, our network has $$n-1$$ layers. We follow 0-based convention just to make it easier while coding this up. 

Every layer $$i$$ is associated with two $$l_i$$--length vectors ( or $$l_i \times 1$$ matrices ) $$z_i$$ and $$a_i$$.  Let us whip out the feedforward equations to see how they are related (Also look at the diagram below):

$$
z_{i+1} =  W_i \times a_i\\
a_i = \sigma(z_i)
$$

A few things to remember:
* Non-linear function $$\sigma$$ acts on $$z_i$$ of a layer and produces $$a_i$$ (of the same layer, as shown by the index). Although we write it here as acting on a vector and producing a vector, the function (for example, sigmoid) that we talk about in this article acts on a scalar and produces a scalar. When written as shown above, we actually mean:

$$a_i = \begin{bmatrix} \sigma(z_i^0) \;\;\; \sigma(z_i^1) \;\;\; ... \;\;\; \sigma(z_i^{l_i - 1})\end{bmatrix}$$

* $$W_i$$ is a matrix (the weight matrix) that acts on  vector $$a_i$$ (weight matrices have been labelled based on the **vector they act upon**) and produces $$z_{i+1}$$ of the next layer. 
* The dimensionality of matrix $$W_i$$ is therefore equal to $$l_{i +1} \times l_i$$
* $$a_0$$ is equal to the input vector, and that is the starting point of our recurrence. $$z_0$$ is never used. 
* $$z_n$$ is the output vector, and the end of our recurrence. $$a_n$$ is never used. 
* Note that we have not yet labelled any elements within each of the layer-vectors or connecting weight matrices. 

Below is a schematic diagram for $$4$$ layers labelled $$0$$ to $$3$$. (Here, $$n$$ = 3)

$$
\underbrace{
\begin{bmatrix}
. \\
. \\
.
\end{bmatrix} }_{z_0, a_0: \;l_0 \times 1}
 \,\,\, \,\underbrace{W_0 }_{l_1 \times  l_0}\,\,\,\,
\underbrace{
\begin{bmatrix}
. \\
. \\
.\\
.
\end{bmatrix} }_{z_1, a_1: \;l_1 \times 1}   
 \,\,\, \,\underbrace{W_1 }_{l_2 \times  l_1}\,\,\,\,
\underbrace{
\begin{bmatrix}
. \\
. \\
. \\
. \\
. \\
\end{bmatrix}}_{z_2, a_2: \;l_2 \times 1}  
 \,\,\, \,\underbrace{W_2 }_{l_3 \times  l_2}\,\,\,\,
\underbrace{
\begin{bmatrix}
. \\
. \\
\end{bmatrix}}_{z_3, a_3: \;l_3 \times 1}
$$

Eventually, we will also need to refer to the elements within each vector and matrix. Since the subscripts are all used up for referring to the vectors and matrices, we will use superscripts for elements. The elements also follow the same 0-based indexing as the vectors and matrices. Do not confuse these for powers. We _never_ use powers in this article, (except for the error, and there it is too clear to be confused).

For example, $$z_0^2$$ refers to the third element of $$z_0$$. $$W_2^{13}$$ refers to the element in the second row and fourth column of $$W_2$$. We will never go as far as two-digit numbers so $$13$$ in the superscript should never be confused as thirteen. 

Having the matrix indices as subscripts and element indices as superscripts is probably opposite to usual convention, but this is useful, as when we refer to matrix transposes, we don't refer to elements, so that allows us to write $$T$$ in the superscript and the matrix index in the subscript as in $$W_2^T$$.

## Setting up the calculations
Let us assume for now that we have a training set with only one data point, that is, we have been given one input vector $$(x)$$ and one output vector $$(y)$$. We will later generalise to a training set with multiple data points. 

The error $$(E)$$ is a function that indicates how different the output vector of the neural network $$(z_n)$$  is from the expected output $$(y)$$. Here, we define it as as $$(y - z_n)^2$$.

The goal at the end of training is to figure out the right weight matrices $$\{W_i\}$$ that minimise this error. 

The goal at the end of backpropagation is to find the changes that need to be made to each weight matrix to get closer to the right weight matrix. 

Because the direction of steepest descent of a function is given by the negative of the gradient, the change $$dW_i$$ we add to the weights is equal to a multiple of the negative of the gradient of $$E$$ with respect to $$W_i$$.
Therefore we have, 

$$dW_i = -m.\frac{\partial E}{\partial W_i}$$

$$m$$ is a hyperparameter that is called "learning rate" which we will worry about later. How does the right hand side of the above equation look? We have one scalar $$E$$ whose derivative we take with respect to $$W_i$$, so the total number of elements in our derivative is equal to the total number of elements in $$W_i$$. 

However, there are two different ways of taking the derivative of a scalar with respect to a matrix, and both will leave you with different dimensionalities. 

The numerator convention ensures that $$dW_i$$ has the same dimensions as $$W_i^T$$. The denominator convention ensures that $$dW_i$$ has the same dimensions as $$W_i$$ (and all the corresponding elements are correct so we can just add $$dW_i$$ to $$W_i$$ to get our new $$W$$. **For this step only**, we will use the denominator convention, since it is so convenient. 

So, to accomplish backpropagation, it is sufficient to compute $$\frac{\partial E}{\partial W_i}$$ for every $$i$$ from $$0$$ to $$n-1$$ (Reminder: There are $$n$$ weight matrices and $$n+1$$ layers. There is no need for a weight matrix to act on the last layer.)

## Writing down the chain rule

We will write down the $$\frac{\partial E}{\partial W_i}$$ for every matrix in the example with $$n=3$$ drawn above. Let's put down the relevant equations here once again:

$$
z_{i+1} =  W_i \times a_i\\
a_i = \sigma(z_i) \\
E = (y - z_n)^2
$$

As an aside, before we start, it helps to remember that the training data is the set of constants that we have and the weights are the variables that we're changing. This is easy to forget, especially because we use $$x$$ and $$y$$ for the training data. 

Let us write down the above equations explicitly for every index. 

$$\begin{aligned}
E &= (y - z_3)^2 \\
z_3 &= W_2 \times a_2 \\
a_2 &= \sigma(z_2) \\
z_2 &= W_1 \times a_1 \\
a_1 &= \sigma(z_1) \\
z_1 &= W_0 \times a_0 \\
a_0 &= x
\end{aligned}$$

We will just write the derivatives by the chain rule (keeping aside dimensionality for now)

$$
\begin{aligned}
\frac{\partial E}{\partial W_2} &=  \frac{\partial E}{\partial z_3} \times \frac{\partial z_3}{\partial W_2}\\
\frac{\partial E}{\partial W_1} &=  \frac{\partial E}{\partial z_3} \times \frac{\partial z_3}{\partial a_2} \times \frac{\partial a_2}{\partial z_2} \times \frac{\partial z_2}{\partial W_1} \\
\frac{\partial E}{\partial W_0} &=  \frac{\partial E}{\partial z_3} \times \frac{\partial z_3}{\partial a_2} \times \frac{\partial a_2}{\partial z_2} \times \frac{\partial z_2}{\partial a_1} \times \frac{\partial a_1}{\partial z_1} \times \frac{\partial z_1}{\partial W_0} \\
\end{aligned}
$$

There are a few things to notice now:
- We can see that if there were more layers, the same pattern would follow for all the weight matrices, so whatever we do for the above generalises to more layers. 
- We notice that there is a repetition in terms that are calculated: The first term calculated for the $$W_2$$ expression is the same as the first term calculated for the $$W_1$$ expression. The first three terms calculated for the $$W_1$$ expression are the same as the first three terms for the $$W_0$$ expression. This repetition would continue were we to add more layers. 
- The LHS of each side, we already decided should have the dimensionality of $$W_i$$ so that we can just add it to $$W_i$$. 
- Let us take a look at the chain rule terms: 
-- The first term $$\frac{\partial E}{\partial z_3}$$ is the derivative of a scalar with respect to a vector. In numerator convention, this gives us a matrix of the same dimensions as $$z_3^T$$, that is $$1 \times l_3$$.
-- Look at the term $$\frac{\partial z_3}{\partial a_2}$$. In numerator convention, the dimensionality of this term is $$l_3 \times l_2$$.
-- Leave out the last term in each expression (the derivative with respect to $$W_i$$), and we notice that assuming numerator convention, the chain rule works out perfectly -- that is, the dimensions of each term make the expression compatible for multiplication. This is no surprise, since it is the whole point of the convention. 
- An issue you probably can notice is the bad-looking last term, which is a derivative of a vector with respect to a matrix, which is not defined well in terms of vectors or matrices. 

As a summary, for each expression, we have (i) the first few terms multiplying out to give a nice derivative, (ii) a last term hat we don't know how to express, and (iii) everything needs to be put together in denominator convention so that we can add it to $$W_i$$. 

Let us deal with these issues one by one. 

## _Really_ digging into the specifics
Just so that we can have atleast one constant amongst all these variable indices in our discussion, we will only run over the specifics for calculating $$\frac{\partial E}{\partial W_0}$$. The derivatives with respect to the other weight matrices follow exactly the same procedure. 
### (i) The nice derivative from the first few terms:
Consider multiplying all terms but the last one for $$\frac{\partial E}{\partial W_0}$$. This is the expression for $$\frac{\partial E}{\partial z_1}$$. And since we followed numerator convention while calculating this, we know the final multiplied value has dimensions $$1 \times l_1$$. It is the following row matrix:

$$\frac{\partial E}{\partial z_1} = \begin{bmatrix} \frac{\partial E}{\partial z_1^0} \;\;\; \frac{\partial E}{\partial z_1^1} \;\;\;  \frac{\partial E}{\partial z_1^2} \;\;\;  ... \;\;\; \frac{\partial E}{\partial z_1^{l_1-1}} \end{bmatrix}$$

### (ii) That pesky last term: $$\frac{\partial z_1}{\partial W_0}$$
The derivative of a vector with $$l_1$$ terms with respect to a matrix with $$l_1 \times l_0$$ terms will have $$l_0 \times l_1 \times l_1$$ terms. There is no well defined matrix to capture this derivative. What we will do here is _not_ calculate this last term at all, and instead try to see if there are any observations for this particular problem that help us get around calculating it. 

### (ii) Putting everything together

Forget the last term for now. Let's look at the LHS $$\frac{ \partial E}{\partial W_0}$$:

$$E = f(z_1,  ...)$$

$$z_1 = W_0 \times a_0$$

Although $$E$$ depends on other terms, those terms don't depend on $$W_0$$ and are hence irrelevant for our below partial derivative. This is what each term of our partial should be:

$${\frac{ \partial E}{\partial W_0}}^{jk} = \sum_i \frac{\partial E}{\partial z_1^i} \frac{\partial z_1^i}{\partial W_0^{jk}}$$

There are two terms in the RHS. In each term of the summation, the first partial we already have from the vector of the previous section. The second partial, we don't know. But what _is_ the second partial? Since $$z_1 = W_0 \times a_0$$, we know that each term $$z_1^i = \sum_r W_0^{ir}a_0^r$$. This implies:

$$\frac{\partial z_1^i}{\partial W_0^{jk}} = a_0^k \delta_{ij}$$

And since the summation for $${\frac{ \partial E}{\partial W_0}}^{jk}$$ runs through every $$i$$, and we only get a non-zero value when $$i=j$$, we have:

$${\frac{ \partial E}{\partial W_0}}^{jk} = \frac{\partial E}{\partial z_1^j} a_0^k$$

Phew! That was simplified quite a bit!
Now, how do we get the above by multiplying matrices we already have? This is not too hard:

$${\frac{ \partial E}{\partial W_0}} = {\left(\frac{\partial E}{\partial z_1}\right)}^Ta_0^T$$

That's it!

### (iv) In summary (of this section)
To calculate $${\frac{ \partial E}{\partial W_i}}$$, write down the chain rule expression as shown in the _Writing down the chain rule_ section above. Calculate each term following numerator convention. Multiply all but the last term to get $$\frac{\partial E}{\partial z_{i+1}}$$ Finally multiply its transpose with $$a_i^T$$ to get $${\frac{ \partial E}{\partial W_i}}$$.

## The in-between calculations
In the previous section, for the first few terms, we just said _calculate them using numerator convention_ but we didn't specify how to do them. Go back up to the _Writing down the chain rule_ section and notice that apart from the first and the last term, there are basically two kinds of derivatives: $$za$$ and $$az$$. 
To calculate an $$az$$ derivative:


$$
\partial {\frac{\partial a_i}{{\partial z_i}}}^{jk} = \frac{\partial \sigma(z_i^j)}{{\partial z_i^k}} = \sigma'(z_i^j)\delta_{jk}
$$

Therefore, 
$$\frac{\partial a_i}{{\partial z_i}} = diag( \begin{bmatrix}\sigma'(z_i^0) \;\;\;\sigma'(z_i^1) \;\;\; ... \;\;\;  \sigma'(z_i^{l_i-1})\end{bmatrix}$$

To calculate a $$za$$ derivative:
We have, 
$$z_{i+1} = W_i \times a_i$$
Therefore, 
$$z_{i+1}^{j} = \sum_r W_i^{jr}a_i^r$$
and
$${\frac{\partial z_{i+1}}{\partial a_i}}^{jk} =  W_i^{jk}$$
So, we have:
$$\frac{\partial z_{i+1}}{\partial a_i}=  W_i$$

That's it! We have the $$za$$ and the $$az$$ derivatives, and now we're good to go!

## The Dynamic Programming Portion
It is worth noting that if you do calculate the expressions from the $$n-1^{th}$$ index to the $$0^{th}$$ index, then you can use a portion of the previous solution to help out with the next solution, and hence it makes sense doing the calculations backward -- which is where the name backpropagation originates from. 

## More than one data point
We do hope you're going to use this for a dataset of a size larger than one. Of course, if that's your way of guaranteeing 0 error, more power to you. However for $$t$$ training samples, you should apply backpropagation for each training sample and add up the $$dW_i$$ you get for each training sample. Remember that the each $$dW_i$$ is obtained by multiplying the learning rate $$m$$ with the negative of the gradient which we saw how to calculate above. 
$$dW_i^{total} = dW_i^0 + dW_i^1 +... +  dW_i^{t-1}$$
We then update the weights by adding $$dW_i$$ to every weight $$W_i$$ and repeat the process. 
## Finally, the code
Code implementing the above, with documentation and testing examples, can be found in [this github repository](https://github.com/Tinkidinki/neural-network). Feel free to comment here or send PR's on the repo! 

