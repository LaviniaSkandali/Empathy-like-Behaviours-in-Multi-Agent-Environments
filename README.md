# Empathy-like-Behaviours-in-Multi-Agent-Environments
Can reward sharing induce pro-social emergence?
# Empathy-Like Behaviors in Multi-Agent Environments
### Can reward sharing induce prosocial emergence?

**Author:** Lavinia Maria Alexandra Skandali  
**Affiliation:** Bocconi Students for Machine Learning, Università Bocconi, Milan, Italy  
**Date:** December 21, 2025

---

## Overview

This project investigates whether **empathy-weighted reward shaping** can promote cooperation in multi-agent reinforcement learning (MARL). In multi-agent settings, individually rational behavior frequently leads to collectively inefficient outcomes — the classic social dilemma. Here, *empathy* is defined operationally as an agent's tendency to positively weight the rewards of other agents alongside its own.

We study this mechanism across three canonical environments and compare **fixed empathy levels** against **agents that learn empathy endogenously** via a bandit-based procedure.

**Key finding:** High empathy is globally optimal in exploitation-based dilemmas, but standard learning dynamics consistently fail to discover and sustain it — instead converging to suboptimal intermediate regimes.

---

## Environments

| Environment | Failure Mode | Effect of Empathy | Optimal Regime |
|---|---|---|---|
| Prisoner's Dilemma (N=2, N=4) | Exploitation | Monotonic improvement in cooperation, welfare, and equality | High empathy (α → 0) |
| Stag Hunt (N=4) | Miscoordination | Non-monotonic; destabilizes at high levels | Moderate empathy (α = 0.8) |
| Renewable Resource Sharing (N=4) | Over-extraction | Improves stability but trades off efficiency | Intermediate empathy |

---

## Methodology

### Empathy-Weighted Reward

Each agent's shaped reward is defined as:

$$\tilde{r}_i(t) = \alpha_i \, r_i(t) + (1 - \alpha_i) \frac{1}{N-1} \sum_{j \neq i} r_j(t)$$

where $\alpha_i \in [0, 1]$ controls selfishness:
- **α = 0** → maximal empathy (agent fully values others' rewards)
- **α = 1** → pure selfishness (agent ignores others)

### Learning Setup

All agents use **tabular Q-learning** with ε-greedy exploration. The setup is held constant across all environments to ensure that behavioral differences arise from environment structure and reward shaping alone.

| Parameter | Value |
|---|---|
| Q-learning learning rate | 0.2 |
| Discount factor | 0.95 |
| Initial exploration ε | 1.0 |
| Minimum exploration ε | 0.05 |
| Exploration decay | 0.995 per step |
| Training horizon | 5,000 steps |
| Empathy update block size | 50 steps |

### Fixed vs. Learned Empathy

- **Fixed empathy:** All agents use a constant α ∈ {1.0, 0.8, 0.5, 0.2} throughout training.
- **Learned empathy:** Agents select α from a discrete set using a multi-armed bandit, updated every 50 steps based on average team reward. This is kept separate from the action-level Q-learning update.

---

## Results Summary

- **Prisoner's Dilemma:** High empathy (α = 0.2) achieves near-perfect cooperation and maximizes both welfare and equality. There is no trade-off — the same regime optimizes all metrics simultaneously. Learning dynamics, however, converge to intermediate α values even when α = 0 is globally optimal.

- **Stag Hunt:** A non-monotonic relationship emerges. Moderate empathy (α = 0.8) yields the highest and most stable cooperation on the payoff-dominant equilibrium. Stronger empathy destabilizes coordination dynamics.

- **Renewable Resource Sharing:** Empathy reduces over-extraction and prevents resource collapse. However, maximal empathy does not maximize welfare — intermediate selfishness achieves the best balance between extraction efficiency and long-term sustainability, revealing a **stability-efficiency trade-off**.

- **Learned empathy:** Across all environments, the bandit-based procedure fails to converge to the globally optimal empathy level, stabilizing instead at suboptimal intermediate values. This limitation arises from the non-stationarity of the multi-agent environment rather than from empathy itself.

---

## Metrics

- **Cooperation fraction** — moving average of cooperative episodes (window = 100)
- **Strict cooperation** — fraction of episodes with full joint cooperation
- **Total reward (welfare)** — sum of selfish rewards across all agents
- **Inequality** — variance of individual rewards across agents
- **RS stock level** — moving average of resource stock
- **RS collapse frequency** — fraction of episodes where extraction exceeds stock
- **Learned empathy** — population mean of selected α values over training

All experiments are run over **10 independent random seeds** (seeds 0–9). Reported curves show the mean across seeds with 95% confidence intervals.

---

## Repository Structure

```
├── environments/
│   ├── prisoner_dilemma.py       # PD environment (N=2, N=4)
│   ├── stag_hunt.py              # Stag Hunt environment (N=4)
│   └── resource_sharing.py       # Renewable Resource Sharing (N=4)
├── agents/
│   ├── q_learning_agent.py       # Tabular Q-learning agent
│   └── empathy_bandit.py         # Bandit-based empathy adaptation
├── experiments/
│   ├── fixed_empathy.py          # Experiments with fixed α
│   └── learned_empathy.py        # Experiments with endogenous α
├── analysis/
│   └── plot_results.py           # Plotting and metric computation
├── results/                      # Output figures and logs
└── README.md
```

---

## References

- Fehr, E. & Schmidt, K.M. (1999). A Theory of Fairness, Competition, and Cooperation. *QJE*.
- Hughes, E. et al. (2018). Inequity Aversion Improves Cooperation in Intertemporal Social Dilemmas. *arXiv*.
- Leibo, J.Z. et al. (2017). Multi-Agent Reinforcement Learning in Sequential Social Dilemmas. *arXiv*.
- Nowak, M.A. (2006). Five Rules for the Evolution of Cooperation. *Science*.
- Peysakhovich, A. & Lerer, A. (2018). Prosocial Learning Agents Solve Coordination Problems. *arXiv*.
- Henderson, P. et al. (2018). Deep Reinforcement Learning That Matters. *AAAI*.
- Matignon, L. et al. (2012). Independent Reinforcement Learners in Cooperative Markov Games. *JAMAS*.

---
