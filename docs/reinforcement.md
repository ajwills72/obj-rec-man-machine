---
layout: page
title: Reinforcement Learning
subtitle: Notes on Sutton & Barto (2020).
---

A learning agent must be able to sense the state of its environment to some
extent and must be able to take actions that affect the state.

The agent also must have a goal or goals relating to the state of the
environment.

Reinforcement learning isn't supervised learning, it is learning from your own experience.

Reinforcement learning isn't unsupervised learning, either; it is trying to maximize a reward signal rather than trying to find structure in unlabelled data. 

Reinforcement learning is a _third_ machine learning paradigm.

Exploration / exploitation is a challenge in reinforcement learning, and not in other kinds of learning. 

"A complete, interactive, goal-seeking agent" (there can be multiple agents in one robot)

"Without rewards there could be no values, and the only purpose of estimating values is to
achieve more reward. Nevertheless, it is values with which we are most concerned when
making and evaluating decisions."


"For example, some textbooks have used the term 'trial- and-error' to describe
artificial neural networks that learn from training examples. This is an
understandable confusion because these networks use error information to update
connection weights, but this misses the essential character of trial-and-error
learning as selecting actions on the basis of evaluative feedback that does not
rely on knowledge of what the correct action should be."

"The origins of temporal-di↵erence learning are in part in animal learning
psychology, in particular, in the notion of secondary reinforcers. A secondary
reinforcer is a stimulus that has been paired with a primary reinforcer such as
food or pain and, as a result, has come to take on similar reinforcing
properties. Minsky (1954) may have been the first to realize that this
psychological principle could be important for artificial learning systems.
Arthur Samuel (1959) was the first to propose and implement a learning method
that included temporal-di↵erence ideas, as part of his celebrated
checkers-playing program"

## Inspired by this book

- Learning as an efficiency upgrade for evolution.

- "If the step size property is reduced appropriately over time" : How does the system know how to do this?
