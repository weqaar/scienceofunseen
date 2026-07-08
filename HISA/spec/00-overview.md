# HISA Spec 00 — Overview and the Layered Stack

HISA specifies the human being as a layered machine, exactly the stack developed in the
book chapter "Human Programmability." Each layer has its own document(s); this page is
the map.

```
┌─────────────────────────────────────────────────────────────┐
│ L7  SUPERVISING WILL (Nafs, PL3)      os/operating-system.md │  ← genuine choice
├─────────────────────────────────────────────────────────────┤
│ L6  OPERATING SYSTEM (learned)        os/operating-system.md │
├─────────────────────────────────────────────────────────────┤
│ L5  NEURAL NETWORK (learning HW)      microarch/pipeline.md  │
├─────────────────────────────────────────────────────────────┤
│ L4  HISA INSTRUCTION SET              spec/03, spec/04, 05   │  ← the ISA proper
├─────────────────────────────────────────────────────────────┤
│ L3  COMMUNICATION INTERFACES (senses) spec/01 (S0–S5)        │
├─────────────────────────────────────────────────────────────┤
│ L2  FIRMWARE (DNA / Fitrah)           spec/01 (FR register)  │
├─────────────────────────────────────────────────────────────┤
│ L1  PHYSICAL HARDWARE (cells, blood)  microarch (bus, fault) │
└─────────────────────────────────────────────────────────────┘
```

**Reading order for newcomers:**
1. This overview.
2. [`spec/01-registers-and-state.md`](01-registers-and-state.md) — what the machine holds.
3. [`spec/02-instruction-formats.md`](02-instruction-formats.md) — how instructions encode.
4. [`spec/03-base-instruction-set-LISA.md`](03-base-instruction-set-LISA.md) — the base (LISA).
5. [`spec/04-extensions-human.md`](04-extensions-human.md) — the human superset (HISA).
6. [`spec/05-vector-extension.md`](05-vector-extension.md) — the parallel engine.
7. [`microarch/pipeline.md`](../microarch/pipeline.md) — how it executes.
8. [`toolchain/compiler.md`](../toolchain/compiler.md) — how experience becomes behaviour.
9. [`os/operating-system.md`](../os/operating-system.md) — how it is managed and defended.

**Claim classes** (used throughout): **[A]** established · **[B]** model/mapping ·
**[C]** speculative. See the [README](../README.md).

**One-line thesis:** `LISA ⊆ HISA`; the human is a massively parallel, vector-native,
self-modifying machine whose highest privilege level is genuinely free — a machine strictly
beyond any silicon design because it is not bound by silicon's physics.
