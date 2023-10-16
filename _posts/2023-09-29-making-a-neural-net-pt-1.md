---
title: "Making a Neural Network"
last_modified_at: 2023-09-29
categories:
  - nn
tags:
  - machine-learning
  - math
---

## Introduction
This is the first in a series of posts in which I will be making a neural network from scratch, along with the associated training infrastructure. This is mainly for my own gratification and to put this work somewhere out there --- maybe someone else can get something out of this as well.

### Where to Begin
First thing to do is to start writing a structure for the network itself. This is going to be a fully connected network to start. Let's assume that each layer has the same number of neurons - then our weights matrix is simply a $n \times m$ matrix, where we have $n$ weights and $m$ layers.

I'm going to try to be careful about writing this nicely because this will become (I'm sure) several articles that I'll publish to my website.

### Problem Setup
To begin, we're going to use a (very simple) neural network to estimate $f(x) = x^2$.  This means we're going to start with pairs of numbers like
$$(2, 4), \, (4, 16), \, (2.5, 6.25), \ldots$$
where the first number $x$ is the input to the network and the second number $y := f(x)$ is the training data. "Training data" what do I mean by this? Let's start by talking about what "training" a neural network actually means.

![nn1](https://raw.githubusercontent.com/axmanmuscle/axmanmuscle.github.io/gh-pages/assets/nn1.png)

#### Training 
Let our network be a function $N(x)$. With our current architecture, this is taking in a scalar $x$ and outputting a scalar $N(x)$, so we'll write $N : \mathbb{R} \to \mathbb{R}$. This is not always the case (in fact is rarely the case) in actual machine learning. Usually our input is a vector or a matrix (or sometimes a tensor), and the output can be a vector or a matrix as well. But, this simple case will illustrate what training means.

We'll use a loss function called $L$. In this case, $L$ takes in the output of the network and its associated training data and gives us the "distance" between these two. A very common loss function is mean-squared error (or MSE):
$$\label{eq:mse} L(y, \hat{y}) = \frac{1}{2}\lVert y - \hat{y} \rVert_2^2 \tag{1}  $$ 
The double bar means we're computing a _norm_. Since our current network is just scalars, this will just be the absolute value. [^1]
To make $\eqref{eq:mse}$ a little more clear (or less clear, depending...) I'll write
$$\hat{y} := N(x\,;\,\theta)$$
is the output of the network $N$ with input $x$ and parameters $\theta$. I'm writing it like this to show why $\eqref{mse}$ allows us to update the _parameters_ (things like the weights and biases) of our network $N$.

[^1]: Since we're squaring the difference, we actually don't need the absolute value. This is the common _least squares_ problem, usually introduced in a statistics class for things like fitting a line to data.

#### Derivatives
Now let's think about the derivatives of our loss function _with respect to our parameters_ $\theta$. Why $\theta$? We want to minimize $\eqref{mse}$ with respect to our parameters - that's what we have control over. So let's start writing out what our network response function is in term of our parameters. This is going to require a more intense description of our network. Let's leave this for the next post.