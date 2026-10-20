# RL Project Ideas

Choose one of these projects and build on it for the rest of the module. Each one
comes with a **working baseline that runs in a few minutes**, plus guided ideas for
improvement and open-ended extensions.

**The projects are introduced at the end of Week 3** so you have time to think before
committing. Pick one by Week 4.

---

## Overview

Each project provides:
- ✅ **Working baseline** - runs in 2-5 minutes
- ✅ **Colab-ready** - no setup required
- ✅ **Improvement ideas** - guided suggestions
- ✅ **Advanced extensions** - open-ended challenges
- ✅ **Can run locally** - with more compute power

**Time commitment:** roughly 5 hours, spread over two and a half sessions

| When | What |
|---|---|
| End of Week 3 | Projects introduced. Browse the options, come back with a preference. |
| Week 4 (second half) | Pick one, get the baseline running, understand the environment. |
| Week 5 | Implement your improvements. |
| Week 6 | Finish, analyse results, present. |

There is no Christmas break in the middle this year, so the project runs straight
through. Get your baseline running in Week 4 - do not leave it until Week 5.

---

## 🎮 Project 1: Atari Pong

Train an agent to play Pong using deep RL with visual inputs.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/projects/project1_atari_pong.ipynb)

**Domain:** Games, Computer Vision  
**Difficulty:** ⭐⭐ Easy-Medium  
**Baseline time:** ~5 minutes (100k steps)

### What You'll Learn
- CNN policies for image inputs
- Frame stacking and preprocessing
- Algorithm comparison (PPO vs DQN)
- Training on visual observations

### Quick Stats
- **State:** 84x84 grayscale images (4 frames stacked)
- **Actions:** 2 discrete (up, down)
- **Success:** 80%+ win rate vs built-in AI

[**Full README →**](project1_atari_README.md)

---

## 🚗 Project 2: Highway Driving

Train an agent for autonomous highway driving with traffic.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/projects/project2_highway_driving.ipynb)

**Domain:** Robotics, Autonomous Vehicles  
**Difficulty:** ⭐⭐⭐ Medium  
**Baseline time:** ~3 minutes (50k steps)

### What You'll Learn
- Safety-critical RL
- Reward shaping for safety/efficiency trade-offs
- Multi-objective optimization
- Continuous vs discrete actions

### Quick Stats
- **State:** Kinematic features (position, velocity of nearby cars)
- **Actions:** 5 discrete (idle, left, right, faster, slower)
- **Success:** 70%+ episodes without collision

[**Full README →**](project2_highway_README.md)

---

## 💰 Project 3: Stock Trading

Apply RL to algorithmic trading using real historical data.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/projects/project3_stock_trading.ipynb)

**Domain:** Finance, Time Series  
**Difficulty:** ⭐⭐⭐ Medium  
**Baseline time:** ~2 minutes (50k steps)

### What You'll Learn
- RL in a different domain (not games/robotics)
- Feature engineering (technical indicators)
- Risk management
- Backtesting and evaluation

### Quick Stats
- **State:** OHLC prices + position
- **Actions:** Buy, Sell, Hold
- **Success:** Outperform buy-and-hold strategy

⚠️ **Disclaimer:** Educational only, not financial advice!

[**Full README →**](project3_trading_README.md)

---

## 🤝 Project 4: Multi-Agent RL

Train multiple cooperative or competitive agents.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/zhaw-physical-ai/faim-rl/blob/main/projects/project4_multi_agent.ipynb)

**Domain:** Multi-Agent Systems, Coordination  
**Difficulty:** ⭐⭐⭐⭐ Hard  
**Baseline time:** ~5 minutes (100k steps)

### What You'll Learn
- Coordination and communication
- Emergent behaviors
- Parameter sharing
- Cooperative vs competitive scenarios

### Quick Stats
- **Environment:** Simple Spread (3 agents, 3 landmarks)
- **Goal:** Each agent covers different landmark
- **Success:** Coordinated coverage

[**Full README →**](project4_multi_agent_README.md)

---

## 📊 Project Comparison

| Project | Domain | Difficulty | Special Features | Training Time |
|---------|--------|------------|------------------|---------------|
| 1. Pong | Games/Vision | ⭐⭐ | Visual input, classic benchmark | ~5 min |
| 2. Highway | Robotics | ⭐⭐⭐ | Safety-critical, real-world | ~3 min |
| 3. Trading | Finance | ⭐⭐⭐ | Different domain, real data | ~2 min |
| 4. Multi-Agent | Coordination | ⭐⭐⭐⭐ | Multiple agents, emergence | ~5 min |

---

## 🎯 Choosing Your Project

**Choose based on:**
- **Interest:** Pick the domain that excites you!
- **Skills:** What do you want to learn?
- **Difficulty:** How challenging do you want it?
- **Extensions:** Which has the most interesting advanced ideas?

**Can't decide?**
- **Project 2 (Highway)** is the best all-rounder: quick to train, easy to see what
  the agent is doing, and reward shaping gives you plenty to experiment with
- **Project 1 (Pong)** if you want to work with visual input and CNNs
- **Project 3 (Trading)** if you want a non-game domain

**Want more challenge?**
- **Project 4 (Multi-Agent)** - cutting-edge research area
- Or define your own project (discuss with instructor first)

---

## 💻 Setup

### Google Colab (Recommended)
Click any "Open in Colab" badge above. Everything runs in the browser, no setup needed!

### Local Setup
Each project has its own requirements - see its README. Generally:
```bash
pip install stable-baselines3 gymnasium
# plus project-specific packages
```

### A note on the baselines

All four baselines were checked against current library versions (Gymnasium 1.4,
stable-baselines3 2.9) and train successfully. Several needed fixes to get there,
because the RL ecosystem moves fast and breaks things:

| Project | What had to change |
|---|---|
| 1. Pong | `PongNoFrameskip-v4` -> `ALE/Pong-v5`; ROMs now ship with `ale-py`, so the old `accept-rom-license` extra is gone; `gym.register_envs(ale_py)` is now required |
| 3. Trading | `yfinance` now returns MultiIndex columns, which breaks the env; the backtest also read attributes that do not exist |
| 4. Multi-agent | MPE moved out of PettingZoo into its own `mpe2` package |

**If something still breaks, that is normal and it is not your fault.** Read the error,
check the library's current docs, and ask. Debugging a dependency change is a real part
of applied RL - it is not a detour from the project.

---

## 📝 Deliverables

For your 15-minute presentation, prepare:

1. **What you did:**
   - Which project and why
   - Baseline results
   - Improvements attempted

2. **Results:**
   - Performance metrics
   - Learning curves
   - Visualizations

3. **Analysis:**
   - What worked and why
   - What didn't work
   - Interesting findings

4. **Next steps:**
   - What would you try with more time?

**No formal report required** - focus on understanding and results!

---

## 🆘 Getting Help

- **Baseline doesn't run?** Check project README troubleshooting section
- **Can't decide which project?** Ask during lab session
- **Want to try something different?** Propose your own project idea!

---

## 📚 Additional Resources

- [Stable-Baselines3 Zoo](https://github.com/DLR-RM/rl-baselines3-zoo) - Pre-trained models
- [Gymnasium Environments](https://gymnasium.farama.org/) - More environments
- [Papers with Code RL](https://paperswithcode.com/area/playing-games) - Latest research

