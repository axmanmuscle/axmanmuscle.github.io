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

The main part of the algorithm is the _factor graph_. This is just a fancy way to draw out how our marginal distributions are all related to each other. In terms of implementation of the algorithm, this is clearly the most important part. Understanding how to represent each type of node along with keeping track of the _messages_ and the _values_ and the _variables_ for each node is the most burdensome part of the algorithm.

The factor graph is called that because it takes the shape of a directed graph. The example from the paper (__which we'll be referring back to quite often__) has a factor graph given by the below picture. In this example, there are four players across three teams, with team 1 winning and teams 2 and 3 tying. ![Scenario from the original paper.](/assets/images/ts.png)

We'll talk about the layers of the factor graph from top to bottom.

### Prior Node

The top node of the factor graph is essentially the _player_ object --- there's one of these for each player in the game. It represents our _prior_ belief about the player's skill. We think about our priors as Gaussians (for more information on this, see [this](#) primer on Bayesian statistics), so this node is a mean and a standard deviation for each player. At the end of a run of the TrueSkill algorithm, this is what will be updated. These nodes are represented by the very top black square in the example image. Each of these nodes is associated with its normal distribution, written by $N(s_1 ; \mu_1, \sigma_1^2)$. This notation means that we're considering a _random variable_ $s_1$ which is governed by a _normal_ (or _Gaussian_) distribution with mean $\mu_1$ and variance $\sigma_1^2$.

This node is connected by a down arrow to a circle containing this variable $s_i$ for player $i$. This is the player's _skill_ (again, this is a random variable governed by a Gaussian distribution). This is in turn connected to the next rung of the factor graph.

### Performance Node

The performance node follows directly from the prior node for each player, as illustrated in the image above.

This is our first node that has more than one component --- we'll refer to a performance node having a _value_ and a _mean_. The (slightly confusing) part about this being a Bayesian setting is that __both the value and the mean are random variables__. This adds a bit of confusion for someone who hasn't dealth with Bayesian statistics before, but it's nice for many reasons. One is that it allows us to represent uncertainty in the mean, something difficult under classical (or _frequentist_) statistics. You can see from the image above that our _mean_ is actually the player's skill from the previous step. The value of this node is what we'll call a player's _performance_. We introduce a factor of $\beta$ here, which controls the variance of the performance distribution. This is a parameter up for selection, but the suggested value is $\sigma/3$. 

### Team Performance Node

The performance of all players on a team now get _added_ into a team performance value. "But Alex," you ask, "how do we add two things that are random variables?" That's an excellent question. In general, a random variable is associated with a function called its _probability distribution function_ (or pdf). The distribution of the sum of random variables is governed by the _convolution_ of their pdfs. 

In this case, our assumption that all variables so far have a Gaussian distribution pays dividends. The convolution of Gaussian pdfs stays a Gaussian. I may add a proof of this to a probability post here, but we're getting a bit far afield from the task at hand. Suffice it to say that this distribution stays Gaussian (with a relatively easy formula). This is best illustrated by the middle team of the above picture --- note how the third layer has a label that says $\mathbb{I}(t_2 = p_2 + p_3)$. This is probability speak for an _indicator variable_, but all that it's really saying is that the variable of the node $t_2$ is exactly the sum of $p_2$ and $p_3$. 

Our assumption up until now has been that all of these variables are Gaussians. Luckily the sum of Gaussians is also Gaussian, so we haven't violated that assumption (yet!).

### Team Difference Node
Now we get to the difference between teams -- basically, which team won (or, in the case of a game with multiple teams per match, what order did the teams place in). Each node here has two inputs (the two teams) and one output (the difference of the two teams distributions). 

Things get kind of funky here. This system is designed for games that allow ties --- chess, for example. The way to think about this in a Bayesian context is that a game is a tie (or two teams are tied) if the _difference in their distributions is less than some small number_ $\varepsilon$. A difference greater than $\varepsilon$ then corresponds to a "clean win" (or a team clearly placing above another). This is what the output of the team difference node and the very bottom level of the factor graph signify. 

This complicates things now. The difference of two Gaussian distributions, in general, is _not_ Gaussian. This becomes the hardest part of the factor graph to deal with since the marginal distributions are now non-analytical.

#### Outline
__Okay, here's the outline of the rest of the posts:__
- Part 2: Factor graphs
- Part 3: Update equations
- Part 4: Implementation
