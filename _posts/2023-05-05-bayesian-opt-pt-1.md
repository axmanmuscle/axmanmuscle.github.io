---
title: "Bayesian Optimization Pt. 1"
date: 2023-05-05T15:00:00-06:00
categories:
  - blog
  - math
  - bayes
tags:
  - Jekyll
  - update
  - Bayesian
---

_Let's discuss Bayesian Optimization. This will be the first in a string of technical posts._

## Introduction
Bayesian optimization is a technique for optimizing black-box functions that are expensive to evaluate and for which we have no gradient (in many cases, the gradient doesn't even exist). Lately, this has typically been used in the context of hyperparameter tuning of machine learning models. The main idea behind Bayesian optimization is to use a probabilistic model to approximate the unknown function being optimized and to use this model to guide the search for the optimum.

The most commonly used probabilistic model is called a __Gaussian process__ (GP). GP's are straightforward extensions of a typical Gaussian (or normal) distribution. More specifically, these are _stochastic processes_ where each _realization_ is a Gaussian distribution. Don't worry, that's not really important for understanding what's going on here. Just imaginge it's an infinite dimensional probability distribution (as if that's any easier).
