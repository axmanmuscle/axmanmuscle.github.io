---
title: "Making a Neural Network part 2: Network Structure"
last_modified_at: 2023-10-15
categories:
  - nn
tags:
  - machine-learning
  - math
---

This is the second in a series of posts about making a neural network from scratch in C++. Read the introduction [here](https://axmanmuscle.github.io/nn/making-a-neural-net-pt-1/)

## Network Structure 
I've started with a simple network to give us an idea of how this all works. We'll just have a single input, a single hidden layer with 2 nodes, and a single output.

I need to put a figure here so it's more clear what I'm talking about. Let's define a single input $x$, 2 neurons, and one output $y$. We'll use the identity for the activation function for right now. 

The first part of the network takes the input $x$ and multiplies it by each weight of the neurons. The interim value is now $$\hat{x} = \begin{bmatrix}w_{11}x \\ w_{12}x\end{bmatrix}$$ I'll write it like this so it's clear that each term is assigned to a different neuron --- this will make it more clear when we consider more neurons. 

We're not going to use an activation function for now, or else this is where it would apply. There are two more weights to get to $y$, so 
$$\begin{equation} \label{nneq}\tag{2} y = w_{21}w_{11}x + w_{22}w_{12}x\end{equation}$$
We can make this simpler (write it in matrix form) as
$$y = \begin{bmatrix}w_{21} & w_{22} \end{bmatrix}
\begin{bmatrix}w_{11}x \\ w_{12}x\end{bmatrix}$$

This is obviously excluding biases or activation functions. We can cover both of those later (maybe as I get more organized in my writing). 

### Loss
What does training our network actually mean? It means that we want to update the __weights__ of the network to minimize some *loss*. We're going to design our loss function so that the weights that minimize it give good performance on some set (usually called a _validation_ set).

A very common loss is mean-square error. Given some input-output pairs $(\tilde{x}, \tilde{y})$, the loss is
$$\begin{equation} l(\tilde{x}) = \frac{1}{2}\lVert f(\tilde{x}; \theta) - \tilde{y}\rVert_2^2 \label{mseeq}\tag{3}\end{equation}$$

### Derivatives
We can use $\ref{nneq}$ to write the actual derivatives now. 