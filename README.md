# CAS Foundations of AI and Machine Learning: Reinforcement Learning

Course materials for the Reinforcement Learning module of the [CAS Foundations of AI and Machine Learning](https://www.zhaw.ch/en/engineering/continuing-education/detail/course/cas-foundations-of-ai-and-machine-learning) at ZHAW School of Engineering (Centre for Artificial Intelligence).

## About the Module

The module shows how reinforcement learning (RL) can be used to optimise control and regulation processes and to make decisions autonomously. It covers:
- Introduction to RL, including deep RL
- Sequential decision-making: value functions and the exploration-exploitation tradeoff
- Sampling-based methods: temporal difference learning and Q-learning
- Policy gradient methods

Each session combines theory with hands-on practice, so the folders below contain the lecture examples and lab exercises as Jupyter notebooks.

## Course Content

### [Week 1: Introduction to Reinforcement Learning & Multi-Armed Bandits](week1/)
- Understand the RL paradigm: agent, environment, actions, and rewards
- Explore the exploration vs exploitation tradeoff
- Implement epsilon-greedy and UCB algorithms from scratch

### [Week 2: Q-Learning](week2/)
- Move from bandits to Markov decision processes: states, transitions and delayed rewards
- Understand the Q-learning update rule and why it works
- Implement tabular Q-learning for turn-based games (Tic-Tac-Toe, Nim) and play against your agent

### [Week 3: Policy Gradient Methods](week3/)
- Move from learning values to learning the policy directly
- Implement REINFORCE from scratch and solve CartPole
- Understand why policy gradients are high-variance, and what a baseline does
- *Projects are introduced at the end of this session*

### [Week 4: Deep RL with Stable-Baselines3](week4/)
- Your 200 lines of REINFORCE vs three lines of PPO
- Compare PPO, A2C and DQN on LunarLander
- Hyperparameters, training length, and reading results honestly
- *Project work starts in the second half of this session*

### [Weeks 5-6: Project Work](projects/)
- Four projects with working baselines: Pong, highway driving, stock trading, multi-agent
- Build on a baseline, improve it, present what you found

## Module Schedule

| Week | Topic | Built by hand? |
|---|---|---|
| 1 | Bandits and the exploration/exploitation tradeoff | yes - from scratch |
| 2 | Q-learning on turn-based games | yes - from scratch |
| 3 | Policy gradients (REINFORCE) | yes - from scratch |
| 4 | Deep RL with stable-baselines3 | no - library |
| 5-6 | Project work | library, your own extensions |

The order is deliberate. Weeks 1-3 build each algorithm by hand so you know what is
inside the library calls you make from Week 4 onwards. By the time you write
`PPO('MlpPolicy', env)`, you will have written every part of it.

## Getting Started

Each week has its own README with "Open in Colab" buttons, so nothing needs to be installed. To work locally instead you need Python 3 with `numpy`, `matplotlib` and `jupyter`:

```bash
pip install numpy matplotlib jupyter
```
