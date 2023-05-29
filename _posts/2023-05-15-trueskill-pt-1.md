---
title: "TrueSkill pt. 1"
date: 2023-05-15T15:00:00-06:00
categories:
  - blog
  - math
  - trueskill
tags:
  - Jekyll
  - update
  - trueskill
---

This is a technical post about the algorithm TrueSkill. You can see the source code [here](https://github.com/axmanmuscle/trueskill).

## Introduction
Trueskill is a Bayesian skill rating system developed by Bungie and Microsoft for Xbox live. It was developed because the Elo algorithm (commonly known in chess) was insufficient for the types of rating computations that are needed for matchmaking in online video games. Elo takes a nice update approach based on the difference between the two players' Elo ratings going into the match.

## Elo

In 1959, Arpad Elo developed a statistical rating system for chess, which was adopted by the World Chess Federation (FIDE) in 1970. The key idea behind the _Elo system_ is to model the probability of the possible game outcomes as a function of the two players' skill ratings $s_1$ and $s_2$. In a game, each player $i$ exhibits performance $p_i \sim N(p_i ; s_i, \beta^2)$ normally distributed around their skills $s_i$ with fixed variance $\beta^2$. The probability that player 1 wins is given by the probability that his performance $p_1$ exceeds the opponent's performance $p_2$:

$$
P(p_1 > p_2 \, \vert \, s_1, s_2) = \Psi\left(\frac{s_1 - s_2}{\sqrt{2}\beta}\right)
$$

where $\Psi$ denotes the cumulative probability density of a zero-mean unit-variance Gaussian.

## Bayesian Statistics
There are a million words written about Bayesian statistics elsewhere on the internet, written by people much smarter and more eloquent than me. The main result from the Bayesian formulation of statistics is _Bayes' rule_: 

$$
P(A \, \vert \, B) = \frac{P(B \, \vert \, A) P(A)}{P(B)}.
$$

__This maybe deserves its own set of posts__.

## TrueSkill

TrueSkill uses a _Bayesian_ framework for updating our belief in a player's skill. What does this mean from a technical perspective? Really, it means that our belief in each player's ability is modeled as a _Gaussian_ (or _normal_) distribution. We use the information from the game to update this Gaussian distribution using _Bayes rule_.

### Factor Graph

The main part of the algorithm is the _factor graph_. This is just a fancy way to draw out how our marginal distributions are all related to each other. I'll leave the description of the factor graph to [the original paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2007/01/NIPS2006_0688.pdf). In terms of implementation of the algorithm, however, this is clearly the most important part. Understanding how to represent each type of node along with keeping track of the _messages_ and the _values_ and the _variables_ for each node is the most burdensome part of the algorithm.

The factor graph is called that because it takes the shape of a directed graph. The example from the paper (__which we'll be referring back to quite often__) has a factor graph given by the below picture. In this example, there are four players across three teams, with team 1 winning and teams 2 and 3 tying. ![alt text](/assets/images/ts.png)

#### Outline
__Okay, here's the outline:__
- Factor graph
  - Prior nodes
  - Likelihood nodes
  - Sum/difference nodes
  - Truncation nodes
- Update equations
- Messages vs. Value
- Graph structure
- Actual implementation?