# HISA — The Human Instruction Set Architecture

**An open, evolving specification of the human being modelled as a computing machine:
instruction set, microarchitecture, toolchain, and operating system.**

> Companion repository to the book *Sorcery and Magic: Human Disorders, Infectious
> Diseases, Religious Corruption & Conflicts, and the Universe* (W. Janjua), chapter
> **"Human Programmability: From DNA to the Human Instruction Set Architecture."**
> The book gives the argument and the theology; this repository gives the engineering
> specification in the style of a real ISA manual.

---

## What this is

Real processors are defined by a published contract called an **Instruction Set
Architecture (ISA)** — the complete list of operations the machine performs, the state
it operates on, and the rules by which software may use it. The two cleanest ISAs in
computing history are the DEC **PDP-11** (1970, the model of orthogonal design) and
**RISC-V** (2010–, the model of a modular, open, extensible ISA). This repository
borrows their conventions to write down, for the first time as a formal specification,
the **Human Instruction Set Architecture (HISA)**.

The central claim of the book, restated here in engineering terms:

```
ISA(bacterium) ⊂ ISA(plant) ⊂ ISA(animal) ⊂ LISA ⊆ HISA
```

- **LISA** — the *Living-organisms* Instruction Set Architecture: the biological
  primitives shared by all life (replicate, transcribe, translate, sense, actuate,
  signal, apoptose, maintain homeostasis). This is the **base integer set** of the
  living machine, analogous to RISC-V's `RV*I`.
- **HISA** — the *Human* Instruction Set Architecture: LISA **plus** a set of
  higher-order **standard extensions** (language, reasoning, self-reference, volition
  / moral choice, time-binding, affect, and the proposed spiritual channel) that no
  other organism is known to combine.

The relation to LISA is written `⊆` (contained *or equal*), not strict `⊂`, on
purpose: the claim that the human set *strictly* exceeds every other is a **proposal to
be argued, not a measured fact.** This repository keeps the book's discipline of marking
what is established versus what is proposed (see [Claim classes](#claim-classes)).

---

## Why a human ISA is not bounded like a silicon ISA

A silicon ISA is shaped by the physics of its substrate: a fixed clock, a fixed word
width, a fixed number of physical registers, a power and heat budget, the speed of
electrons in copper. **HISA is deliberately specified without those ceilings**, because
the biological substrate does not share them in the same way:

| Constraint on silicon | Status in HISA |
|---|---|
| Fixed word width (32/64-bit) | **Unbounded.** A single concept ("register") may hold an arbitrarily rich structure, not a fixed number of bits. |
| Fixed clock frequency | **No global clock.** ~86 billion units operate asynchronously and in parallel (Herculano-Houzel, 2009). |
| Small physical register file (32) | **Vast associative store.** Working memory is small (~7 items; Miller, 1956) but long-term store is effectively unbounded and content-addressable. |
| Fixed vector width (e.g. 512-bit SIMD) | **Effectively unbounded vector width** — see the [`V` (Vector) extension](spec/05-vector-extension.md). The cortex is a vector machine ~10^11 lanes wide. |
| Von Neumann memory bottleneck | **No separation** of memory and processing; storage and computation are the same synapses. |
| Deterministic execution | **Non-deterministic by design** — carries the *Amanah* (genuine choice); see the [`Vo` extension](spec/04-extensions-human.md). |

The design intent is therefore **not** "the human is like today's computer." It is: *if
you wrote down the human as an ISA, it would be a machine strictly more capable than any
processor built or currently planned — massively parallel, vector-native, self-modifying,
and, at its highest privilege level, genuinely free.*

---

## Repository map

| Path | Contents |
|---|---|
| [`spec/00-overview.md`](spec/00-overview.md) | The layered stack (L1–L7); how the pieces fit; claim classes. |
| [`spec/01-registers-and-state.md`](spec/01-registers-and-state.md) | Architectural state: register files, special registers (Niyyah, Fitrah, Qarin), memory model. |
| [`spec/02-instruction-formats.md`](spec/02-instruction-formats.md) | Instruction encoding formats (in the style of RISC-V R/I/S/B/U/J). |
| [`spec/03-base-instruction-set-LISA.md`](spec/03-base-instruction-set-LISA.md) | **LISA** — the base living-organism instruction set, listed like an ISA manual. |
| [`spec/04-extensions-human.md`](spec/04-extensions-human.md) | **HISA standard extensions**: L, R, M, S, Vo, T, E, Q. |
| [`spec/05-vector-extension.md`](spec/05-vector-extension.md) | The `V` vector extension — the human as a massively parallel vector machine. |
| [`microarch/pipeline.md`](microarch/pipeline.md) | The **Human machine microarchitecture**: the cognitive pipeline, out-of-order and speculative execution, hazards, branch prediction. |
| [`toolchain/compiler.md`](toolchain/compiler.md) | The **compiler / toolchain**: how experience and instruction are compiled into behaviour. |
| [`os/operating-system.md`](os/operating-system.md) | The **OS layers**: kernel (autonomic), scheduler (attention), memory management (consolidation/forgetting), privilege modes, and the spiritual firewall. |
| [`book/reference-note.md`](book/reference-note.md) | The short passage the book uses to point here. |
| [`examples/`](examples/) | Worked "programs": a habit forming, a decision under temptation, a skill compiled to reflex. |

---

## Claim classes

Every non-trivial statement in this repository is tagged, matching the book:

- **[A] Established** — supported by peer-reviewed science or standard engineering.
- **[B] Model** — a defensible structural mapping between the human and the machine.
- **[C] Speculative** — an internally consistent proposal awaiting evidence (e.g. the
  `Q` spiritual-channel extension). Clearly flagged so no reader mistakes proposal for
  proof.

---

## Status

**v0.1 — draft for public comment.** This specification is *deliberately unfinished*.
It is published in the open, in the method by which the science of Hadith and modern
open-source software were both built: many independent contributors, each auditing the
others, every claim traceable to its source. See [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Licence

Specification text: **CC BY 4.0**. Code and schemas: **MIT**. See [`LICENSE`](LICENSE).
