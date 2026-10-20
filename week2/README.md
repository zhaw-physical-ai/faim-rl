# Week 2: Q-Learning


## Learning Objectives

By the end of this session, you will be able to:
- Understand the difference between value-based and policy-based learning
- Explain the Q-learning update rule and why it works
- Implement Q-learning for turn-based games
- Train agents and play against them interactively
- Analyze how state space complexity affects learning

## Materials

### Worked Example: Q-Learning with Tic-Tac-Toe
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week2/example_tictactoe.ipynb)

Worked example you can run and modify, building up Q-learning step by step:
- From bandits to MDPs
- Value functions and the Bellman equation
- Building a Tic-Tac-Toe agent from scratch
- Watching the agent improve over time
- Visualizing learned Q-values

### Exercise: Q-Learning for Nim
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week2/lab_assignment.ipynb)

The hands-on part of this week. You will:
- Implement the Q-learning update rule
- Train agents on progressively harder Nim configurations
- Play interactively against your trained agents
- **Challenge:** Can you beat the agent at 4 piles?

### Solutions
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/week2/lab_solutions.ipynb)

Complete solutions with detailed explanations.

## The Game of Nim

Nim is a mathematical strategy game:
- One or more **piles** of objects
- Two players take turns
- On your turn: remove **any number** (≥1) from **one pile**
- **The player who takes the last object LOSES**

### Why Nim?
- Perfect for tabular Q-learning
- Trains in seconds
- Natural difficulty progression (1 pile → 4 piles)
- Mathematical optimal strategy exists (but it's not obvious!)

## Getting Started

### Using Google Colab (Recommended)
1. Click the "Open in Colab" button above
2. All libraries are pre-installed
3. Run cells with Shift+Enter

### Running Locally
```bash
pip install numpy matplotlib jupyter
jupyter notebook lab_assignment.ipynb
```

## Exercise Structure

### Part 1-2: Understand the Environment
Run cells to see how Nim works. Simple variable changes only.

### Part 3: Implement the Q-Learning Update ⭐
**The key task.** Build the target, then nudge `Q(s,a)` toward it:
```python
if next_state is None:
    target = final_reward                 # terminal move: the verdict is the target
else:
    next_q_values = [self.Q[(next_state, a)] for a in valid_actions_for(next_state)]
    target = self.gamma * max(next_q_values)   # reward is 0 here, so only the future counts

self.Q[(state, action)] = current_q + self.alpha * (target - current_q)
```

### Part 4-6: Train and Play
- Train on 1, 2, 3, and 4 piles
- Play interactively against your agents
- See if you can beat them!

### Part 7: Evaluation
Compare agent performance across configurations.

### Part 8: Analysis Questions
Reflect on what you observed.

### Bonus Challenges (Optional)
- Self-play training
- Learning curve visualization
- Hyperparameter exploration

## Key Concepts

### From Bandits to MDPs
| Multi-Armed Bandits | Markov Decision Processes |
|---------------------|---------------------------|
| Single state | Multiple states |
| Immediate rewards | Delayed rewards |
| Find best action | Find best action **per state** |

### The Q-Learning Update
```
Q(s,a) ← Q(s,a) + α[r + γ·max Q(s',a') - Q(s,a)]
```
- **α (alpha):** Learning rate - how fast we update
- **γ (gamma):** Discount factor - importance of future rewards
- **r:** Immediate reward
- **max Q(s',a'):** Best expected future value

### Value vs Policy Learning
| Value-Based (Q-Learning) | Policy-Based |
|--------------------------|--------------|
| Learn Q(s,a) values | Learn π(a\|s) directly |
| Policy: pick argmax Q | Explicit policy function |
| Good for discrete actions | Good for continuous actions |

## Tips for Success

### The Update You Need to Implement
Week 1's bandit update aimed straight at the reward:
```python
Q[a] = Q[a] + alpha * (reward - Q[a])
```
Nim only pays out on the final move, so that target would be 0 everywhere else and
the opening would never be learned. Q-learning aims at the reward **plus the value of
where you land**:
```
Q(s,a) <- Q(s,a) + alpha * ( r + gamma * max_a' Q(s',a') - Q(s,a) )
```
In the `learn()` method you walk backwards through the game, so `s'` is simply the
position you faced on your *next* turn, and `r` is 0 for every move but the last:
```python
# In the learn() method:
if next_state is None:
    target = final_reward
else:
    next_q_values = [self.Q[(next_state, a)] for a in valid_actions_for(next_state)]
    target = self.gamma * max(next_q_values)

self.Q[(state, action)] = current_q + self.alpha * (target - current_q)
```
The *shape* is identical to the bandit update - nudge the old estimate a fraction
`alpha` toward a target. Only the target changed. That `max_a' Q(s',a')` term is what
carries the win or loss back from the last move to the first.

### Playing Against the Agent
1. Uncomment the `play_against_agent(...)` line
2. Run the cell
3. Enter moves as: `pile, amount` (e.g., `0, 2` removes 2 from pile 0)

### Winning at Nim
- **1 pile:** Leave opponent with exactly 1 object
- **2 piles:** Make the piles equal, then mirror opponent's moves
- **3+ piles:** The XOR strategy (ask about this in class!)

### Common Issues
- If Q-values all stay 0: check your update formula - the `self.Q[(state, action)] = ...`
  line is still the untouched `current_q` placeholder
- If only the final moves ever get non-zero values: your target is missing the
  `gamma * max_a' Q(s',a')` term, so nothing propagates backwards
- If agent plays randomly: Make sure epsilon=0 during evaluation
- If training is slow: You may need more games for complex configurations

## Challenge Progression

| Config | Difficulty | Can You Win? |
|--------|------------|--------------|
| [5] | Easy | Yes, with the right first move |
| [3, 4] | Medium | Yes, make piles equal |
| [2, 3, 4] | Hard | Requires deeper strategy |
| [1, 3, 5, 7] | Expert | Only if you go second! |

## Connection to Next Week

This week: **Tabular Q-Learning**
- Works great for small state spaces
- Q-table stores one value per (state, action) pair
- Limited by memory and exploration

Next week: **Deep Q-Networks (DQN)**
- Neural network replaces Q-table
- Generalizes to unseen states
- Scales to Atari games and beyond!

## Additional Resources

**Theory:**
- "RL: An Introduction" - Chapter 6 (TD Learning)
- [Q-Learning Explained](https://www.freecodecamp.org/news/an-introduction-to-q-learning-reinforcement-learning-14ac0b4493cc/)

**Nim Strategy:**
- [Nim Game Theory](https://en.wikipedia.org/wiki/Nim#Mathematical_theory)
- The XOR trick: Position is losing if pile1 ⊕ pile2 ⊕ ... = 0

**Fun Extensions:**
- [OpenAI Gym Environments](https://gymnasium.farama.org/)
- [AlphaGo Documentary](https://www.youtube.com/watch?v=WXuK6gekU1Y) - Self-play at scale!

