# Claude Code Setup & Agent Teams

This project leverages Claude Code's **Agent Teams** feature to perform complex, multi-perspective stock analysis.

## 🛠️ Setup

To enable parallel agent teams and full functionality, ensure you have the experimental flag enabled in your environment:

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=true
```

## 👥 Available Agent Teams

The project defines two specialized teams in `.claude/teams/`:

1.  **Debate Team (`debate-team`)**
    - **Members**: `bullish-analyst`, `bearish-analyst`, `debate-moderator`
    - **Purpose**: Conducts adversarial debates on stock prospects, guided by a neutral moderator to map out core conflicts.

2.  **Risk Manager Team (`risk-manager-team`)**
    - **Members**: `aggressive-risk-manager`, `neutral-risk-manager`, `conservative-risk-manager`, `chief-risk-officer`
    - _Note_: In `risk-manager-team`, the `chief-risk-officer` acts as the coordinator/moderator.
    - **Purpose**: Evaluates trading strategies from multiple risk appetites, overseen by the Chief Risk Officer.

## 🛠️ Reliability & Troubleshooting

To ensure smooth team operations:

- **Verify Existence**: Before calling `TeamCreate`, always use `ls .claude/teams/` and `ls .claude/agents/` to verify the definition files exist and match the intended names.
- **Explicit Naming**: Never guess or shorten team/agent names. Use the exact strings defined in the mapping tables.
- **Context Preservation**: When spawning agents via `Task`, ensure the `prompt` contains enough context from previous "Human-in-the-Loop" approvals.

## 🚀 Workflow

The analysis is orchestrated via the `multi-agent-trading-analysis` skill.

### How to run:

1.  Run the skill: `multi-agent-trading-analysis`
2.  Input the target stock **Ticker** (e.g., `AAPL`).
3.  **Human-in-the-Loop**: You will be asked to approve, reject, or modify agent outputs at each phase:
    - **Phase 1**: Research (Fundamental, Technical, Sentiment)
    - **Phase 2**: Debate (Bull vs Bear, moderated)
    - **Phase 3**: Strategy (Trader)
    - **Phase 4**: Risk Evaluation (Team-based)
    - **Phase 5**: Final Decision (Executive Summary)

## 📁 Project Structure

- `.claude/agents/`: Individual agent definitions.
- `.claude/teams/`: Team compositions and roles.
- `.claude/skills/`: Orchestration logic for the HITL workflow.
