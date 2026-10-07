# HISA Spec 00 - Overview and the Layered Stack

HISA describes the human being as a layered machine. This is an analogy **[B]**: the
book this repository accompanies holds that a person remains more than a machine
executing instructions. Each layer has its own document(s); this page is the map.

```
┌─────────────────────────────────────────────────────────────┐
│ L7  SUPERVISING WILL (Nafs, PL3)      os/operating-system.md │  ← genuine choice
├─────────────────────────────────────────────────────────────┤
│ L6  OPERATING SYSTEM (learned)        os/operating-system.md │
├─────────────────────────────────────────────────────────────┤
│ L5  NEURAL NETWORK (learning HW)      spec/06, microarch     │
├─────────────────────────────────────────────────────────────┤
│ L4  HISA INSTRUCTION SET              spec/03, spec/04, 05   │  ← the ISA proper
├─────────────────────────────────────────────────────────────┤
│ L3  COMMUNICATION INTERFACES (senses) spec/01 (S0 to S5)     │
├─────────────────────────────────────────────────────────────┤
│ L2  INNATE CONFIGURATION (DNA; Fitrah) spec/01 (FR register) │
├─────────────────────────────────────────────────────────────┤
│ L1  PHYSICAL HARDWARE (cells, blood)  microarch (bus, fault) │
└─────────────────────────────────────────────────────────────┘
```

**Note on L2.** DNA (biological inheritance) and the *Fitrah* (the original disposition
taught in Islam) share a layer here only because both are present before learning begins.
The book treats them as different kinds of claim, and so does this specification: DNA is
biology [A]; the `FR` register's modelling of the *Fitrah* is an analogy [B].

**Reading order for newcomers:**
1. This overview.
2. [`spec/01-registers-and-state.md`](01-registers-and-state.md) - what the machine holds.
3. [`spec/02-instruction-formats.md`](02-instruction-formats.md) - how instructions encode.
4. [`spec/03-base-instruction-set-LISA.md`](03-base-instruction-set-LISA.md) - the base (LISA).
5. [`spec/04-extensions-human.md`](04-extensions-human.md) - the human superset (HISA).
6. [`spec/05-vector-extension.md`](05-vector-extension.md) - the parallel engine.
7. [`spec/06-neural-network-extension.md`](06-neural-network-extension.md) - why the ISA is
   learned and plastic, not static.
8. [`microarch/pipeline.md`](../microarch/pipeline.md) - how it executes.
9. [`toolchain/compiler.md`](../toolchain/compiler.md) - how experience becomes behaviour.
10. [`os/operating-system.md`](../os/operating-system.md) - how it is managed and defended.

**Claim classes** (used throughout): **[A]** established · **[B]** model/mapping ·
**[C]** speculative. See the [README](../README.md).

**One-line thesis:** `LISA ⊆ HISA`; written as an ISA, the human is a massively parallel,
vector-native, self-modifying machine whose instruction set is **learned and keeps
changing** (the `N` extension), and whose highest privilege level the book holds to be
genuinely free [B/C].
