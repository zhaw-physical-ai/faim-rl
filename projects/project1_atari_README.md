# Project 1: Atari Pong with Deep RL

Train an agent to play Atari Pong using deep reinforcement learning.

---

## 🎮 Overview

**Pong** is a classic Atari game where you control a paddle to bounce a ball past your opponent. It's a great testbed for visual RL because:
- State is raw pixels (84x84 grayscale images)
- Actions are simple (6 in the ALE action set, effectively up, down and stay)
- It is one of the easier Atari games - but "easy" for Atari still means a long
  training run (see below)
- Clear progress visualization

**Goal:** Beat the built-in AI opponent consistently

---

## 🚀 Quick Start

The baseline uses **stable-baselines3** with a CNN policy trained on preprocessed frames.

### Requirements
```bash
pip install "stable-baselines3[extra]" "gymnasium[atari]"
# ale-py ships the ROMs now; the old accept-rom-license extra no longer exists
```

### Run Baseline (about 10 minutes)
Open `project1_atari_pong.ipynb` and run all cells. This will:
1. Load Pong environment with frame preprocessing
2. Train a PPO agent for 100k steps
3. Evaluate and visualize the trained agent

**Expected result:** after 100k steps the agent still loses almost every point (a
score close to -21). That is normal: the baseline is a **pipeline check** - it shows
that preprocessing, training and evaluation all work - not a trained player.

---

## 📊 What Training Pong Takes

Pong typically needs somewhere between several hundred thousand and a few million
steps before the agent wins points regularly, and longer still to beat the built-in
opponent consistently. On a CPU that is hours, so:

- **Switch Colab to a GPU runtime** (*Runtime -> Change runtime type*). The CNN is
  the bottleneck here, unlike in the earlier labs.
- **Save checkpoints** (`model.save(...)`) and continue with `model.learn(...)`
  across sessions - Colab runtimes do disconnect.
- **Track progress by the score, not wins:** going from -21 to -15 is real progress
  long before the agent wins a game.

Part of your project is finding out how far you can get within your compute budget.

---

## 💡 Ideas to Improve

### 1. **Train Longer**
- Baseline: 100k steps
- Try: 1M steps or more (GPU runtime)
- Impact: the single biggest lever for this project

### 2. **Hyperparameter Tuning**
```python
# Try different learning rates
learning_rate = [1e-4, 3e-4, 1e-3]

# Adjust frame stack
n_stack = [2, 4, 8]  # How many frames to stack

# Modify network architecture
policy_kwargs = dict(
    net_arch=dict(pi=[128, 128], vf=[128, 128])  # Bigger network
)
```

### 3. **Algorithm Comparison**
- Try DQN (value-based) vs PPO (policy-based)
- Compare A2C (simple) vs PPO (robust)
- Use different CNN architectures

### 4. **Frame Preprocessing**
- Current: 84x84 grayscale, 4-frame stack
- Try: Different resolutions (64x64, 96x96)
- Experiment: Skip more frames for faster training

### 5. **Reward Shaping**
- Default: +1 for scoring, -1 for opponent scoring
- Try: Add intermediate rewards (e.g., hitting ball)
- Caution: Can help but might create shortcuts

---

## 🔬 Advanced Extensions

### 1. **Multi-Game Transfer**
Train on multiple Atari games and see if skills transfer:
- Pong → Breakout (both paddle games)
- Compare single-task vs multi-task

### 2. **Curiosity-Driven Exploration**
Add intrinsic motivation to explore novel states:
```python
# Use RND (Random Network Distillation) or ICM
from stable_baselines3.common.torch_layers import BaseFeaturesExtractor
# Implement curiosity bonus
```

### 3. **Self-Play**
Instead of fixed opponent, train two agents against each other:
- Leads to more diverse strategies
- Can discover novel tactics
- Requires custom environment wrapper

### 4. **Visual Attention Analysis**
Understand what the agent is "looking at":
- Use Grad-CAM or saliency maps
- Visualize which pixels are important
- Debug why agent fails in certain situations

### 5. **Distributional RL**
Use distributional value functions (C51, QR-DQN):
- Better uncertainty estimation
- Can handle stochastic rewards
- More sample efficient

### 6. **Human-Level Performance Analysis**
- Measure: frames to reach 90% win rate
- Compare: different algorithms on sample efficiency
- Benchmark: your agent vs human players

---

## 📈 Evaluation Metrics

Track these to measure progress:
- **Win rate** vs built-in AI
- **Average episode reward** (-21 to +21)
- **Training time** to reach 90% wins
- **Sample efficiency** (wins per 100k steps)
- **Robustness** (performance across 5 random seeds)

---

## 🎯 Project Deliverables

Choose what to focus on (6 hours total):

**Minimum** (complete baseline):
- Run baseline notebook
- Document results (plots, win rate)
- Brief analysis (what worked, what didn't)

**Standard** (improvements):
- Try 2-3 hyperparameter configurations
- Compare 2 algorithms (e.g., PPO vs DQN)
- Analyze learning curves and failure cases

**Advanced** (extensions):
- Implement 1-2 advanced ideas
- Detailed comparison and analysis
- Novel insight or technique

---

## 📚 Resources

**Papers:**
- [Playing Atari with Deep RL](https://arxiv.org/abs/1312.5602) (DQN)
- [PPO Paper](https://arxiv.org/abs/1707.06347)

**Tutorials:**
- [Stable-Baselines3 Atari Docs](https://stable-baselines3.readthedocs.io/en/master/guide/examples.html#atari-games)
- [OpenAI Gym Atari](https://gymnasium.farama.org/environments/atari/)

**Tips:**
- Use a GPU if available (much faster for the CNN)
- Save checkpoints every 100k steps
- Record videos to debug failures
- Try multiple random seeds for robustness

---

## 🐛 Troubleshooting

**Agent doesn't improve:**
- Train longer - at 100k steps a score near -21 is expected
- Look at the score trend (e.g. -21 -> -18), not just wins
- Check frame preprocessing is working
- Verify rewards are being received

**Training is too slow:**
- Use GPU if available
- Reduce evaluation frequency
- Decrease frame size (84x84 → 64x64)

**Unstable training:**
- Lower learning rate (1e-4)
- Increase batch size
- Use PPO instead of DQN

---

Good luck! 🚀 Pong is a great starting point for visual RL.
