# Project 2: Autonomous Driving with Highway-Env

Train an agent to drive safely on a highway with dynamic traffic.

---

## 🚗 Overview

**Highway-Env** is a simulation environment for autonomous driving research. It provides realistic scenarios like:
- Highway driving with multiple lanes
- Merging and lane changing
- Collision avoidance
- Speed control

**Why it's good for learning:**
- Visual and interpretable (bird's eye view)
- Multiple difficulty levels
- Fast simulation (no physics engine overhead)
- Configurable scenarios (traffic density, speed limits, etc.)

**Goal:** Drive safely while maintaining speed and avoiding collisions

---

## 🚀 Quick Start

The baseline trains a DQN agent on the highway-fast-v0 environment.

### Requirements
```bash
pip install stable-baselines3 highway-env
```

### Run Baseline (3 minutes)
Open `project2_highway_driving.ipynb` and run all cells. This will:
1. Create highway environment with 5 lanes
2. Train a DQN agent for 50k steps (~3 min)
3. Visualize the driving behavior

**Expected result:** Agent drives safely with 70-80% success rate.

---

## 📊 Baseline Performance

| Timesteps | Success Rate | Avg Speed | Collision Rate |
|-----------|--------------|-----------|----------------|
| 25k | 40-50% | ~25 km/h | 50-60% |
| 50k | 70-80% | ~28 km/h | 20-30% |
| 150k | 85-95% | ~30 km/h | 5-15% |

**Success** = reach goal without collision

---

## 💡 Ideas to Improve

### 1. **Reward Shaping**
Current reward: +1 for high speed, -1 for collision

Try different reward structures:
```python
# Speed + lane keeping
reward = speed_reward + lane_center_bonus - collision_penalty

# Safety-first
reward = -10 * collision + 0.5 * speed + 2 * reached_goal

# Efficiency
reward = speed / fuel_used - 5 * collision
```

### 2. **Observation Configuration**
```python
# Current: 5x5 grid of nearby vehicles
config = {
    "observation": {
        "type": "Kinematics",
        "vehicles_count": 15,  # See more vehicles
        "features": ["presence", "x", "y", "vx", "vy"],
        "absolute": False
    }
}

# Try: Image observations
config["observation"]["type"] = "GrayscaleObservation"
# Requires CNN policy!
```

### 3. **Traffic Scenarios**
```python
# Dense traffic
config["vehicles_count"] = 50

# Aggressive drivers
config["other_vehicles_type"] = "highway_env.vehicle.behavior.AggressiveVehicle"

# Variable speeds
config["initial_lane_id"] = None  # Random starting lane
```

### 4. **Multi-Objective Optimization**
Balance multiple goals:
- Safety (minimize collisions)
- Efficiency (maximize speed)
- Comfort (minimize acceleration changes)
- Legality (follow lane rules)

### 5. **Algorithm Comparison**
- **DQN**: Good baseline, discrete actions
- **PPO**: Handles continuous actions (steering angle)
- **SAC**: State-of-art for continuous control

---

## 🔬 Advanced Extensions

### 1. **Continuous Action Space**
Switch from discrete (5 actions) to continuous (steering + throttle):
```python
env.config["action"]["type"] = "ContinuousAction"
# Use PPO or SAC instead of DQN
```

### 2. **Attention Mechanisms**
Learn to focus on relevant vehicles:
```python
# Use attention layers in network
policy_kwargs = dict(
    net_arch=[dict(vf=[256, 256], pi=[256, 256])],
    features_extractor_class=AttentionExtractor  # Custom
)
```

### 3. **Curriculum Learning**
Start easy, gradually increase difficulty:
- Stage 1: Empty highway (no traffic)
- Stage 2: Light traffic (10 vehicles)
- Stage 3: Dense traffic (50 vehicles)
- Stage 4: Aggressive drivers

### 4. **Multi-Agent Scenarios**
Train multiple agents to cooperate:
- Platoon formation
- Zipper merging
- Roundabouts with multiple agents

### 5. **Sim-to-Real Transfer**
Techniques to bridge simulation and reality:
- Domain randomization (vary dynamics)
- System identification
- Robust training with perturbations

### 6. **Explainable Driving**
Understand agent decisions:
- Visualize attention weights
- Log decision rationale
- Safety verification (formal methods)

---

## 📈 Evaluation Metrics

Track these for comprehensive analysis:
- **Success rate** (reached goal without collision)
- **Average speed** (efficiency)
- **Collision rate** (safety)
- **Lane changes per episode** (smoothness)
- **Time to goal** (performance)
- **Comfort** (acceleration variance)

---

## 🎯 Project Deliverables

**Minimum** (complete baseline):
- Run baseline notebook
- Document success rate and speed
- Analyze common failure cases

**Standard** (improvements):
- Try 2-3 reward configurations
- Test on different traffic densities
- Compare discrete vs continuous actions

**Advanced** (extensions):
- Implement curriculum learning
- Add attention mechanism
- Multi-objective optimization

---

## 📚 Resources

**Documentation:**
- [Highway-Env Docs](https://highway-env.farama.org/)
- [Configuration Options](https://highway-env.farama.org/configuration/)

**Papers:**
- [Learning to Drive in a Day](https://arxiv.org/abs/1807.00412)
- [End-to-End Driving](https://arxiv.org/abs/1604.07316)

**Related Environments:**
- `highway-v0`: Standard highway
- `merge-v0`: Merging onto highway
- `roundabout-v0`: Navigate roundabout
- `parking-v0`: Parallel parking

---

## 🐛 Troubleshooting

**Agent crashes constantly:**
- Increase collision penalty in reward
- Train longer (150k steps)
- Use safety constraints or shields

**Agent drives too slowly:**
- Increase speed reward weight
- Lower speed threshold for reward
- Add time penalty for slow driving

**Environment won't render:**
- Install pygame: `pip install pygame`
- For Colab, use: `env.render(mode='rgb_array')`

**Training is unstable:**
- Lower learning rate (1e-4)
- Increase replay buffer size
- Use prioritized experience replay

---

## 💡 Tips

- **Visualization is key:** Always watch agent drive to debug
- **Start simple:** Empty highway first, then add traffic
- **Balance rewards:** Weight safety higher than speed initially
- **Use checkpoints:** Save models every 25k steps
- **Test robustly:** Evaluate on unseen traffic patterns

---

Good luck! 🚗 Highway-Env is perfect for learning control and safety trade-offs.
