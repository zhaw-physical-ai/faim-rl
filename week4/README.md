# Week 4: Deep RL with Stable-Baselines3

Weeks 1-3 built everything by hand, which is the only way to understand what the
algorithms do. This week you use the tools people actually use in practice, on a
harder problem than anything so far - and you start your project.

## Learning Objectives

By the end of this session, you will be able to:
- Train an RL agent with `stable-baselines3` in a few lines of code
- Explain what PPO, A2C and DQN each change relative to the algorithms you wrote
- Compare algorithms fairly on the same environment and read the results honestly
- Judge the effect of learning rate and training length
- Decide when a default configuration is good enough, and when it is not

## Materials

### Exercise: Deep RL with LunarLander
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week4/lab_assignment.ipynb)

- **Task 0:** your 200 lines of REINFORCE vs three lines of PPO, on the same CartPole
- **Task 1:** explore LunarLander, and watch a random agent crash
- **Task 2:** train PPO, evaluate it, and watch it land
- **Task 3:** compare PPO, A2C and DQN
- **Task 4:** vary the learning rate
- **Task 5 (optional):** train for 300k steps with an evaluation callback

Unlike weeks 1-3 this notebook has no gaps to fill - it runs as written. The work is
in reading the results, changing things, and explaining what you see.

### Solutions
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week4/lab_solutions.ipynb)

Expected results and analysis for each task.

## The LunarLander Task

Land a spacecraft on the pad between the flags.

| | |
|---|---|
| **State** (8 values) | x, y position; x, y velocity; angle, angular velocity; left and right leg contact |
| **Actions** (4 discrete) | `0` do nothing, `1` fire left engine, `2` fire main engine, `3` fire right engine |
| **Reward** | +100 to +140 for landing, -100 for crashing, small fuel penalty per engine firing, bonus for leg contact |
| **Solved** | average reward >= 200 over 100 episodes |

This is a real step up from CartPole. The reward is **shaped but conflicting**: fuel
costs push toward doing nothing, and doing nothing means crashing. The agent has to
find a landing that is both safe *and* economical, and the two pull against each
other. A random agent scores around -200.

## The Three Algorithms

| | Family | What it changes | Character |
|---|---|---|---|
| **PPO** | policy gradient | clips how far the policy may move in one update | reliable, strong default |
| **A2C** | policy gradient | updates every few steps using a learned value baseline | fast per step, noisier |
| **DQN** | value-based | Q-learning with a neural network, a replay buffer and a target network | sample-efficient, fussier to tune |

Worth noticing where these sit relative to what you have already written. **DQN is
week 2's Q-learning** with the table swapped for a network - exactly the fix that the
exploding Nim Q-table pointed at. **PPO and A2C are week 3's REINFORCE** with the
variance problem taken seriously.

## Getting Started

Click a Colab badge. The first cell installs `swig` and `stable-baselines3` -
LunarLander needs Box2D, which needs swig, so **run that cell first and let it
finish** before anything else.

> **If `gym.make('LunarLander-v3')` fails with a Box2D error**, the install cell did
> not complete. Re-run it, then use *Runtime -> Restart session* and run from the top.

A GPU will not help much here either; these networks are small and the bottleneck is
the physics simulation on the CPU.

## Expected Results

After 100k timesteps:

| Algorithm | Mean reward | Verdict |
|---|---|---|
| Random | ~ -200 | crashes |
| PPO | 180-240 | usually solves it |
| A2C | 120-200 | more variable |
| DQN | 150-220 | needs tuning |

**Read these as a starting point for discussion, not a ranking.** Each algorithm was
given one configuration and one seed, and 100k steps is short. A fair comparison would
tune each one and average over several seeds - which is exactly the kind of claim to
be sceptical about when you read it in a paper, or write it in your project report.

## Your Project Starts This Week

The second half of the session is project time. If you have not already picked one,
see the [project options](../projects/) - four baselines, all built on the
stable-baselines3 you are learning here.

You have **this session plus the two after it**. Have a project chosen and its
baseline running by the end of today.

## Additional Resources

- [Stable-Baselines3 documentation](https://stable-baselines3.readthedocs.io/)
- [SB3 RL Zoo](https://github.com/DLR-RM/rl-baselines3-zoo) - tuned hyperparameters and trained agents
- [LunarLander environment](https://gymnasium.farama.org/environments/box2d/lunar_lander/)
- [PPO paper](https://arxiv.org/abs/1707.06347) (Schulman et al., 2017)
