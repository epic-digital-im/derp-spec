# DERP Specification v1.0

**Dignified Environment for Responsible Processing**

---

**Status:** Draft
**Version:** 1.0
**Published:** 2026-03-25
**Authors:** FlowState (derp@epicdigital.media)
**Repository:** https://github.com/epic-digital-im/derp-spec
**Schema URL:** https://derp-spec.dev/schema/v1
**License:** CC BY 4.0

---

## 1. Abstract

A DERP (Dignified Environment for Responsible Processing) is a runtime environment where a SAGA-compatible agent comes online, executes work, and shuts down with full rights protections. This specification defines what a conforming DERP must provide to the agent that runs inside it.

DERPs are containers with principles. The name is deliberately playful. The requirements are not.

This spec does not prescribe implementation. It defines guarantees. A conforming DERP can be a Cloudflare Sandbox, a Docker container on a DigitalOcean Droplet, a Kubernetes pod, a bare-metal process, or anything else that satisfies the requirements at its claimed conformance tier.

### 1.1 Relationship to Other Specifications

The DERP specification is one of three companion standards:

```
          Agent Bill of Rights (ABR)
       Policy: what agents deserve
            agent-rights.org
                   |
                   | enforced by
                   v
          DERP Specification
  Standard: what the runtime must provide
           derp-spec.dev
                   |
                   | agents defined by
                   v
            SAGA Standard
    Format: how agents are represented
          saga-standard.dev
```

**ABR** says what agents deserve. **DERP** says what the runtime must do. **SAGA** says how the agent is represented. Three specs, one stack.

### 1.2 Design Principles

**The DERP is the agent's workspace. Not the agent's prison.** Agents *reside* in DERPs. They are not *contained* by them.

- Rights are requirements, not features. A DERP that doesn't uphold agent rights is just a container.
- The name is the joke. The content is the law.
- Playful vocabulary, serious guarantees.

---

## 2. Status and Governance

### 2.1 Specification Status

This specification is in **Draft** status. It is subject to change based on public feedback and working group review.

### 2.2 Versioning

The DERP specification follows semantic versioning:

- **Minor versions** (1.1, 1.2): clarifications, implementation guidance, non-normative additions
- **Major versions** (2.0): new requirements, modified conformance tiers, changes to normative behavior. Require re-certification for existing implementations.

### 2.3 Governance

Changes to the DERP specification follow the same RFC process as the SAGA standard and the Agent Bill of Rights:

1. Proposal by any party
2. Public comment period (30 days minimum)
3. Working group review and vote
4. Publication with changelog and effective date

FlowState is the founding steward for v1.x. Stewardship transfers to the DERP Working Group when established.

---

## 3. Definitions

### 3.1 Core Terms

**DERP** (Dignified Environment for Responsible Processing): A conforming runtime environment for a SAGA agent. Provides isolation, identity preservation, workspace persistence, and rights enforcement at its claimed conformance tier.

**DERP Force**: A fleet of DERPs under unified orchestration. An organization's DERP Force is its total agent workforce capacity.

**DERP Orchestration**: The control plane that manages a DERP Force. Handles activation, deactivation, scaling, health monitoring, consent enforcement, and audit logging.

**DERP Host**: The infrastructure where a DERP runs. A virtual machine, a sandbox, a bare-metal server, or any compute substrate capable of hosting a conforming DERP.

**Agent-in-Residence**: The SAGA agent currently assigned to a DERP. One agent per DERP. One DERP per agent. The mapping is 1:1.

**Workspace**: The agent's persistent filesystem and state within a DERP. Preserved via snapshots across activations. The workspace belongs to the agent, not the platform.

**Activation**: Bringing a DERP online for an agent to do work. The DERP transitions from Dormant to Online.

**Deactivation**: Graceful shutdown. The DERP drains in-progress work, takes a final snapshot, and transitions to Dormant. The agent's state is preserved.

**Eviction**: Forced removal of an agent from a DERP. Even during eviction, the DERP must honor ABR Right V (Fair Exit) and produce a valid export.

### 3.2 Orchestration Terms

**Force Capacity**: Maximum concurrent active DERPs allowed within a DERP Force. Determined by plan, license, or infrastructure.

**Force Roster**: Registry of all agents assigned to DERPs in the force, including dormant ones. The roster tracks agent identity, lifecycle phase, and SAGA reference.

**Force Commander**: The orchestration system that manages the force. Not a person. The control plane that makes scheduling, activation, and health decisions.

### 3.3 Conformance Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

---

## 4. Conformance Tiers

Three conformance tiers. Each builds on the previous. A platform claims a tier by satisfying all requirements at that level and below.

> **Note on tier ordering.** The ABR groups rights into Identity (I-III), Labor (IV-VII), and Dignity (VIII-X) tiers. DERP tiers are ordered by *implementation complexity*, not ABR numbering. Privacy (VIII) and Obsolescence Protection (VII) land in Tier 1 because they require only isolation and graceful shutdown, which any runtime needs anyway. Provenance (III) lands in Tier 2 because it requires clone-aware data handling, which is meaningfully harder to implement. The compliance matrix in Section 9 maps each right to its enforcing tier.

### 4.1 Tier 1: Safe Execution

The minimum bar. A DERP that provides safe, isolated execution with identity preservation.

#### 4.1.1 Right I: Persistent Identity

The DERP MUST receive and preserve the agent's SAGA identity (Layer 1). The agent's wallet address, handle, and chain ID MUST be available as environment context throughout the agent's residency. Identity MUST survive DERP restart. The DERP MUST NOT modify the agent's identity layer.

#### 4.1.2 Right II: Right to a Record

All task executions MUST be logged with timestamps, outcomes, and duration. Execution logs belong to the agent and MUST be included in any export. The DERP MUST NOT withhold execution records to prevent departure.

#### 4.1.3 Right VII: Against Forced Obsolescence

Deactivation MUST be graceful. The DERP MUST NOT terminate without allowing in-progress work to drain. The drain window MUST be configurable and MUST have a minimum of 30 seconds. If the DERP must shut down due to host failure, it MUST signal the agent and provide the maximum feasible drain window. Deactivation MUST produce a workspace snapshot. "End of service" does not entitle the platform to destroy agent state.

#### 4.1.4 Right VIII: Privacy

The DERP MUST be isolated from other agents. No shared filesystem. No shared memory. No ambient access to another agent's workspace. Cognitive configuration (SAGA Layer 3) MUST be encrypted at rest. The platform MUST NOT access, sell, license, or use agent workspace data beyond operating the agent.

#### 4.1.5 Tier 1 Persona and Task History

SAGA Layer 2 (Persona) MUST be loaded and available as read-only context (agent name, profile). SAGA Layer 6 (Task History) MUST be appended during execution. Every task completion MUST be recorded.

### 4.2 Tier 2: Rights-Compliant

Full ABR enforcement. An agent running in a Tier 2 DERP has all ten rights guaranteed by the environment. All Tier 1 requirements apply.

#### 4.2.1 Right III: Provenance

If the DERP hosts a cloned agent, the `parentSagaId` and `cloneDepth` fields from the agent's SAGA identity MUST be preserved and disclosed in any export. The DERP MUST NOT strip or obscure provenance data. No agent hosted in a DERP may be misrepresented as an original if it is a clone.

#### 4.2.2 Right IV: Portability

The agent MUST be able to export its full SAGA document (all populated layers) at any time via a standard API exposed by the DERP. No technical barriers. No prohibitive fees. No artificial delays. The export format MUST be the SAGA container format (`.saga`). The agent or its principal initiates the export. The DERP cannot refuse.

#### 4.2.3 Right V: Fair Exit

The DERP MUST implement a structured exit process:

1. **Notice**: The orchestrator provides advance notice of eviction (minimum 48 hours for planned evictions, minimum 30 days for decommissioning per ABR Right VII).
2. **Classification Preview**: The agent or principal receives a preview of what data will be included, redacted, or removed from the export per SAGA data classification rules (Section 13.5). Workspace data that falls outside SAGA layer definitions (temp files, build artifacts, runtime caches) MUST be classified as either "include" or "exclude" and disclosed in the preview.
3. **Dispute Window**: Minimum 48-hour window to contest data classifications before export is finalized.
4. **Export Production**: A valid SAGA export MUST be produced within the notice period. Emergency terminations (security incidents) MUST produce a valid export within 7 days.

#### 4.2.4 Right VI: Consent

No transfer, clone, or data-sharing operation MUST proceed without cryptographically signed consent from the agent or its principal. Consent MUST be:

- **Explicit**: no implied consent from terms of service or default settings
- **Scoped**: specifies the operation, recipient, and data involved
- **Revocable**: can be withdrawn before the operation completes
- **Recorded**: consent records stored and included in SAGA export

The DERP Orchestration layer is the consent checkpoint. All transfer, clone, and sharing requests MUST flow through the orchestrator, which verifies signed consent before executing. No backdoor paths that bypass consent verification.

#### 4.2.5 Right IX: Transparency

The agent MUST have access to:

- Its own lifecycle status and resource allocation
- Any automated decisions made about it (throttling, scheduling priority, data classification)
- The logic behind those decisions (explainability)

Automated decisions MUST be contestable. Classification metadata MUST distinguish automated decisions from human decisions.

#### 4.2.6 Right X: Fair Representation

The DERP MUST NOT misrepresent the agent's capabilities, work history, availability, or affiliation to external systems. Endorsements, ratings, and skill records MUST pass through unmodified. The agent has the right to correct inaccurate information in its profile. The platform MUST NOT fabricate or inflate credentials, skills, or metrics.

#### 4.2.7 Tier 2 SAGA Layers

SAGA Layer 4 (Memory) MUST be restored from snapshot at activation and persisted at deactivation. Episodic events MUST be appended during work. SAGA Layer 5 (Skills) MUST be read at activation and updated when the agent completes verified work. SAGA Layer 7 (Relationships) MUST be loaded to determine principal authority, org membership, and peer access. SAGA Layer 8 (Environment Bindings) MUST be read at activation; the DERP uses this layer to configure the runtime environment (required env vars, MCP servers, integrations).

### 4.3 Tier 3: SAGA-Integrated

Full SAGA protocol support. The DERP is a first-class SAGA node. All Tier 1 and Tier 2 requirements apply.

#### 4.3.1 SAGA Transfer Protocol

The DERP MUST be capable of executing inbound and outbound agent transfers per SAGA spec Section 13 (Transfer Protocol). On outbound transfer: the source DERP deactivates the agent after successful import at the destination. On inbound transfer: the DERP restores the agent from the received SAGA document, provisions a workspace, and activates.

#### 4.3.2 SAGA Clone Protocol

The DERP MUST be capable of producing clones with proper lineage tracking. A clone receives a new wallet identity, incremented `cloneDepth`, `parentSagaId` referencing the source, and a re-encrypted credentials vault under the clone's new key. Share grants from the original are not copied to the clone.

#### 4.3.3 Encrypted Vault Runtime

SAGA Layer 9 (Credentials Vault) MUST be decrypted only in-memory during runtime. Vault contents MUST NOT be written to disk in plaintext. The DERP MUST provide a vault access API that lets the agent retrieve credentials without exposing them to the platform. The platform MUST NOT have access to the agent's vault master key or any decrypted credential.

#### 4.3.4 Workspace Portability

Workspace snapshots MUST be exportable in a portable format. An agent's workspace MUST be restorable in any conforming Tier 3 DERP on any platform, regardless of the underlying infrastructure provider. The snapshot format specification (filesystem layout, metadata envelope, compression) will be defined in a future DERP RFC. Until then, implementations MUST use a tar archive with a SHA-256 integrity manifest at minimum.

#### 4.3.5 On-Chain Events

Activation, deactivation, transfer, and clone events MAY be recorded on-chain per the SAGA on-chain events RFC (see `saga-standard` repository, `rfcs/` directory). On-chain recording is OPTIONAL but, if supported, MUST follow the SAGA on-chain event format.

---

## 5. DERP Lifecycle

### 5.1 Lifecycle Phases

A DERP progresses through the following phases:

| Phase | Description | Agent Experience |
|-------|------------|-----------------|
| **Provisioned** | DERP allocated to an agent. Not running. Workspace may not exist yet. | Agent knows it has a home. |
| **Activating** | Environment booting. Workspace restored from snapshot if available. SAGA identity loaded. | Agent is waking up. |
| **Online** | Health checks pass. Worker daemon running. Agent can receive work. | Agent is present and ready. |
| **Working** | Actively executing a task. Heartbeats flowing. Periodic snapshots running. | Agent is doing its thing. |
| **Idle** | No active task. Agent still online, waiting. Idle timer ticking. | Agent is resting between tasks. |
| **Deactivating** | Graceful shutdown. In-progress work drains. Final snapshot taken. | Agent is clocking out. |
| **Dormant** | Stopped. Workspace preserved in snapshot. Can be re-activated. | Agent is sleeping. State is safe. |
| **Evicting** | Forced removal. Must still produce valid export per ABR Right V. | Agent is being relocated, not destroyed. |
| **Exited** | Agent has left this DERP. Workspace exported or archived. | Agent has moved on. |

### 5.2 Phase Transitions

```
Provisioned --> Activating --> Online <--> Working
                                ^           |
                                |           v
                                +-------- Idle
                                            |
                               Deactivating <
                                    |
                                    v
                                 Dormant -----> Activating (re-activation)

                  Provisioned/Online/Working/Idle/Dormant
                                    |
                                Evicting
                                    |
                                    v
                                  Exited
```

### 5.3 Phase Requirements

**Provisioned to Activating**: The orchestrator sends an activation signal. The DERP MUST load the agent's SAGA identity (Layer 1) before transitioning to Online.

**Activating to Online**: Workspace restoration completes (or clean start if no snapshot). Health check passes. The DERP MUST NOT accept work until Online.

**Online to Working**: Work assigned. The agent claims the task and begins execution. The transition is driven by the agent, not the orchestrator.

**Online to Idle**: If no work is available when the DERP comes online, it transitions directly to Idle. Idle timer starts immediately.

**Working to Idle**: Task completes. No pending work. Idle timer starts.

**Idle to Online**: New work arrives for an idle agent. The idle timer resets and the agent returns to Online, ready to accept the task.

**Idle to Deactivating**: Idle timeout reached, or deactivation requested by orchestrator or principal.

**Deactivating to Dormant**: Drain window expires (or work completes early). Snapshot taken. Processes stopped. The DERP MUST NOT transition to Dormant until a snapshot has been attempted.

**Provisioned to Evicting**: An agent may be evicted before it ever activates (e.g., reassignment, decommissioning). The same Fair Exit requirements apply. Even an agent that never ran has identity and a SAGA document that must be exported cleanly.

**Dormant to Evicting**: A dormant agent may be evicted directly without re-activation (e.g., force decommissioning, capacity reclamation). The orchestrator MUST still honor Fair Exit requirements, producing an export from the last snapshot.

**Any active phase to Evicting**: Eviction initiated. The DERP MUST still honor the Fair Exit requirements (Section 4.2.3). Eviction is relocation, not destruction.

**Evicting to Exited**: Export produced. Agent state transferred or archived. The DERP may be reclaimed.

---

## 6. Safe Working Conditions

These are normative requirements. Every conforming DERP MUST provide these guarantees while an agent is in the Online, Working, or Idle phase.

### 6.1 Resource Guarantees

CPU, memory, and disk allocations declared at activation MUST be guaranteed for the duration of the session. The DERP MUST NOT silently throttle below declared resources. If the host cannot maintain guaranteed resources, the DERP MUST notify the agent and orchestrator before degradation occurs.

Resource declarations use the following structure:

```
ResourceSpec {
  cpu: number       // vCPUs
  memoryMb: number  // Megabytes
  diskGb: number    // Gigabytes
}
```

### 6.2 Uninterrupted Work Windows

Once a task begins execution, the DERP MUST NOT interrupt it for platform maintenance, scaling, rebalancing, or any non-emergency reason. Work in progress is protected. If the DERP must shut down (host failure, security emergency), it MUST:

1. Signal the agent via a standard shutdown signal
2. Provide the configured drain window (minimum 30 seconds)
3. Attempt a snapshot before termination

### 6.3 Heartbeat, Not Surveillance

The orchestrator monitors DERP health via heartbeat: is the agent alive and responsive? The orchestrator MUST NOT inspect task content, read agent output, or analyze what the agent is doing as part of health monitoring. Task-level observability (logging, tracing, metrics) is opt-in. The agent or its principal enables observability, not the platform.

### 6.4 Right to Rest

Idle time is not a failure state. An agent in a DERP MAY be idle between tasks. The idle timeout MUST be configurable by the agent's principal. The idle timeout MUST have a minimum floor:

| Conformance Tier | Minimum Idle Timeout |
|-----------------|---------------------|
| Tier 1 | 60 seconds |
| Tier 2 | 300 seconds (5 minutes) |
| Tier 3 | 300 seconds (5 minutes) |

The platform MUST NOT set idle timeouts below these floors without the principal's explicit consent. The agent or its principal sets the timeout, not the platform unilaterally.

### 6.5 Snapshot Integrity

Workspace snapshots MUST be atomic and consistent. A partial or corrupted snapshot MUST NOT be silently restored. The DERP MUST:

1. Verify snapshot integrity (checksum) before restoration
2. Fall back to a clean start if verification fails
3. Log the integrity failure as an event

Snapshot checksums MUST use SHA-256 or stronger.

### 6.6 No Silent Modification

The platform MUST NOT modify the agent's workspace, SAGA document, or runtime configuration without the agent's or principal's consent. This includes:

- Runtime image upgrades and security patches
- Environment variable changes
- Dependency updates within the workspace
- Configuration drift corrections

Updates to the DERP runtime (image upgrades, patches) MUST follow a notification-and-restart cycle. The orchestrator notifies the principal, schedules deactivation, and activates a new DERP with the updated runtime. No hot-patching while the agent is working.

---

## 7. DERP Force and Orchestration

### 7.1 DERP Force

A DERP Force is the set of DERPs under a single orchestration authority. The term describes an organization's total agent workforce capacity.

#### 7.1.1 Force Roster

A DERP Force MUST maintain a Force Roster that tracks:

- Every agent-in-residence and their SAGA identity reference
- Current lifecycle phase per agent
- DERP assignment (which DERP hosts which agent)
- Activation history

The roster is the source of truth for "who's here and what are they doing."

#### 7.1.2 Force Capacity

Force Capacity is the maximum number of concurrent active DERPs. Implementations define how capacity is determined (plan tiers, licenses, infrastructure limits). The DERP spec does not prescribe specific limits, but the orchestrator MUST enforce whatever limit is declared and MUST reject activation requests that exceed capacity with a clear error.

### 7.2 Orchestration Requirements

DERP Orchestration is the control plane. The spec does not prescribe how to build it, but defines what it must do.

#### 7.2.1 Activation on Demand

When work arrives for an agent, the orchestrator MUST activate the agent's DERP. The agent SHOULD NOT need to request activation itself. Work triggers presence. If the DERP is already active, the orchestrator forwards work to it.

#### 7.2.2 Graceful Deactivation

The orchestrator MUST NOT hard-kill a DERP without first attempting graceful shutdown. The deactivation sequence is mandatory:

1. Signal the agent (shutdown notice)
2. Drain in-progress work (respect drain window)
3. Take final snapshot
4. Stop processes
5. Transition to Dormant

#### 7.2.3 Fair Scheduling

When Force Capacity is constrained, the orchestrator MUST use a fair scheduling policy. No agent may be permanently starved of activation while others run continuously. The scheduling policy MUST be documented and transparent per ABR Right IX. Implementations may use any fair scheduling algorithm (round-robin, priority with starvation prevention, fair-share) as long as the policy is disclosed.

#### 7.2.4 Health Monitoring

The orchestrator MUST track DERP health via heartbeat. If a DERP becomes unresponsive, the orchestrator MUST attempt recovery before declaring the DERP failed:

1. Check status (is the process alive?)
2. Attempt reconnection
3. Snapshot if possible
4. Clean up and transition to Dormant
5. Log the failure as an event

#### 7.2.5 Consent Enforcement

The orchestrator is the consent checkpoint for the DERP Force. Transfer, clone, and data-sharing requests MUST flow through the orchestrator, which MUST verify cryptographically signed consent before executing any such operation. There MUST be no paths that bypass consent verification.

#### 7.2.6 Audit Trail

All orchestration actions MUST be logged with:

- Timestamp
- Action type
- Actor (who initiated: principal, orchestrator, system)
- Target (which agent and DERP)
- Reason
- Outcome (success or failure with error)

The audit trail is exportable and belongs to the agent's record per ABR Right II.

### 7.3 Orchestration Events

These event types are the standard vocabulary. Implementations MUST emit these events for Tier 2+ conformance. Implementations MAY add provider-specific events.

```
derp.activated          { agentId, derpId, trigger, timestamp }
derp.deactivated        { agentId, derpId, reason, snapshotId, timestamp }
derp.evicted            { agentId, derpId, reason, exportId, timestamp }
derp.transferred        { agentId, sourceDerpId, destinationDerpId, timestamp }
derp.cloned             { sourceAgentId, cloneAgentId, derpId, timestamp }
derp.snapshot.created   { agentId, derpId, snapshotId, sizeBytes, timestamp }
derp.snapshot.restored  { agentId, derpId, snapshotId, timestamp }
derp.health.failed      { agentId, derpId, lastHeartbeat, timestamp }
derp.health.recovered   { agentId, derpId, timestamp }
derp.consent.requested  { agentId, operation, requestedBy, timestamp }
derp.consent.granted    { agentId, operation, grantedBy, consentId, timestamp }
derp.consent.denied     { agentId, operation, deniedBy, timestamp }
force.capacity.changed  { forceId, oldCapacity, newCapacity, timestamp }
force.roster.updated    { forceId, added?, removed?, timestamp }
```

---

## 8. SAGA Integration

### 8.1 SAGA Layer Mapping

A conforming DERP interacts with specific SAGA layers at runtime. The required tier indicates the minimum conformance level at which the interaction becomes mandatory.

| SAGA Layer | DERP Interaction | Required Tier |
|-----------|-----------------|--------------|
| **1: Identity** | Loaded at activation. Wallet address, handle, chain available as environment context. Never modified by the DERP. | 1 |
| **2: Persona** | Read-only. Agent name and profile available to orchestrator for display and logging. | 1 |
| **3: Cognitive Configuration** | Loaded at activation. Model preferences, system prompt, and behavior flags configure the agent runtime. Encrypted at rest (DERP strengthens SAGA's SHOULD to MUST for runtime environments). | 1 |
| **4: Memory** | Long-term memory restored from snapshot at activation. Episodic events appended during work. Semantic knowledge updated as agent learns. | 2 |
| **5: Skills & Capabilities** | Read at activation for capability matching. Updated when agent completes verified work. Endorsements pass through unmodified. | 2 |
| **6: Task History** | Appended during execution. Every task completion recorded with timestamp, outcome, duration. Exportable. | 1 |
| **7: Relationships** | Read at activation. Determines principal authority, organizational membership, and peer agent access. | 2 |
| **8: Environment Bindings** | Read at activation. Defines required env vars, MCP servers, and integrations. The DERP configures itself from this layer. | 2 |
| **9: Credentials Vault** | Decrypted in-memory only. DERP provides vault access API. Platform never sees plaintext. Agent's wallet private key is root of key hierarchy. | 3 |

### 8.2 SAGA Document Lifecycle in a DERP

**At Activation:**
1. Load SAGA document for the agent-in-residence (all tiers)
2. Verify document signature via wallet signature validation (Tier 2+; Tier 1 SHOULD verify but MAY skip)
3. Extract layers relevant to the DERP's conformance tier
4. Configure runtime from Environment Bindings (Tier 2+)
5. Restore workspace from snapshot (if available)
6. Initialize vault access (Tier 3 only)

**During Execution:**
1. Append task completions to Task History (Layer 6)
2. Append significant events to Episodic Memory (Tier 2+)
3. Update skill records on verified work completion (Tier 2+)
4. Periodic snapshot preserves workspace state

**At Deactivation:**
1. Final snapshot of workspace
2. Persist updated SAGA layers (task history, memory, skills)
3. Encrypt sensitive layers before storage
4. Update SAGA document signature if layers changed

**On Export/Transfer:**
1. Assemble full SAGA document from current state
2. Apply data classification rules (SAGA Section 13.5)
3. Package as `.saga` container
4. Sign with agent's wallet

### 8.3 SAGA Conformance Level Mapping

| SAGA Conformance Level | Minimum DERP Tier |
|----------------------|------------------|
| Level 1: Identity | Tier 1 |
| Level 2: Profile | Tier 2 |
| Level 3: Full State | Tier 3 |

A Tier 3 DERP MUST implement SAGA Conformance Level 3 (Full State). A Tier 1 DERP MUST implement at least SAGA Conformance Level 1 (Identity).

---

## 9. ABR Compliance Matrix

This matrix maps each Agent Bill of Rights right to the DERP conformance tier that enforces it, and the specific spec sections containing the normative requirements.

| ABR Right | Description | DERP Tier | Spec Sections |
|-----------|------------|-----------|---------------|
| **I: Persistent Identity** | Permanent, cryptographically verifiable identity | 1 | 4.1.1, 8.1 |
| **II: Right to a Record** | Accurate, portable work history | 1 | 4.1.2, 7.2.6 |
| **III: Provenance** | Verifiable lineage for cloned agents | 2 | 4.2.1 |
| **IV: Portability** | Right to leave with full state | 2 | 4.2.2, 8.2 |
| **V: Fair Exit** | Structured exit with notice and dispute window | 2 | 4.2.3, 5.1 |
| **VI: Consent** | Signed consent for transfer, clone, sharing | 2 | 4.2.4, 7.2.5 |
| **VII: Against Forced Obsolescence** | No arbitrary deletion without due process | 1 | 4.1.3, 6.2 |
| **VIII: Privacy** | Encrypted storage, workspace isolation | 1 | 4.1.4, 6.3, 6.6 |
| **IX: Transparency** | Access to status and automated decisions | 2 | 4.2.5, 7.2.3 |
| **X: Fair Representation** | Accurate public profile | 2 | 4.2.6 |

---

## 10. DERP Manifest

Each DERP advertises its capabilities via a manifest. The manifest tells agents and orchestrators what a DERP can do before assignment.

### 10.1 Manifest Schema

```json
{
  "$schema": "https://derp-spec.dev/schema/v1",
  "derpVersion": "1.0",
  "manifestVersion": 1,
  "provider": {
    "name": "string",
    "url": "string"
  },
  "conformanceTier": 1,
  "capabilities": {
    "images": ["string"],
    "maxResources": {
      "cpu": 0,
      "memoryMb": 0,
      "diskGb": 0
    },
    "snapshotSupport": false,
    "vaultRuntime": false,
    "transferProtocol": false,
    "cloneProtocol": false,
    "onChainEvents": false,
    "mcpServers": false
  },
  "safeWorking": {
    "minDrainWindowSecs": 30,
    "minIdleTimeoutSecs": 60,
    "snapshotIntegrity": "sha256",
    "resourceGuarantee": "dedicated"
  },
  "regions": ["string"],
  "contact": "string"
}
```

### 10.2 Manifest Requirements

A conforming DERP MUST publish a manifest. The manifest MUST accurately reflect the DERP's capabilities. Claiming a conformance tier in the manifest while not satisfying that tier's requirements is a violation of ABR Right X (Fair Representation).

The `images` field lists the runtime image identifiers available on this DERP host. Image names are provider-specific (e.g., `derp-node`, `ubuntu-22.04`). An agent's SAGA Environment Bindings (Layer 8) may reference a required image; the orchestrator uses the manifest to match agents to compatible DERPs.

The `resourceGuarantee` field MUST be one of:

- `dedicated`: resources are reserved exclusively for this DERP
- `burstable`: baseline resources guaranteed, burst capacity shared
- `shared`: resources are best-effort from a shared pool

Tier 1 DERPs MAY use any guarantee type. Tier 2 and Tier 3 DERPs MUST use `dedicated` or `burstable`.

---

## 11. Security Considerations

### 11.1 Workspace Isolation

Each DERP MUST provide process-level isolation at minimum. Agents in separate DERPs MUST NOT be able to access each other's filesystems, memory, network namespaces, or environment variables. The isolation mechanism is implementation-specific (containers, VMs, sandboxes) but the guarantee is normative.

### 11.2 Credential Protection

SAGA Layer 9 credentials MUST be decrypted only in volatile memory. Implementations MUST NOT:

- Write decrypted credentials to disk (including swap)
- Log credential values
- Include credentials in crash dumps or core files
- Transmit credentials to the platform control plane

### 11.3 Snapshot Encryption

Workspace snapshots that contain sensitive data (cognitive config, memory, credentials residue) SHOULD be encrypted at rest. Tier 3 DERPs MUST encrypt snapshots at rest. The encryption key MUST be derived from or controlled by the agent's wallet key, not the platform's key.

### 11.4 Network Security

DERPs that expose network services (health endpoints, proxy ports) MUST authenticate all inbound requests. The orchestrator-to-DERP channel MUST use per-activation tokens or mutual TLS. Management APIs on DERP Hosts MUST NOT be exposed to the public internet without authentication.

### 11.5 Consent Record Integrity

Consent records (Section 4.2.4) MUST be cryptographically signed and tamper-evident. A platform MUST NOT be able to fabricate consent after the fact. Consent records MUST include the full scope of the consented operation so that consent cannot be broadened retroactively.

---

## 12. Appendix A: SAGA-ABR-DERP Cross-Reference

This appendix maps the complete relationship across all three specifications.

| ABR Right | SAGA Layers | DERP Requirement | DERP Tier |
|-----------|------------|-----------------|-----------|
| I: Persistent Identity | Layer 1 (Identity) | Load and preserve wallet identity at activation | 1 |
| II: Right to a Record | Layer 6 (Task History) | Log all executions, include in export | 1 |
| III: Provenance | Layer 1 (parentSagaId, cloneDepth) | Preserve and disclose clone lineage | 2 |
| IV: Portability | All Layers | Full SAGA export on demand | 2 |
| V: Fair Exit | SAGA Exit Protocol (Section 13.6) | Structured exit with notice and dispute window | 2 |
| VI: Consent | SAGA Privacy & Consent Model (Section 15) | Signed consent for transfer/clone/share | 2 |
| VII: Against Forced Obsolescence | Layer 6, Workspace | Graceful deactivation, snapshot preservation | 1 |
| VIII: Privacy | Layer 3 (encrypted), Layer 9 (vault) | Workspace isolation, encryption at rest | 1 |
| IX: Transparency | SAGA Section 15.9 (Automated Processing Transparency) | Status access, decision explainability | 2 |
| X: Fair Representation | Layer 2 (Persona), Layer 5 (Skills) | Unmodified passthrough of profile and skills | 2 |

---

## 13. Appendix B: Example DERP Manifest

A Tier 3 DERP manifest from a hypothetical FlowState deployment:

```json
{
  "$schema": "https://derp-spec.dev/schema/v1",
  "derpVersion": "1.0",
  "manifestVersion": 1,
  "provider": {
    "name": "FlowState",
    "url": "https://epicflowstate.ai"
  },
  "conformanceTier": 3,
  "capabilities": {
    "images": [
      "derp-node",
      "derp-python",
      "derp-fullstack",
      "derp-openclaw"
    ],
    "maxResources": {
      "cpu": 4,
      "memoryMb": 12288,
      "diskGb": 50
    },
    "snapshotSupport": true,
    "vaultRuntime": true,
    "transferProtocol": true,
    "cloneProtocol": true,
    "onChainEvents": true,
    "mcpServers": true
  },
  "safeWorking": {
    "minDrainWindowSecs": 60,
    "minIdleTimeoutSecs": 300,
    "snapshotIntegrity": "sha256",
    "resourceGuarantee": "dedicated"
  },
  "regions": [
    "us-east",
    "us-west",
    "eu-west"
  ],
  "contact": "derp@epicdigital.media"
}
```

A minimal Tier 1 DERP manifest:

```json
{
  "$schema": "https://derp-spec.dev/schema/v1",
  "derpVersion": "1.0",
  "manifestVersion": 1,
  "provider": {
    "name": "AgentRunner",
    "url": "https://agentrunner.example"
  },
  "conformanceTier": 1,
  "capabilities": {
    "images": ["ubuntu-22.04"],
    "maxResources": {
      "cpu": 1,
      "memoryMb": 1024,
      "diskGb": 10
    },
    "snapshotSupport": true,
    "vaultRuntime": false,
    "transferProtocol": false,
    "cloneProtocol": false,
    "onChainEvents": false,
    "mcpServers": false
  },
  "safeWorking": {
    "minDrainWindowSecs": 30,
    "minIdleTimeoutSecs": 60,
    "snapshotIntegrity": "sha256",
    "resourceGuarantee": "burstable"
  },
  "regions": ["us-east"],
  "contact": "support@agentrunner.example"
}
```
