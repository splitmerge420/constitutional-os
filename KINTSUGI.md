# Constitutional OS — Kintsugi Policy

## Philosophy: Repair Over Replacement

Kintsugi (金継ぎ) is the Japanese art of repairing broken pottery with gold, treating the breakage and repair as part of the object's history rather than something to disguise. In Constitutional OS, this philosophy is foundational: **nothing is ever deleted.**

Every error, every correction, every evolution of the system is preserved. When something breaks or becomes outdated, we don't erase it — we repair it with gold. The "gold" in our case is the GoldenTrace audit trail: an append-only ledger that captures the complete history of the system, including its mistakes.

This is not merely a technical policy. It is a constitutional commitment (INV-24) that reflects a core belief: **the history of a system's evolution — including its failures — is more valuable than the illusion of perfection.**

---

## Constitutional Basis

The Kintsugi policy is enforced by:

- **INV-24: Kintsugi No-Delete Policy** — Data is never deleted, only deprecated. Repair over replacement.
- **INV-8: GoldenTrace Audit Requirement** — Every constitutionally significant action is recorded in the append-only ledger.
- **INV-4: Consent Revocability** — Consent revocation prevents future operations but does not delete historical records.
- **INV-15: Council Transparency** — All governance history is accessible and preserved.

---

## GoldenTrace Event Format

Every event in the GoldenTrace ledger follows this canonical format:

```
┌─────────────────────────────────────────────────────┐
│ GoldenTrace Event                                    │
├─────────────────────────────────────────────────────┤
│ event_id:        UUID v4                             │
│ timestamp:       ISO 8601 (UTC, nanosecond precision)│
│ sequence:        Monotonically increasing integer    │
│ event_type:      DECISION | MUTATION | CONSENT |     │
│                  GOVERNANCE | DEPRECATION | ERROR |   │
│                  AMENDMENT | AUDIT                    │
│ actor:           Agent identifier (S1–S5, S144,      │
│                  SYSTEM, USER)                        │
│ invariants:      List of invariant IDs evaluated     │
│ verdict:         PERMIT | DENY | ESCALATE            │
│ context:         Structured event payload (JSON)     │
│ references:      List of related event_ids           │
│ prev_hash:       SHA3-256 hash of previous event     │
│ hash:            SHA3-256(prev_hash + event_data)    │
└─────────────────────────────────────────────────────┘
```

### SHA3-256 Hash Chaining

Each event's hash is computed as:

```
hash = SHA3-256(prev_hash || event_id || timestamp || sequence || event_type || actor || invariants || verdict || context)
```

This creates an immutable chain. If any historical event is modified, all subsequent hashes break, and the integrity verification system (part of GoldenTrace at L2) triggers an immediate alert.

### Genesis Event

The first event in any GoldenTrace ledger uses a well-known genesis hash:

```
prev_hash = SHA3-256("CONSTITUTIONAL_OS_GENESIS_v1")
```

---

## Deprecation Process

When data, decisions, or components become outdated, Constitutional OS follows a strict deprecation process rather than deletion:

### Step 1 — Flag
The item is marked with a `DEPRECATED` flag in a new GoldenTrace event of type `DEPRECATION`. The event includes:
- The `event_id` of the original item being deprecated
- The reason for deprecation
- The agent initiating the deprecation
- Constitutional basis (which invariant supports this action)

### Step 2 — Supersede
A new item is created that explicitly references and supersedes the deprecated item. The `references` field links the new item to the old.

### Step 3 — Annotate
The deprecated item receives a constitutional annotation visible to anyone querying the audit trail, indicating:
- That the item is deprecated
- What supersedes it
- When the deprecation occurred
- Why

### Step 4 — Preserve
The original item remains in GoldenTrace exactly as it was. It is never modified, moved, or deleted. It continues to participate in the hash chain.

```
Original Event ──────────────────────────────────────────
  │                                                      
  │  [DEPRECATION event points back to original]         
  │                                                      
  ▼                                                      
Deprecation Event ───────────────────────────────────────
  │                                                      
  │  [New event references deprecation and original]     
  │                                                      
  ▼                                                      
Superseding Event ───────────────────────────────────────
```

---

## Contradiction Resolution

When the system encounters contradictory information (e.g., two conflicting governance decisions, or a new policy that conflicts with an older one), the following process applies:

### Detection
Contradictions are detected by the Sovereignty Engine (L1) during invariant evaluation, or flagged by the adversarial seat (Grok/S3) during governance review.

### Recording
Both the contradiction and the existing conflicting records are preserved. A new GoldenTrace event of type `AUDIT` records the contradiction with references to all conflicting events.

### Resolution
The Pantheon Council convenes to resolve the contradiction through the standard governance process (see [GOVERNANCE.md](GOVERNANCE.md)):
1. Each conflicting position is analyzed
2. Adversarial review is conducted (INV-11)
3. A resolution is voted on
4. The resolution is recorded as a new event that references all prior conflicting events

### Preservation
After resolution:
- The conflicting events remain unchanged in GoldenTrace
- The resolution event supersedes them
- The contradiction itself becomes part of the system's golden history
- Future queries return the resolution but can trace back to the original contradiction

---

## Audit Trail Requirements

### Completeness
Every constitutionally significant action must have a GoldenTrace entry (INV-8). "Constitutionally significant" includes:
- All governance decisions and votes
- All consent grants and revocations
- All routing decisions
- All invariant evaluations that result in DENY or ESCALATE
- All state mutations across L1–L5
- All deprecations and amendments
- All error events with constitutional context (INV-23)

### Integrity
The hash chain must be continuously verified. Verification runs:
- On every new event append (verify previous hash)
- Periodically (full chain verification at configurable intervals)
- On demand (user or agent may request verification at any time)

Any hash chain break triggers:
- Immediate GoldenTrace integrity alert
- Constitutional incident containment (INV-32)
- Council notification
- System enters degraded mode until integrity is restored

### Accessibility
Per INV-15, the audit trail is accessible to:
- Users (their own relevant records, plus all governance records)
- Council agents (full access for governance purposes)
- The Sovereignty Engine (full access for invariant evaluation)

### Retention
GoldenTrace records are retained indefinitely. There is no expiration, no archival process that removes records, and no mechanism for bulk deletion. This is a direct consequence of INV-24.

---

## Summary

The Kintsugi policy is not a limitation — it is a feature. By preserving the complete history of the system, including its mistakes and corrections, Constitutional OS builds:

- **Trust** — through radical transparency
- **Accountability** — through immutable attribution
- **Resilience** — through learning from preserved history
- **Beauty** — through the golden traces of repair

*Every crack filled with gold tells the story of a system that heals rather than hides.*

---

*Kintsugi policy version: Alpha Genesis v1.0.0-alpha*
