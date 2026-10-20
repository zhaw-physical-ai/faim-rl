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

More weeks will be added as the module progresses.

## Getting Started

Each week has its own README with "Open in Colab" buttons, so nothing needs to be installed. To work locally instead you need Python 3 with `numpy`, `matplotlib` and `jupyter`:

```bash
pip install numpy matplotlib jupyter
```
