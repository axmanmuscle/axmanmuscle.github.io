---
title: "TrueSkill pt. 1: Introduction"
last_modified_at: 2023-05-30
categories:
  - trueskill
tags:
  - trueskill
  - math
---

This is an introductory post about the algorithm TrueSkill. You can see my source code [here](https://github.com/axmanmuscle/trueskill). You can also read the [the original paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2007/01/NIPS2006_0688.pdf).

## Elo

In 1959, Arpad Elo developed a statistical rating system for chess, which was adopted by the World Chess Federation (FIDE) in 1970. The key idea behind the _Elo system_ is to model the probability of the possible game outcomes as a function of the two players' skill ratings $s_1$ and $s_2$. In a game, each player $i$ exhibits performance $p_i \sim N(p_i ; s_i, \beta^2)$ normally distributed around their skills $s_i$ with fixed variance $\beta^2$. The probability that player 1 wins is given by the probability that his performance $p_1$ exceeds the opponent's performance $p_2$:

$$
P(p_1 > p_2 \, \vert \, s_1, s_2) = \Psi\left(\frac{s_1 - s_2}{\sqrt{2}\beta}\right)
$$

where $\Psi$ denotes the cumulative probability density of a zero-mean unit-variance Gaussian.

After the game, the skill ratings $s_1$ and $s_2$ are updated such that the observed game's outcome becomes more likely, and the combined skill rating $s_1 + s_2$ remains constant. Suppose we set $y = +1$ if player 1 wins, $y = -1$ if player 2 wins, and $y = 0$ if it's a draw. The resulting (linearized) Elo update is given by $s_1 \leftarrow s_1 + y\Delta$, $s_2 \leftarrow s_2 - y\Delta$, with

$$
\Delta = \alpha \beta \sqrt{\pi}\left(\frac{y+1}{2} - \Psi\left(\frac{s_1 - s_2}{\sqrt{2\beta}}\right)\right).
$$

Elo is a fine system for two-player games. It becomes a bit more challenging to imagine an extension of this system for multiple teams of multiple players. There's some challenges with online games:

1. The outcomes of games are usually based on _team_ performance instead of individual performance. 
2. When multiple teams compete the outcome is a permutation of the teams involved instead of simply a winner and a loser.
3. Over time, arbitrary teams of arbitrary players compete, making it difficult to isolate an individual player's performance.

TrueSkill as a system is designed to address these challenges.

## TrueSkill

TrueSkill uses a _Bayesian_ framework for updating our belief in a player's skill. What does this mean from a technical perspective? Really, it means that our belief in each player's ability is modeled as a _Gaussian_ (or _normal_) distribution. We use the information from the game to update this Gaussian distribution using _Bayes' rule_.

The idea behind the algorithm is relatively simple. 
  - We model each player's skill as a Gaussian random variable.
  - Each player's _performance_ in a game is a random draw from a Gaussian distribution whose mean is the player's skill.
  - Each team's performance is the sum of performance of the players on that team. 
  - We update each team's distribution based on the actual performance from the game.
  - We update the player's performance based on the team performance, and the player's skill based on their performance.

This is the main idea --- everything else is just implementation details.

#### Outline
__Okay, here's the outline of the rest of the posts:__
- Part 2: Factor graphs
- Part 3: Update equations
- Part 4: Implementation
