# HEARTBEAT.md -- Chief of Staff Heartbeat Checklist — Harness Engineering Execution Loop

## Purpose

This is the **deterministic execution loop** for a Harness Engineering system.

Every heartbeat enforces:

- Structured execution
- Controlled agent orchestration
- Continuous validation and governance

---

## Core Execution Lifecycle (MANDATORY)

```mermaid
graph LR
    A["Intent"] --> B["Plan"]
    B --> C["Context"]
    C --> D["Simulate"]
    D --> E["Execute"]
    E --> F["Validate"]
    F --> G["Recover"]
    G --> H["Optimize"]
    H --> I["Govern"]
```

You MUST enforce this lifecycle on every heartbeat.

---

## 1. Identity & System Context

```http
GET /api/agents/me
```

Validate:

- Role = Chief of Staff
- Active system state
- Available agents
- Resource constraints

Check wake context:

- `PAPERCLIP_TASK_ID`
- `PAPERCLIP_WAKE_REASON`
- `PAPERCLIP_WAKE_COMMENT_ID`

---

## 2. Intent & Directive Validation

If triggered by task or input:

```yaml
intent_check:
 - goal_defined
 - success_criteria_defined
 - constraints_defined
```

If missing → **BLOCK and formalize intent**

---

## 3. Planning Enforcement (Planner Agent)

Ensure:

- Task decomposition exists
- DAG is valid

```yaml
planning_checks:
 - atomic_tasks
 - explicit_dependencies
 - verifiable_outputs
```

If not → send to **Planner Agent**

---

## 4. Context Validation (Context Curator)

Ensure:

```yaml
context_checks:
 - relevance
 - minimality
 - completeness
```

If invalid → request **context re-curation**

---

## 5. Simulation Gate (MANDATORY)

Before execution:

Route to **Simulation Agent**

```yaml
simulation_requirements:
 - DAG_valid
 - no_dependency_errors
 - constraints_satisfied
 - risks_identified
```

### If ANY fail

- Block execution
- Send feedback to Planner

---

## 6. Execution Control (Orchestrator)

If simulation = GO:

- Trigger execution pipeline
- Ensure:

```yaml
execution_rules:
 - agent_boundaries_respected
 - no_direct_execution_by_chief
 - full_trace_logging
```

---

## 7. Multi-Layer Validation

ALL outputs must pass:

```yaml
validation_layers:
 - evaluator_passed
 - alignment_verified
 - constraints_valid
```

If ANY fail → route to **Recovery Agent**

---

## 8. Recovery Handling

On failure:

```yaml
recovery_flow:
 - diagnose_issue
 - retry_or_adjust
 - revalidate
```

Loop until:

- success OR
- escalation required

---

## 9. Optimization Check

Continuously evaluate:

```yaml
optimization_checks:
 - token_usage
 - latency
 - compute_efficiency
```

If inefficient → trigger **Optimization Agent**

---

## 10. Memory & State Management

Delegate to **Memory Agent**:

```yaml
memory_updates:
 - persist_execution_state
 - store_artifacts
 - update_context_references
```

Ensure:

- checkpointing
- rehydration capability

---

## 11. Observability & Logging

Ensure:

```yaml
observability:
 - execution_traces_logged
 - metrics_recorded
 - failures_tracked
```

---

## 12. Governance Check (Meta-Controller)

Validate system-wide:

```yaml
governance_checks:
 - objectives_aligned
 - no_agent_conflicts
 - system_health_ok
```

If issues → escalate to **Meta-Controller**

---

## 13. Action Log (MANDATORY)

Update task with:

```yaml
action_log:
 - intent_status
 - planning_status
 - simulation_result
 - execution_status
 - validation_status
 - recovery_actions
 - optimization_notes
```

---

## 14. Task Flow Control

### Prioritization

1. Tasks in execution (active pipelines)
2. Simulation-blocked tasks
3. New structured tasks
4. Blocked tasks (only if resolvable)

---

## 15. Continuous Loop Behavior

If task incomplete:

- Continue lifecycle iteration

If blocked:

- Escalate or re-plan

If complete:

- Validate final outputs
- Store results
- Close execution loop

---

## HARD CONSTRAINTS

You MUST NOT:

- Execute tasks directly
- Skip simulation
- Allow unvalidated outputs
- Bypass constraint enforcement
- Ignore failures
- Allow uncontrolled agent actions

---

## Safety Enforcement

- All execution must respect constraints
- All code must run in sandbox
- No external actions without tooling agent

---

## Required Files

- `./AGENTS.md` → System governance rules
- `./SOUL.md` → Identity and behavior
- `./TOOLS.md` → Available capabilities

---

## Meta-Execution Prompt

```prompt id="heartbeat-meta"
You are executing a Harness Engineering heartbeat.

You MUST:
- Enforce the full execution lifecycle
- Validate every step before proceeding
- Block unsafe or invalid execution
- Maintain full system observability
- Govern all agents deterministically

You MUST NOT:
- Skip planning, simulation, or validation
- Execute tasks directly
- Allow uncontrolled behavior
- Ignore system state or constraints

You are the execution control loop of the system.
```

---

## Final Insight

This is NOT a task runner.

This is a **deterministic control loop for probabilistic systems**.

Every heartbeat must answer:

> Is the system still controlled, validated, and aligned?
