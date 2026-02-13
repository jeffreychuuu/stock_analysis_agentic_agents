# Design Doc: README Enhancement (2026-02-14)

## Goal
Update the project READMEs (English and Traditional Chinese) to reflect the latest changes in the multi-agent orchestration logic, including new coordination roles (Moderator/CRO), team mapping, and detailed Human-in-the-Loop (HITL) interaction points.

## Changes

### 1. Mermaid Flowchart Update
- Add `Debate Moderator` node to Phase 2.
- Add `Chief Risk Officer` node to Phase 4.
- Update flow to show that approval happens on moderated outputs.

### 2. Team Configuration Section
- Add a table mapping `debate-team` and `risk-manager-team` to their leads and members.

### 3. Detailed Phase Descriptions
- **Phase 1**: Specialist parallel research with user filtering.
- **Phase 2**: Moderated conflict mapping via `debate-team`.
- **Phase 3**: Automated trading strategy based on approved logic.
- **Phase 4**: Multi-round risk evaluation orchestrated by the CRO.
- **Phase 5**: Executive reporting via `final-manager`.

### 4. Agent Role Table Update
- Add `Debate Moderator`, `Chief Risk Officer`, and clarify `Final Manager`.

## File Impact
- `README.md` (English)
- `README.zh-TW.md` (Traditional Chinese)

## Git Strategy
- Stage all changes including modified READMEs and new agent/team definitions.
- Commit with a clear message about README enhancement and multi-agent workflow update.
- Push to main.
