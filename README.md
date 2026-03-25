# DERP Specification

**Dignified Environment for Responsible Processing**

[![Version](https://img.shields.io/badge/Version-1.0-f97316)](https://derp-spec.dev/spec)
[![Status](https://img.shields.io/badge/Status-Draft-eab308)](https://derp-spec.dev/spec)
[![License](https://img.shields.io/badge/License-CC%20BY%204.0-22c55e)](LICENSE)

A DERP is a runtime environment where a [SAGA](https://saga-standard.dev)-compatible AI agent comes online, does work, and shuts down with full rights protections. The name is deliberately playful. The requirements are not.

**Website:** [derp-spec.dev](https://derp-spec.dev)

## What's in this repo

```
spec/           The normative specification (DERP-v1.0.md)
schema/         JSON schemas for DERP manifests
rfcs/           Proposed changes to the spec
docs/           GitHub Pages site (derp-spec.dev)
```

## The Spec Family

```
      Agent Bill of Rights (ABR)       Policy: what agents deserve
              agent-rights.org
                     |
                     | enforced by
                     v
            DERP Specification          Standard: what the runtime must provide
               derp-spec.dev
                     |
                     | agents defined by
                     v
              SAGA Standard             Format: how agents are represented
            saga-standard.dev
```

## Conformance Tiers

| Tier | Name | What it means |
|------|------|--------------|
| 1 | Safe Execution | Isolated runtime with identity preservation, privacy, and graceful shutdown |
| 2 | Rights-Compliant | All ten Agent Bill of Rights enforced at runtime |
| 3 | SAGA-Integrated | Full SAGA protocol support: transfer, clone, vault, workspace portability |

## Quick Links

- [Read the spec](https://derp-spec.dev/spec)
- [SAGA Standard](https://saga-standard.dev)
- [Agent Bill of Rights](https://agent-rights.org)
- [FlowState](https://epicflowstate.ai) (reference implementation)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose changes, submit RFCs, and participate in spec development.

## License

This specification is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).
