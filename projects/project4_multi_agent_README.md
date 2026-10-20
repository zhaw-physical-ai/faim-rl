# Project 4: Multi-Agent Reinforcement Learning

Train multiple agents to cooperate or compete using PettingZoo environments.

---

## 🤝 Overview

**Multi-agent RL** is fundamentally different from single-agent:
- Agents must model other agents' behavior
- Environment is non-stationary (other agents are learning too!)
- Coordination and communication challenges
- Emergent behaviors and strategies

**Why it's interesting:**
- Models real-world scenarios (team sports, traffic, economics)
- Leads to surprising emergent strategies
- More challenging than single-agent
- Active research area

**Goal:** Train agents to cooperate/compete effectively

---

## 🚀 Quick Start

The baseline uses **PettingZoo** with a cooperative game (Simple Spread).

### Requirements
```bash
pip install stable-baselines3 mpe2 supersuit
# MPE moved out of PettingZoo into its own package (mpe2)
```

### Run Baseline (5 minutes)
Open `project4_multi_agent.ipynb` and run all cells. This will:
1. Create Simple Spread environment (3 agents, 3 landmarks)
2. Train agents using independent PPO
3. Visualize cooperative behavior

**Expected result:** Agents learn to spread out to cover all landmarks

---

## 📊 Baseline Performance

| Environment | Agents | Task | Success Rate |
|-------------|--------|------|--------------|
| Simple Spread | 3 | Cover landmarks | 70-85% |
| Simple Tag | 3 + 1 | Catch adversary | 50-70% |
| Waterworld | 5 | Eat food, avoid poison | 60-80% |

---

## 💡 Ideas to Improve

### 1. **Communication**
Enable agents to share information:
```python
# Add communication channel
observation = {
    'own_obs': agent_state,
    'messages': [other_agent_messages],
    'teammates': [teammate_positions]
}

# Learn when/what to communicate
comm_action = comm_network(obs)
```

### 2. **Centralized Training, Decentralized Execution (CTDE)**
- Training: Use global information
- Execution: Each agent acts independently
- Better coordination without communication overhead

### 3. **Parameter Sharing**
All agents share same network:
- Faster learning (more data per update)
- Better generalization
- Works for homogeneous agents
```python
# Single shared policy for all agents
shared_policy = PPO('MlpPolicy', env, ...)
```

### 4. **Curriculum Learning**
Gradually increase difficulty:
- Stage 1: 2 agents, 2 landmarks
- Stage 2: 3 agents, 3 landmarks
- Stage 3: 4 agents, 4 landmarks
- Stage 4: Add obstacles

### 5. **Different Agent Types**
Heterogeneous teams:
- Specialist roles (scout, defender, attacker)
- Different observation/action spaces
- Division of labor

---

## 🔬 Advanced Extensions

### 1. **Self-Play**
Train agents against themselves:
- Leads to diverse strategies
- Arms race dynamics
- Used in AlphaGo, OpenAI Five

### 2. **Population-Based Training**
Maintain population of strategies:
- Different skill levels
- Counter-strategies emerge
- More robust final policy

### 3. **Graph Neural Networks**
Model agent interactions:
- Nodes = agents
- Edges = relationships
- Attention mechanism for important agents

### 4. **Mean Field Approximation**
Handle many agents efficiently:
- Model agent interactions statistically
- Scales to 100s or 1000s of agents
- Good for crowd scenarios

### 5. **Opponent Modeling**
Explicitly model other agents:
- Predict opponent actions
- Theory of mind
- Best response strategies

### 6. **Mixed Cooperative-Competitive**
Complex scenarios:
- Team A vs Team B (cooperation + competition)
- Alliances and betrayals
- Emergence of social norms

---

## 📈 Evaluation Metrics

Track these for multi-agent systems:
- **Task success rate** (overall goal achievement)
- **Individual rewards** (fairness)
- **Coordination level** (how well agents work together)
- **Diversity of strategies** (avoid mode collapse)
- **Robustness to missing agents**
- **Transfer to different team sizes**

---

## 🎮 Available Environments

**PettingZoo MPE (Multi-Particle Environments):**
- **simple_spread**: Cooperative coverage
- **simple_tag**: Predator-prey
- **simple_adversary**: Mixed incentives
- **simple_push**: Physical coordination
- **simple_world_comm**: With communication

**Competitive:**
- **multiwalker**: Keep beam balanced
- **waterworld**: Eat food, avoid poison
- **knights_archers_zombies**: Combat game

**Classic:**
- **chess**: Two-player board game
- **go**: Ancient board game
- **hanabi**: Cooperative card game

---

## 🎯 Project Deliverables

**Minimum** (complete baseline):
- Run baseline on Simple Spread
- Document success rate
- Visualize agent behaviors

**Standard** (improvements):
- Try 2 different environments
- Implement parameter sharing
- Compare independent vs shared policies

**Advanced** (extensions):
- Add communication channel
- Implement self-play
- Test on competitive environment (Simple Tag)
- Analyze emergent strategies

---

## 📚 Resources

**Libraries:**
- [PettingZoo Docs](https://pettingzoo.farama.org/)
- [SuperSuit](https://github.com/Farama-Foundation/SuperSuit) (wrappers)

**Papers:**
- [Multi-Agent DDPG](https://arxiv.org/abs/1706.02275)
- [QMIX](https://arxiv.org/abs/1803.11485)
- [CommNet](https://arxiv.org/abs/1605.07736)

**Tutorials:**
- [PettingZoo SB3 Tutorial](https://pettingzoo.farama.org/tutorials/sb3/)

---

## 🐛 Troubleshooting

**Agents don't cooperate:**
- Check reward structure (aligned incentives?)
- Try parameter sharing
- Increase training time
- Use centralized critic

**Training is unstable:**
- Reduce learning rate
- Use smaller batch sizes
- Independent learning first, then joint

**Agents ignore each other:**
- Ensure observations include other agents
- Add relative positions to observations
- Use communication channels

**Mode collapse (all agents do same thing):**
- Add diversity bonus to reward
- Use population-based training
- Different initializations per agent

---

## 💡 Tips

- **Start with cooperation:** Easier than competition
- **Visualize:** Always watch agents interact
- **Curriculum:** Start with 2 agents before scaling up
- **Patience:** Multi-agent learning is slower
- **Randomize:** Use different random seeds
- **Monitor all agents:** Not just average reward

---

## 🧠 Key Challenges

Multi-agent RL is harder because:
1. **Non-stationarity**: Other agents are moving targets
2. **Credit assignment**: Who caused the success/failure?
3. **Scalability**: N agents = exponential complexity
4. **Coordination**: Without communication, hard to cooperate
5. **Equilibrium selection**: Multiple valid strategies

But it's also more realistic and interesting! 🚀

---

Good luck! Multi-agent systems lead to fascinating emergent behaviors.
