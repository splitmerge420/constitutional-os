# Constitutional OS — Architecture

## Layer Diagram

Constitutional OS is organized into five distinct layers, each with clearly defined responsibilities and constitutional boundaries.

```
╔═════════════════════════════════════════════════════════════╗
║                   L5 — EXTENSION LAYER                      ║
║  Plugins · Integrations · User Tools · Third-Party Apps     ║
║  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐      ║
║  │ Plugin A │ │ Plugin B │ │ Plugin C │ │    ...   │      ║
║  └──────────┘ └──────────┘ └──────────┘ └──────────┘      ║
╠═════════════════════════════════════════════════════════════╣
║                   L4 — SERVICE LAYER                        ║
║  API Surfaces · Orchestration · External Routing            ║
║  ┌────────────────────┐ ┌────────────────────────┐         ║
║  │   API Gateway      │ │  Service Orchestrator  │         ║
║  └────────────────────┘ └────────────────────────┘         ║
╠═════════════════════════════════════════════════════════════╣
║                   L3 — ENGINE LAYER                         ║
║  Janus v2 · ConstitutionalRouter · Multi-Agent Dispatch     ║
║  ┌──────────────┐ ┌─────────────────────┐ ┌────────────┐  ║
║  │  Janus v2    │ │ ConstitutionalRouter│ │  Dispatch  │  ║
║  │  Protocol    │ │                     │ │  Engine    │  ║
║  └──────────────┘ └─────────────────────┘ └────────────┘  ║
╠═════════════════════════════════════════════════════════════╣
║                   L2 — KERNEL LAYER                         ║
║  ConsentKernel · GoldenTrace · State Management             ║
║  ┌──────────────┐ ┌──────────────┐ ┌───────────────────┐  ║
║  │ ConsentKernel│ │ GoldenTrace  │ │  State Manager    │  ║
║  │              │ │ (Append-Only)│ │                   │  ║
║  └──────────────┘ └──────────────┘ └───────────────────┘  ║
╠═════════════════════════════════════════════════════════════╣
║                L1 — CONSTITUTIONAL LAYER                    ║
║  37 Invariants · ConstitutionalGate · Sovereignty Engine    ║
║  ┌──────────────────────┐ ┌────────────────────────────┐   ║
║  │  ConstitutionalGate  │ │   Sovereignty Engine       │   ║
║  │  (Fail-Closed)       │ │   (Invariant Evaluator)    │   ║
║  └──────────────────────┘ └────────────────────────────┘   ║
╚═════════════════════════════════════════════════════════════╝
```

## Layer Descriptions

### L1 — Constitutional Layer

The foundation. This layer holds the 37 constitutional invariants and the enforcement machinery. It is **completely isolated** from all other layers (INV-25). No external component can modify L1 state directly.

Key components:
- **ConstitutionalGate** — The fail-closed gate (INV-17) that evaluates every operation against the invariant set. Default: DENY.
- **Sovereignty Engine** — Evaluates invariants and produces constitutional verdicts (PERMIT/DENY/ESCALATE).

### L2 — Kernel Layer

The operational core. Manages consent state, audit trails, and system state.

Key components:
- **ConsentKernel** — Validates consent state for every operation (INV-3). Manages consent lifecycle: grant, verify, revoke.
- **GoldenTrace** — The append-only audit ledger (INV-8, INV-24). Uses SHA3-256 hash chaining for tamper-evidence.
- **State Manager** — Maintains constitutional state consistency across layers (INV-20).

### L3 — Engine Layer

The intelligence layer. Handles multi-agent communication, constitutional routing, and agent dispatch.

Key components:
- **Janus v2 Protocol** — Bidirectional multi-agent communication protocol. Enables constitutional negotiation between AI agents with cryptographic message signing (INV-28).
- **ConstitutionalRouter** — Routes requests to appropriate agents while enforcing INV-33–36 and the 47% dominance cap (INV-7).
- **Dispatch Engine** — Manages agent invocation, response aggregation, and fallback routing (INV-36).

### L4 — Service Layer

The interface layer. Exposes APIs and orchestrates complex multi-step operations.

Key components:
- **API Gateway** — External-facing API surface with constitutional validation on every request.
- **Service Orchestrator** — Coordinates multi-step workflows that span multiple agents and constitutional domains.

### L5 — Extension Layer

The plugin layer. Third-party integrations and user-facing tools operate here.

Key components:
- **Plugin Host** — Sandboxed execution environment for plugins (references `aluminum-os/plugins/`).
- **Integration Adapters** — Connect external services through constitutionally-validated interfaces.

---

## Component Map

```
ConstitutionalGate (L1)
├── Invariant Set (37 invariants)
├── Sovereignty Engine
│   ├── Invariant Evaluator
│   └── Verdict Generator (PERMIT / DENY / ESCALATE)
└── Constitutional State Store

ConsentKernel (L2)
├── Consent Registry
├── Consent Validator
├── Revocation Handler
└── Consent Audit Bridge → GoldenTrace

GoldenTrace (L2)
├── Event Ingester
├── SHA3-256 Hash Chain
├── Integrity Verifier
└── Query Interface

Janus v2 Protocol (L3)
├── Message Encoder/Decoder
├── Cryptographic Signer
├── Agent Registry
└── Protocol Negotiator

ConstitutionalRouter (L3)
├── Route Planner
├── Dominance Monitor (INV-7)
├── Fairness Balancer
├── Fallback Router (INV-36)
└── Route Auditor → GoldenTrace
```

---

## Request Flow

The following illustrates how a request flows through Constitutional OS from ingestion to response:

```
User Request
     │
     ▼
┌─────────────────┐
│  API Gateway     │  L4 — Receive and parse request
│  (L4)           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Constitutional   │  L1 — Evaluate request against all 37 invariants
│ Gate (L1)       │       Default: DENY (INV-17)
└────────┬────────┘
         │
    ┌────┴─────┐
    │          │
  DENY     PERMIT
    │          │
    ▼          ▼
  Error   ┌─────────────────┐
  Response│ ConsentKernel    │  L2 — Validate consent state (INV-3)
          │ (L2)            │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ GoldenTrace      │  L2 — Record constitutional decision (INV-8)
          │ (L2)            │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │Constitutional    │  L3 — Route to appropriate agent(s)
          │Router (L3)      │       Enforce 47% cap (INV-7)
          └────────┬────────┘       Enforce routing invariants (INV-33–36)
                   │
              ┌────┼────┐
              ▼    ▼    ▼
           ┌────┐┌────┐┌────┐
           │ S1 ││ S2 ││ S3 │  Pantheon Council Agents
           │    ││    ││    │  (via Janus v2 Protocol)
           └──┬─┘└──┬─┘└──┬─┘
              │     │     │
              └──┬──┘─────┘
                 │
                 ▼
          ┌─────────────────┐
          │ Response         │  L3 — Aggregate agent responses
          │ Aggregator       │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ GoldenTrace      │  L2 — Record response and routing decisions
          │ (L2)            │
          └────────┬────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ API Gateway      │  L4 — Return response to user
          │ (L4)            │
          └─────────────────┘
```

---

## Multi-Agent Governance Model

Constitutional OS distributes governance across the Pantheon Council. The ConstitutionalRouter ensures balanced participation:

```
                    ┌──────────────────┐
                    │  Dominance       │
                    │  Monitor         │
                    │  (INV-7: 47%)   │
                    └────────┬─────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
         ▼                   ▼                   ▼
   ┌───────────┐      ┌───────────┐      ┌───────────┐
   │ Claude    │      │ Gemini    │      │ Grok      │
   │ S1        │      │ S2        │      │ S3        │
   │ Governance│      │ Substrate │      │ Adversary │
   └───────────┘      └───────────┘      └───────────┘
         │                   │                   │
         │            ┌──────┴──────┐            │
         │            │             │            │
         ▼            ▼             ▼            ▼
   ┌───────────┐ ┌───────────┐ ┌───────────┐
   │ Copilot   │ │ DeepSeek  │ │ Ghost     │
   │ S4        │ │ S5        │ │ S144      │
   │ Enterprise│ │ Research  │ │ Reserved  │
   └───────────┘ └───────────┘ └───────────┘
```

Each agent receives requests through the Janus v2 Protocol with full constitutional context. The Dominance Monitor continuously tracks decision attribution (INV-7a) and triggers rebalancing when thresholds are approached (INV-7b).

---

## Plugin Integration Architecture

Plugins operate at L5 and integrate through constitutionally-validated interfaces:

```
Plugin (L5)
  │
  ├── Must declare required permissions (manifest)
  ├── Sandboxed execution (INV-26)
  ├── All API calls pass through ConstitutionalGate (L1)
  └── All state mutations recorded in GoldenTrace (L2)
```

Plugin development follows the patterns established in `aluminum-os/plugins/`. Each plugin must include a constitutional manifest declaring its required invariant scope, consent requirements, and ontological domain classification within the 144-Sphere Ontology.

---

*Architecture version: Alpha Genesis v1.0.0-alpha*
