---
title: "TrueSkill pt. 2: Factor Graphs"
last_modified_at: 2023-05-30
categories:
  - trueskill
tags:
  - trueskill
  - math
---

This is part 2 in a technical series about the algorithm TrueSkill. Part 1 (introduction) is [here](https://axmanmuscle.github.io/trueskill/trueskill-pt-1). You can see my source code [here](https://github.com/axmanmuscle/trueskill). You can also read the [the original paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2007/01/NIPS2006_0688.pdf).

## Factor Graph

The main part of the algorithm is the _factor graph_. This is just a fancy way to draw out how our marginal distributions are all related to each other. I'll leave the description of the factor graph to [the original paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2007/01/NIPS2006_0688.pdf). In terms of implementation of the algorithm, however, this is clearly the most important part. Understanding how to represent each type of node along with keeping track of the _messages_ and the _values_ and the _variables_ for each node is the most burdensome part of the algorithm.

The factor graph is called that because it takes the shape of a directed graph. The example from the paper (__which we'll be referring back to quite often__) has a factor graph given by the below picture. In this example, there are four players across three teams, with team 1 winning and teams 2 and 3 tying. ![alt text](/assets/images/ts.png)

#### Outline
__Okay, here's the outline of the rest of the posts:__
- Part 2: Factor graphs
- Part 3: Update equations
- Part 4: Implementation
