# Week 3: Policy Gradient Methods

So far the agent has always learned a *value* - `Q(s,a)` - and then acted greedily on
it. This week it learns the **policy directly**: a neural network that takes a state
and outputs a probability for each action, trained by pushing up the probability of
actions that led to good outcomes.

This is also the week the course leaves the lookup table behind. Week 2 ended on a
wall: a Q-table cannot generalise between similar states, and the state space
multiplies with every piece of the problem you add. CartPole has a *continuous* state
space - there is no table to build.

## Learning Objectives

By the end of this session, you will be able to:
- Explain the difference between learning a value function and learning a policy
- Describe what the policy gradient does to action probabilities, and why
- Implement a stochastic policy network and sample actions from it
- Compute discounted returns and the REINFORCE loss
- Train an agent to solve CartPole from scratch
- Explain why policy gradients are high-variance, and how a baseline helps

## Materials

### Worked Example: Policy Gradients
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week3/example_policy_gradient.ipynb)

Build-up notebook: value-based vs policy-based, the policy gradient theorem, then
REINFORCE on a two-action toy problem small enough to check by hand, before CartPole.

### Exercise: REINFORCE for CartPole
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week3/lab_assignment.ipynb)

The hands-on part. Five tasks, each a few lines, assembling into a working
policy-gradient algorithm:

1. Build the policy network
2. Sample an action from the policy and record its log-probability
3. Compute discounted returns
4. Implement the REINFORCE loss
5. (Optional) Add a learned value baseline

### Solutions
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week3/lab_solutions.ipynb)

Complete solutions in the same order as the exercise, including the optional baseline.

## The CartPole Task

Balance a pole on a moving cart for as long as possible.

| | |
|---|---|
| **State** (4 continuous values) | cart position, cart velocity, pole angle, pole angular velocity |
| **Actions** (2 discrete) | `0` push left, `1` push right |
| **Reward** | `+1` for every timestep the pole stays up |
| **Episode ends** | pole falls past 12°, cart leaves the track, or 500 steps |
| **Solved** | average reward >= 475 over 100 episodes |

Note what is new here: the state is a vector of real numbers, not a cell in a table.
Two states that differ in the fourth decimal place are effectively the same state, and
a neural network treats them that way for free. A Q-table could not.

## Key Concepts

### Value-based vs policy-based

| | Value-based (Weeks 1-2) | Policy-based (this week) |
|---|---|---|
| What is learned | `Q(s,a)`, the value of each action | `pi(a|s)`, the probability of each action |
| How you act | pick `argmax Q` | sample from the distribution |
| Exploration | bolted on (epsilon-greedy) | built in - the policy *is* stochastic |
| Action spaces | discrete, ideally small | discrete or continuous |

### The REINFORCE update

For every action taken in an episode, scale its log-probability by the return that
followed it, and ascend:

```
loss = - sum_t  log pi(a_t | s_t) * G_t
```

Read it plainly: **an action followed by a good outcome gets made more likely, in
proportion to how good.** The minus sign is only because optimisers minimise.

### Why variance is the central problem

`G_t` is a single sampled return from a single episode. The same action can be
followed by a great outcome in one episode and a terrible one in the next, purely
because of what the *other* actions did. So the gradient is unbiased but noisy, and
learning is unstable.

The fix is a **baseline**: subtract an estimate of how good the state already was,
and use the difference. Now you reinforce an action for being *better than expected*,
not merely for being followed by a high number. Normalising the returns - which the
training loop does - is the crudest version of this; a learned value network (the
optional task) is the real thing, and it is the first half of actor-critic.

## Getting Started

Click any Colab badge above. PyTorch and Gymnasium are pre-installed in Colab;
nothing to set up.

Training takes a few minutes on a free Colab CPU. **A GPU will not help** - the
network is tiny and almost all the time goes on stepping the environment, which is
pure Python on the CPU. Do not bother requesting one.

## What You Should See

A correct implementation reaches the 500-step ceiling. From an actual run of the
solutions notebook (100-episode averages):

| Episode | 100 | 200 | 300 | 500 | 1000 |
|---|---|---|---|---|---|
| Avg reward | 79 | 341 | 469 | 467 | **500** |

- Early episodes last ~20-30 steps - the pole falls almost immediately
- Most of the climb happens between episodes 100 and 300
- The last 100 episodes should sit at or near 500, and greedy evaluation afterwards
  typically gives a clean 500.00 on every episode

**Expect dips on the way up.** A run can reach 500, drop back to 200 for a stretch,
and recover. That is the variance described above, not a bug in your code - the same
run that ended at 500.00 had a worst episode of 12. Runs also differ a lot from each
other; if one stalls completely, re-run the training cell before assuming your
implementation is wrong.

**The optional value baseline may come out either better or worse than plain
REINFORCE.** Two runs of the solutions notebook gave opposite orderings. The
solutions notebook explains why the effect is so weak here: normalizing the returns
is already a strong baseline, and CartPole states are all worth about the same, so
the state-dependence a value network buys you is nearly worthless. Sound idea, wrong
problem to show it off on.

Related, and worth knowing before your project: these notebooks set
`torch.manual_seed(42)` but never seed `env.reset()`, so **they do not actually
reproduce run to run.** Seeding the framework is not the same as seeding the
experiment.

## Running Locally (optional)

Everything runs in Colab, so this is only if you want it on your own machine.

```bash
pip install -r requirements.txt       # torch, gymnasium, numpy, matplotlib
python train_cartpole.py              # standalone script, no notebook
```

`train_cartpole.py` is the same algorithm as the exercise, as a plain script - handy
for experimenting with hyperparameters outside a notebook:

```bash
python train_cartpole.py --episodes 2000 --lr 0.005 --gamma 0.995
python train_cartpole.py --no-plot                      # skip the plot window
python train_cartpole.py --visualize --model trained_policy.pth   # watch it play
```

It saves the best model as it goes, so you can train once and re-watch it later. Pass
`--no-plot` if you are running it over SSH or piping the output - otherwise it opens
a plot window at the end and waits for you to close it.

## Looking Ahead

**Next week: Deep RL with Stable-Baselines3.** You will have written a policy-gradient
algorithm by hand; next week you meet the library version - a harder environment
(LunarLander) solved in three lines - and compare PPO, A2C and DQN. Doing it the hard
way first is the point: you will recognise what each of those algorithms is fixing.

**Next week you also choose your project.** Have a look at the
[project options](../projects/) before the session so you arrive with a preference.

## Additional Resources

- Sutton & Barto (2018), *Reinforcement Learning: An Introduction* - Chapter 13
- [Lilian Weng: Policy Gradient Algorithms](https://lilianweng.github.io/posts/2018-04-08-policy-gradient/)
- [OpenAI Spinning Up: Intro to Policy Optimization](https://spinningup.openai.com/en/latest/spinningup/rl_intro3.html)
