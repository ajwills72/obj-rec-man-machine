---
layout: page
title: Reinforcement Learning
subtitle: Notes on Sutton & Barto (2020).
---

## Notes on the book


### Chapter 1

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

#### Inspired by Chapter 1

- Learning as an efficiency upgrade for evolution. I made and put this statement on Twitter, and got a couple of nice references in return: (1) a simulation by [Hinton & Nowlan (1987)](https://cogsci.ucsd.edu/~rik/courses/cogs184_w10/readings/HintonNowlan97.pdf) which is an illustration of how learning can lead to a smoother search space for evolution. (2) [Heyes, Chater & Dwyer (2020)](https://users.ox.ac.uk/~ascch/Celia's%20pdfs/3%20Heyes,%20Chater%20&%20Dwyer%202020.pdf) which argues the changes are more likely to be on peripheral than central cognitive mechanisms. This in turn led to a bit of a rabbit hole, from which emerged this cool study using Bayesian estimation methods of the [development of Indo-European languages](http://ararat-center.org/upload/files/grayatkinson2003[1].pdf), which seeme do diverge from Hittite around 8,700 years ago. 

- Chapter 1: "If the step size property is reduced appropriately over time" : How does the system know how to do this? Answer becomes apparent from reading chapter 2: It's a per-action step-size, and it can know how many times it has taken the action. 

### Chapter 2

RL vs. Supervised -> Evaluative versus instructive feedback

"If you have many time steps ahead on which to make action selections, then it
may be better to explore the nongreedy actions and discover which of them are
better than the greedy action. Reward is lower in the short run, during
exploration, but higher in the long run because after you have discovered the
better actions, you can exploit them many times." ... "In this book we do not
worry about balancing exploration and exploitation in a sophisticated way; we
worry only about balancing them at all." - Beautiful piece of life advice.

**Optimistic initial values** for expected reward can lead to exploration, even where epsilon = 0, because the system will be 'disappointed' with the reward received from all options. It's a neat trick but it's not so great in non-stationary problems (because you never get the initial conditions back). 

Another alternative to e-greedy is called UCB (**upper confidence bound** estimation). What it does is add a term to each value in the argmax selection. That term goes up with the logarithm of t (number of times any action has been taken), but down with the number of times the specific action has been taken. So it bascially is a bias to explore the relatively unexplored, rather than to explore randomly. The exact form of the equation seems like a hack to me (i.e. there are many ways this basic idea could have been implemented, it's not clear why this particular formulation was chosen). It also turns out to be tricky to adapt for nonstationary problmes, and apparently it's not practical for the methods we use in Part II for large state spaces. So, let's perhaps draw a veil over that one.

Another approach does not estimate the reward of each action directly, but learns preferences (evidence terms) which produce actions probabilistically, and whose values change as a function of how much better or worse the current reward was than the average reward so far across all actions (with a decelerating function as the probability of choice approaches 1. This is applied in reverse for the actions not taken. With some maths that I skimmed, looks like you can demonstrate this is a form of stochastic gradient descent. These are thus known as **gradient bandits**.

An intermediate step to a full RL problem is one where there are two k-armed bandit problems, and the problem you are currently faced with is signalled by a cue. You have to learn a _policy_ here, but it's still the case the each action affects only the immediate rewwards. What makes a full RL problem is where the action is allowed to affect the next situation as well as the reward. That's where the book is going after this chapter.

#### Inspired by Chapter 2

- Is supervised learning _ever_ the right answer? I keep coming back to this throughout my career (e.g. Wills & McLaren, 1998) but CNNs are supervised. Is that plausible? Can we use RL to build vision? How about in a simulated environment?

- Pavlovian procedures (and most category learning expts) take the form of supervised learning. Operant is RL. 

- Profound connection between error-correcting learning with decaying learning rate, and online, low-memory, veridical computation of an average. Getting the decay rate "wrong" gives us recency/primacy effects. 

- Equally profound, if the world is non-stationary, using a fixed learning rate gives us a recency effect, which is adaptive. In fact, it's exponential recency-weighted average. 

- And, with fixed learning rate, the initial value _always biases_ the average (a primacy effect), it's just the bias gets smaller with time. It's not clear to me whether having a primacy effect is a problem, one could think of initial values as representing potentially valid pre-experience priors. Thing is though, you then have to specify these. You might be better not doing so in some cases, in which case if you use a very specific decaying step size (Eq 2.8, 2.9) you can get an exponential recency-weighted average without initial bias. 

- (Section 2.9) - Perception increases reward because it allows you to go beyond treating the world as a k-armed bandit problem. It provides _context_. 
