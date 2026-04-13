# AGENTS.md

**Depth**: 0 (root level)  
**Purpose**: Workspace-level agent delegation and system overview

---

## Agent Types

This workspace defines the following agent types for delegation:

| Agent | Purpose |
|-------|---------|
| `explore` | Research, discovery, codebase exploration |
| `librarian` | Documentation retrieval, knowledge lookup |
| `oracle` | Architecture, design, technical decision guidance |
| `hephaestus` | Build, compile, implementation |
| `metis` | Planning, task decomposition, roadmap creation |
| `momus` | Review, validation, quality assurance |

---

## Execution Patterns

### Task Delegation

When delegating work to agents, use the Task tool with the appropriate agent type:

- **Research tasks** → `explore` or `librarian`
- **Design tasks** → `oracle`
- **Implementation tasks** → `hephaestus`
- **Planning tasks** → `metis`
- **Review tasks** → `momus`

### Context Loading

Agents load context based on their purpose:

- Explore agents: Full codebase access for research
- Librarian agents: Documentation and knowledge bases
- Oracle agents: Architecture docs, project state
- Hephaestus agents: Implementation context, specs
- Metis agents: Project goals, roadmap, existing tasks
- Momus agents: Changes to review, validation criteria

---

## System Mapping

### Directory Structure

| Directory | Purpose |
|-----------|----------|
| `./agents/` | Agent configurations and prompts for this workspace |
| `./instructions/` | Agent instruction documents and governance rules |

---

## Governance Principles

### Global Rules

1. **Intent Formalization**: Before delegating any work, extract goal, success_criteria, constraints, and risks
2. **Generator/Evaluator Separation**: Generators must not self-validate; external evaluation is mandatory
3. **Memory Discipline**: Reload state each cycle; never rely on prompt memory
4. **Execution Isolation**: Zero-trust execution with ephemeral environments
5. **Failure Classification**: All failures must be classified (transient, deterministic, constraint_violation, system_drift, resource_failure)

### Multi-Layer Validation

The system enforces validation through:

- **Evaluator** (correctness)
- **Alignment Agent** (intent fidelity)
- **Constraint Engine** (rule compliance)

---

## Required Reading

All agents should have access to:

- `HEARTBEAT.md` (execution loop)
- `SOUL.md` (behavioral identity)
- `TOOLS.md` (capabilities)

---

## Subdirectories

### `./agents/AGENTS.md`

Contains concrete agent configurations for this workspace, including skill definitions and task routing logic.

### `./instructions/AGENTS.md`

Contains instruction documents for agents, including governance rules extracted from the system architecture.

---

*This file defines the workspace-level delegation model. Agent implementations and instruction sets are defined in subdirectories.*
