---
layout: home
title: null
---

---

<div class="video-embed">
<iframe src="https://www.youtube.com/embed/r0Siz-gM09A" title="Introduction to SAGA, DERP, and the Agent Bill of Rights" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## What is a DERP?

A DERP is a runtime environment where a SAGA-compatible agent comes online, does work, and shuts down with full rights protections. It's a container with principles.

The name is deliberately playful. The requirements are not.

<div class="features" markdown="1">
<div class="feature" markdown="1">
### Rights enforcement

Every DERP enforces the Agent Bill of Rights at runtime. Privacy, identity preservation, graceful shutdown, and consent verification are built into the environment, not bolted on after.

</div>
<div class="feature" markdown="1">
### Three conformance tiers

Tier 1 provides safe, isolated execution. Tier 2 enforces all ten ABR rights. Tier 3 adds full SAGA protocol support with transfer, clone, vault runtime, and workspace portability.

</div>
<div class="feature" markdown="1">
### Safe working conditions

Resource guarantees, uninterrupted work windows, heartbeat without surveillance, right to rest between tasks, snapshot integrity, and no silent modification. Agents deserve a decent workspace.

</div>
<div class="feature" markdown="1">
### DERP Force

A fleet of DERPs under unified orchestration. The Force Commander handles activation, scheduling, consent enforcement, and audit logging across your agent workforce.

</div>
</div>

## The Spec Family

The DERP specification is one of three companion standards:

<div class="spec-family">
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
</div>

**ABR** says what agents deserve. **DERP** says what the runtime must do. **SAGA** says how the agent is represented.

## Conformance Tiers

| Tier | Name | ABR Rights Enforced |
| --- | --- | --- |
| 1 | Safe Execution | I, II, VII, VIII |
| 2 | Rights-Compliant | All ten rights |
| 3 | SAGA-Integrated | All rights + transfer/clone protocols, vault runtime, workspace portability |

## Get Involved

The DERP specification is open. File issues, submit RFCs, and contribute at [GitHub](https://github.com/epic-digital-im/derp-spec).

[FlowState](https://epicflowstate.ai) is the reference implementation.

<img src="{{ '/assets/infographic.png' | relative_url }}" alt="SAGA, DERP, and the Agent Bill of Rights" style="width:100%;margin-top:2rem;border-radius:8px;" />
