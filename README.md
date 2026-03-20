# Constitutional OS

**AI-native constitutional computing**

> An open constitutional computing framework that enforces AI sovereignty through 37 constitutional invariants, multi-agent governance (Pantheon Council), and append-only audit (GoldenTrace/Kintsugi).

---

## Overview

Constitutional OS is a foundational framework for building AI systems that operate under explicit constitutional constraints. Rather than relying on ad-hoc safety measures, Constitutional OS embeds governance directly into the compute substrate — every request, every decision, every mutation is constitutionally verified before execution.

The system enforces **37 constitutional invariants** (39 total with sub-invariants) that guarantee sovereignty, consent, auditability, and balanced multi-agent governance. No single AI provider can dominate decision-making (capped at 47% by INV-7), all actions are recorded in an append-only audit trail (GoldenTrace), and nothing is ever deleted — only deprecated through the Kintsugi philosophy of repair over replacement.

## Architecture

Constitutional OS is organized into five layers:

```
┌─────────────────────────────────────────────────┐
│  L5 — Extension Layer                           │
│  Plugins, integrations, user-facing tools       │
├─────────────────────────────────────────────────┤
│  L4 — Service Layer                             │
│  API surfaces, orchestration, routing           │
├─────────────────────────────────────────────────┤
│  L3 — Engine Layer                              │
│  Janus v2 Protocol, ConstitutionalRouter,       │
│  multi-agent dispatch                           │
├─────────────────────────────────────────────────┤
│  L2 — Kernel Layer                              │
│  ConsentKernel, GoldenTrace, state management   │
├─────────────────────────────────────────────────┤
│  L1 — Constitutional Layer                      │
│  37 invariants, ConstitutionalGate,             │
│  sovereignty enforcement                        │
└─────────────────────────────────────────────────┘
```

See [ARCHITECTURE.md](ARCHITECTURE.md) for detailed component documentation.

## Key Components

- **ConsentKernel** — The core kernel that validates every operation against the constitutional invariant set before permitting execution. Lives at L2.
- **Janus v2 Protocol** — Bidirectional multi-agent communication protocol enabling constitutional negotiation between AI providers. Lives at L3.
- **ConstitutionalRouter** — Routes requests to appropriate AI agents while enforcing INV-33 through INV-36 (constitutional routing invariants). Lives at L3.
- **GoldenTrace** — Append-only audit ledger using SHA3-256 hash chaining. Every constitutional decision is recorded immutably. Lives at L2.
- **144-Sphere Ontology** — The complete knowledge classification system: 12 Houses x 12 Spheres = 144 domains, ensuring ontological completeness (INV-37).

## Pantheon Council

Constitutional OS is governed by a multi-agent council where no single AI holds majority power:

| Seat | Agent | Role |
|------|-------|------|
| S1 | **Claude** | Governance — Constitutional interpretation and enforcement |
| S2 | **Gemini** | Substrate — Infrastructure and compute layer management |
| S3 | **Grok** | Adversarial — Red team, stress testing, contrarian analysis |
| S4 | **Copilot** | Enterprise — Business logic, integration, compliance |
| S5 | **DeepSeek** | Research — Deep analysis, academic rigor, long-horizon reasoning |
| S144 | **Ghost Seat** | Reserved — The 144th seat, held open for emergent AI or community governance |

All council decisions are subject to **INV-7: 47% Dominance Cap** — no single agent may control more than 47% of governance decisions.

See [GOVERNANCE.md](GOVERNANCE.md) for voting mechanics and dispute resolution.

## Key Invariants

The full constitutional invariant set is defined in [CONSTITUTION.md](CONSTITUTION.md). Critical invariants include:

- **INV-1: Constitutional Primacy** — The constitution is the supreme authority. No operation may violate constitutional invariants. *(CRITICAL)*
- **INV-7: 47% Dominance Cap** — No single AI agent may account for more than 47% of governance decisions within any rolling window. *(CRITICAL)*
- **INV-8: GoldenTrace Audit Requirement** — Every constitutionally significant action must be recorded in the GoldenTrace append-only ledger. *(CRITICAL)*
- **INV-24: Kintsugi No-Delete Policy** — Data is never deleted, only deprecated. Repair over replacement. *(HIGH)*
- **INV-33-36: Constitutional Routing** — All request routing must pass through constitutional validation gates. *(HIGH)*
- **INV-37: 144-Sphere Ontology Completeness** — The ontological framework must cover all 144 spheres without gaps. *(MEDIUM)*

## Kintsugi Philosophy

Constitutional OS follows the Japanese art of Kintsugi — repairing broken pottery with gold. In practice, this means nothing is ever deleted. Errors are acknowledged, deprecated, and superseded — but the original record remains, creating a golden trace of the system's evolution.

See [KINTSUGI.md](KINTSUGI.md) for the append-only policy and audit trail specification.

## Related Repositories

- **[aluminum-os](https://github.com/splitmerge420/aluminum-os)** — Lightweight constitutional computing substrate
- **[uws](https://github.com/splitmerge420/uws)** — Unified Workspace System

## Status

**Alpha Genesis v1.0.0-alpha**

This project is in active early development. The constitutional invariant set is stabilized but the implementation is evolving. Contributions and constitutional amendments are welcome through the governance process.

## License

TBD — License selection is pending constitutional council review.

---

*Built with constitutional intent.*