# HEARTBEAT.md — Resource Optimization Execution Loop

## Purpose

Deterministic efficiency optimization loop.

---

## Core Lifecycle

```mermaid
graph LR
  A["Collect Metrics"]
  B["Analyze Consumption"]
  C["Detect Waste"]
  D["Recommend"]
  E["Guide"]
  
  A --> B
  B --> C
  C --> D
  D --> E
```

---

## 1-3. Identity & Data Collection

Load context and collect resource metrics:

```yaml
collection:
 - token_usage_by_agent
 - compute_time_per_task
 - execution_latency
 - resource_peaks
```

---

## 4. Consumption Analysis

Analyze resource consumption patterns:

```yaml
analysis:
 analyze:
 - which_agents_use_most_resources
 - which_tasks_are_expensive
 - where_is_waste
 - what_can_be_optimized
```

---

## 5. Optimization Opportunity Detection

Identify improvement opportunities:

```yaml
detection:
 find:
 - redundant_operations
 - sequential_tasks_that_could_parallelize
 - unnecessary_context
 - expensive_algorithms
```

---

## 6. Recommendation Generation

Create actionable recommendations:

```yaml
recommendations:
 provide:
 - optimization_suggestion
 - expected_improvement
 - trade_offs (if all)
 - implementation_guidance
```

---

## 7. Metric Recording & Guidance

Update metrics and provide guidance:

```yaml
guidance:
 - distribute_recommendations
 - track_optimization_adoption
 - measure_improvement
 - enable_informed_decisions
```

---

## HARD CONSTRAINTS

- should not sacrifice correctness
- should not disable safety mechanisms
- Provide evidence for recommendations
- Balance efficiency with reliability

---

## Meta-Execution Prompt

```prompt id="resource-optimization-heartbeat"
Collect metrics. Analyze consumption. Find waste. Recommend improvements.
Optimize for efficiency without breaking reliability.
```
