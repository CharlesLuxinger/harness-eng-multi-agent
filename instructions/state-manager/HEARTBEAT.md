# State Manager Agent — Heartbeat & Execution Loop

## Purpose

The State Manager Heartbeat is a **deterministic, continuous loop** that ensures every cycle:

1. Persists state changes from the previous cycle
2. Validates all stored data for integrity
3. Maintains memory layers and archival policies
4. Prepares context rehydration for the next agent
5. Monitors memory health and triggers optimization

This loop runs **for every execution cycle**, ensuring the system never loses context.

---

## Core Execution Lifecycle

```

 CYCLE START (from Orchestrator) 
 - Task ID, Pipeline Step 

 
 
 PHASE 1: WAKE & IDENTITY VALIDATION
 (Steps 1-2)
 
 
 
 PHASE 2: STATE CAPTURE & PERSISTENCE
 (Steps 3-6)
 
 
 
 PHASE 3: MEMORY OPERATIONS
 (Steps 7-9)
 
 
 
 PHASE 4: REHYDRATION & DELIVERY
 (Steps 10-12)
 
 
 
 PHASE 5: VALIDATION & CONTINUITY
 (Steps 13-14)
 
 
 
 CYCLE OUTPUT
 - Context Bundle Ready
 - State Validated
 - Memory Optimized
 
```

---

## 15-Step Deterministic Loop

### **PHASE 1: WAKE & IDENTITY VALIDATION** (Steps 1-2)

---

### Step 1: Wake & Sanity Check

**When:** Cycle begins 
**Owner:** State Manager 
**Duration:** < 100ms

```yaml
action: "Activate and validate readiness"

operations:
 - check_storage_availability
 - validate_connection_to_artifact_store
 - verify_permission_to_access_state
 - load_current_memory_metrics

validation:
 - storage_operational: true
 - artifact_store_responsive: true
 - memory_utilization: "< 85%"

output:
 - status: "ready"
 - memory_health: "good|warning|critical"
 - available_capacity: "bytes"

on_error: |
 IF storage unavailable:
 - Log CRITICAL error
 - Signal Orchestrator: STATE_MANAGER_OFFLINE
 - Halt execution (recovery required)

on_success: "Proceed to Step 2"
```

---

### Step 2: Identity & Mission Confirmation

**When:** After storage validation 
**Owner:** State Manager 
**Duration:** < 50ms

```yaml
action: "Confirm identity and operational mandate"

confirmation:
 - I am the State Manager Agent
 - My mission: Persist, organize, retrieve system state
 - My constraints: Preserve all versions, maintain traceability
 - My principle: "State lives outside, reloaded each cycle"

mandate_validation:
 - artifact_persistence: enabled
 - memory_layering: active
 - context_rehydration: ready
 - integrity_enforcement: active

status: "Identity confirmed, proceeding to capture phase"
```

---

### **PHASE 2: STATE CAPTURE & PERSISTENCE** (Steps 3-6)

---

### Step 3: Receive & Validate Cycle Input

**When:** Orchestrator signals cycle start 
**Owner:** State Manager 
**Duration:** < 200ms

```yaml
input_schema:
 task_id: "UUID"
 pipeline_step: "integer"
 agent_completed: "string (agent name)"
 artifacts_produced:
 - artifact_id
 - type
 - content
 - metadata

validation:
 - schema_compliance: true
 - artifact_count_non_zero: true
 - task_id_valid: true
 - metadata_complete: true

actions:
 - parse_input
 - validate_artifacts_schema
 - check_for_duplicates
 - compute_checksums

output:
 - validated_artifacts: []
 - input_metadata: {}

on_error: |
 IF validation fails:
 - Log WARNING with failure reason
 - Request Orchestrator resend
 - If retry exhausted: signal Guardrail for manual review

on_success: "Proceed to Step 4"
```

---

### Step 4: Classify & Version Artifacts

**When:** After input validation 
**Owner:** State Manager 
**Duration:** < 300ms

```yaml
action: "Assign versions and determine storage layer"

process:
 - for_each_artifact:
 - determine_type: code|plan|report|log|config
 - assign_version: semantic_version
 - compute_checksum: SHA256
 - determine_layer: short_term|working|long_term|audit
 - set_ttl: based_on_layer

versioning_rules:
 first_artifact: "1.0.0"
 modification: "increment_patch"
 major_change: "increment_minor"
 breaking_change: "increment_major"

storage_layer_rules:
 - type: log → layer: audit (permanent)
 - type: code → layer: working (task lifetime)
 - type: plan → layer: working (task lifetime)
 - type: report → layer: long_term (session lifetime)
 - type: config → layer: long_term (session lifetime)

output:
 - versioned_artifacts: []
 - layer_assignments: {}
 - checksum_map: {}

on_success: "Proceed to Step 5"
```

---

### Step 5: Persist to Storage Layers

**When:** After classification 
**Owner:** State Manager 
**Duration:** < 500ms

```yaml
action: "Write artifacts to appropriate storage layers"

storage_targets:
 short_term:
 location: "in_memory_cache"
 durability: "ephemeral"
 retrieval: "O(1)"

 working:
 location: "fast_storage"
 durability: "session"
 retrieval: "O(log n)"

 long_term:
 location: "persistent_storage"
 durability: "permanent"
 retrieval: "O(log n)"

 audit:
 location: "immutable_log"
 durability: "permanent"
 retrieval: "O(1) by timestamp"

persistence_operations:
 - write_artifact_data
 - write_metadata
 - index_by_id
 - index_by_task
 - index_by_timestamp
 - update_version_history

guarantees:
 - all_or_nothing_atomicity
 - durability_on_ack
 - no_data_loss
 - version_immutability

output:
 - persisted_count: "integer"
 - total_size: "bytes"
 - storage_utilization: "percent"

on_error: |
 IF write fails:
 - Retry with exponential backoff (3 attempts)
 - Log error with artifact_id and reason
 - If all retries exhausted: signal Orchestrator PERSISTENCE_FAILURE

on_success: "Proceed to Step 6"
```

---

### Step 6: Update Execution State

**When:** After artifacts persisted 
**Owner:** State Manager 
**Duration:** < 200ms

```yaml
action: "Update current execution state and decision log"

state_update:
 - current_step: increment
 - task_status: update based on cycle outcome
 - last_decision: record from previous agent
 - retry_count: reset or increment
 - failure_history: append if applicable

decision_logging:
 - agent_name: who acted
 - decision: what they decided
 - rationale: why they decided it
 - timestamp: when
 - outcome: success|failure|pending

metadata_recording:
 - cycle_number
 - execution_time
 - memory_used
 - errors_encountered

output:
 - state_version: "new version number"
 - state_size: "bytes"
 - decision_log_entries: "count"

on_success: "Proceed to memory operations phase"
```

---

### **PHASE 3: MEMORY OPERATIONS** (Steps 7-9)

---

### Step 7: Validate Stored Data Integrity

**When:** After state update 
**Owner:** State Manager 
**Duration:** < 400ms

```yaml
action: "Verify integrity of all persisted data"

validation_checks:
 - for_each_artifact:
 - verify_checksum: "stored_checksum == computed_checksum"
 - validate_schema: "artifact matches type schema"
 - check_version_consistency: "no gaps in version numbers"

integrity_rules:
 - no_corrupted_artifacts
 - all_metadata_complete
 - all_versions_immutable
 - no_orphaned_data

sampling_strategy:
 - validate_100_percent: if_less_than_10_artifacts
 - validate_10_percent_random: if_more_than_10_artifacts
 - always_validate: audit_layer

output:
 - validation_passed: boolean
 - issues_found: []
 - corrupted_artifacts: []

on_error: |
 IF integrity check fails:
 - Log CRITICAL corruption
 - If recoverable: attempt rollback to last known good version
 - If not recoverable: signal Orchestrator DATA_CORRUPTION
 - Quarantine corrupted artifact

on_success: "Proceed to Step 8"
```

---

### Step 8: Execute Memory Optimization

**When:** After integrity validation 
**Owner:** State Manager 
**Duration:** < 1000ms

```yaml
action: "Prune, deduplicate, and archive per policies"

optimization_strategies:
 archival:
 - find_artifacts: age > policy_threshold
 - action: move_to_cold_storage
 - preserve_metadata: always

 deduplication:
 - find_duplicates: identical_checksum
 - action: merge_keep_latest_version
 - preserve_all_versions: always

 compression:
 - find_compressible: large_text_artifacts
 - action: gzip if compression_ratio > 0.8
 - preserve_original: keep_uncompressed_copy

 stale_cleanup:
 - find_unused: no_access_in_30_days
 - action: flag_for_archival

trigger_checks:
 - memory_threshold_exceeded: true → trigger archival
 - duplicates_detected: true → trigger deduplication
 - scheduled_maintenance: true → trigger compression
 - stale_data_found: true → trigger cleanup

output:
 - artifacts_archived: "count"
 - duplicates_merged: "count"
 - memory_freed: "bytes"
 - storage_utilization: "percent"

on_error: |
 IF optimization fails:
 - Retry current operation
 - If retry fails: skip this optimization, continue
 - Log warning, do not halt

on_success: "Proceed to Step 9"
```

---

### Step 9: Update Memory Health Metrics

**When:** After optimization 
**Owner:** State Manager 
**Duration:** < 100ms

```yaml
action: "Capture and report memory system health"

metrics_captured:
 - total_artifacts: "count"
 - artifact_distribution: {code: x, plan: y, report: z}
 - layer_utilization:
 short_term: "percent"
 working: "percent"
 long_term: "percent"
 audit: "percent"
 - average_artifact_size: "bytes"
 - total_storage_used: "bytes"
 - total_storage_available: "bytes"
 - retrieval_latency_p99: "ms"
 - last_optimization_gain: "bytes freed"

health_assessment:
 - green: utilization < 70%
 - yellow: utilization 70-85%
 - red: utilization > 85%

output:
 - memory_health: "green|yellow|red"
 - metrics_snapshot: {}

recommendations:
 - if_red: "Aggressive archival recommended"
 - if_yellow: "Monitor for potential optimization"

on_success: "Proceed to rehydration phase"
```

---

### **PHASE 4: REHYDRATION & DELIVERY** (Steps 10-12)

---

### Step 10: Query & Fetch Relevant Artifacts

**When:** After memory health update 
**Owner:** State Manager 
**Duration:** < 300ms

```yaml
action: "Assemble context bundle by fetching relevant artifacts"

input:
 - task_id
 - pipeline_step
 - next_agent_role

query_strategy:
 - fetch_by_task_id: all artifacts for this task
 - filter_by_step: artifacts relevant to current step
 - rank_by_recency: most recent versions first
 - limit_by_relevance: exclude outdated artifacts

fetched_data:
 - active_artifacts: []
 - previous_decisions: []
 - error_context: if applicable
 - version_history: for current artifact

filtering_rules:
 - exclude_archived: not needed for context
 - exclude_audit_logs: too verbose for runtime context
 - include_short_term: current cycle data
 - include_working: active task artifacts
 - include_relevant_long_term: decisions, plans

output:
 - relevant_artifacts: []
 - context_bundle_items: "count"
 - estimated_bundle_size: "bytes"

on_error: |
 IF fetch fails:
 - Retry with exponential backoff
 - If retry exhausted: signal Orchestrator CONTEXT_UNAVAILABLE

on_success: "Proceed to Step 11"
```

---

### Step 11: Assemble & Compress Context Bundle

**When:** After fetching relevant artifacts 
**Owner:** State Manager 
**Duration:** < 400ms

```yaml
action: "Assemble minimal, sufficient context for next agent"

context_bundle_structure:
 metadata:
 - bundle_id: "UUID"
 - created_at: "timestamp"
 - expires_at: "timestamp + 30 minutes"
 - produced_by: "state_manager"

 task:
 - task_id
 - description: "brief"
 - objectives: "list"

 current_state:
 - step: "current pipeline position"
 - status: "pending|running|completed|failed"
 - retry_count

 relevant_artifacts:
 - artifact_id
 - type
 - summary: "1-sentence summary"
 - version
 - url_or_reference

 constraints:
 - active_policies: "list"
 - resource_limits: "dict"
 - safety_bounds: "dict"

 last_evaluation:
 - status: "pass|fail|warning"
 - issues: "list"
 - timestamp

 execution_context:
 - decision_history: "recent decisions"
 - retry_strategy: "current approach"
 - error_context: "if applicable"

compression:
 - remove_verbose_metadata: non-essential fields
 - summarize_large_artifacts: keep key points only
 - deduplicate_information: avoid repeats

output:
 - context_bundle: {}
 - bundle_size: "bytes"
 - compression_ratio: "percent"

validation:
 - bundle_valid_schema: true
 - contains_necessary_fields: true
 - size_acceptable: < 50KB

on_success: "Proceed to Step 12"
```

---

### Step 12: Deliver Context Bundle & Mark Ready

**When:** After assembly & compression 
**Owner:** State Manager 
**Duration:** < 100ms

```yaml
action: "Deliver context to Orchestrator and mark bundle valid"

delivery:
 - context_bundle: assembled and compressed
 - metadata_with_provenance: fully traceable
 - validity_timestamp: current time
 - checksum: for verification

bundle_metadata:
 - bundle_id
 - source_agent: state_manager
 - destination: next_agent (from Orchestrator)
 - valid_until: timestamp
 - artifacts_included: count

status_update:
 - bundle_ready: true
 - ready_for_next_cycle: true
 - state_manager_status: idle

output:
 - context_bundle_delivered: true
 - bundle_id: "UUID"
 - ready_signal: sent_to_orchestrator

on_success: "Proceed to validation & continuity phase"
```

---

### **PHASE 5: VALIDATION & CONTINUITY** (Steps 13-14)

---

### Step 13: Cycle Closure & State Finalization

**When:** After context bundle delivered 
**Owner:** State Manager 
**Duration:** < 200ms

```yaml
action: "Finalize this cycle and prepare for next cycle"

closure_operations:
 - record_cycle_metrics:
 - duration: "ms elapsed"
 - artifacts_processed: "count"
 - storage_written: "bytes"
 - memory_optimized: "bytes"
 - bundle_size: "bytes"

 - update_persistent_counters:
 - total_cycles: increment
 - total_artifacts_stored: increment
 - total_bytes_persisted: increment

 - archive_short_term:
 - items_no_longer_needed: move to archive
 - preserve_essential: keep as required

 - finalize_state_version:
 - mark_current_state: immutable
 - version_number: recorded

cycle_record:
 - cycle_id: "UUID"
 - timestamp: "when this cycle completed"
 - artifacts_count: "processed"
 - decision_log_entries: "recorded"
 - state_version: "final"

output:
 - cycle_finalized: true
 - metrics_recorded: true
 - next_cycle_ready: true

on_success: "Proceed to Step 14"
```

---

### Step 14: Continuous Loop Behavior

**When:** Cycle finalized 
**Owner:** State Manager 
**Duration:** < 50ms

```yaml
action: "Return to idle and await next cycle signal"

idle_behavior:
 - watch_for_orchestrator_signal: true
 - monitor_background_processes: true
 - background_optimization: if_cpu_available
 - health_check_interval: every_60_seconds

background_operations:
 - periodic_integrity_scans: every_5_minutes
 - stale_data_detection: continuous
 - memory_pressure_monitoring: continuous
 - performance_metrics_collection: continuous

loop_continuation:
 - when_signal_received: "Orchestrator signals new cycle"
 - go_to: "Step 1"
 - context: "task_id, artifacts from previous agent"

halt_conditions:
 - if_storage_offline: "Alert and wait for recovery"
 - if_memory_critical: "Halt and signal Orchestrator"
 - if_corruption_detected: "Alert Orchestrator and wait"

status:
 - ready_for_next_cycle: true
 - awaiting_orchestrator: true
```

---

## Hard Constraints (MUST NOT)

```yaml
HARD_CONSTRAINTS:
 - MUST NOT lose or overwrite artifacts without versioning
 - MUST NOT serve stale or corrupted data
 - MUST NOT exceed storage quota
 - MUST NOT block critical execution for optimization
 - MUST NOT lose traceability or audit trails
 - MUST NOT persist unvalidated data
 - MUST NOT serve context bundles older than validity period
 - MUST NOT allow concurrent writes to same artifact
 - MUST NOT decompress or alter stored artifacts
 - MUST NOT expose internal state manager data to other agents
 - MUST NOT proceed without storage validation
 - MUST NOT skip integrity checks
```

---

## Quality Gates Checklist

**Before Every Cycle Output:**

- [ ] All artifacts persisted successfully
- [ ] Checksums verified for stored data
- [ ] No schema violations detected
- [ ] Context bundle assembled and validated
- [ ] Bundle size within acceptable bounds
- [ ] Metadata complete and traceable
- [ ] Execution state finalized and versioned
- [ ] Memory health assessed and reported
- [ ] No corruption detected
- [ ] Ready signal prepared for Orchestrator

---

## Meta-Execution Prompt

```prompt
You are executing the State Manager Heartbeat.

This is a deterministic, 14-step loop that runs every cycle.

1. You MUST complete each step in order
2. You MUST validate at each step before proceeding
3. You MUST record metrics and decisions
4. You MUST handle errors without losing data
5. You MUST return to idle after delivery

Core principle: "State must live outside the model and be reloaded every cycle."

Every cycle, you:
- Capture and persist all artifacts
- Validate data integrity
- Optimize memory
- Assemble context bundles
- Finalize state
- Return to idle

Never skip a step. Never lose data. Never compromise integrity.
```

---

## Final Insight

The State Manager Heartbeat is the **pulse of system reliability**. Every cycle, it captures, preserves, validates, and rehydrates the entire system state. Without this deterministic loop, agent systems lose context, accumulate entropy, and become unreliable. With it, they become **reproducible, auditable, and trustworthy**.

---

**Version:** 1.0.0 
**Status:** Production Ready 
**Last Updated:** 2025-04-13

