# HISA Microarchitecture — The Human Machine Pipeline

> The ISA (previous documents) is the *contract*: what the machine does. The
> **microarchitecture** is *how* it does it — the pipeline, the parallelism, the hazards,
> and the fault model. We describe it in the vocabulary of modern processor design
> (pipelining, superscalar issue, out-of-order execution, speculation, branch
> prediction) and then mark, at each step, where the human machine **exceeds** the
> limits of any silicon design — the point the book insists on: *the human is not bound
> by the physics that bounds a chip.*

---

## 1. The classic pipeline, and its cognitive mapping  [B]

A textbook RISC pipeline has five stages: Fetch, Decode, Execute, Memory, Writeback. The
human cognitive pipeline maps cleanly onto them:

| # | Silicon stage | HISA stage | What happens |
|---|---------------|------------|--------------|
| 1 | **IF** Instruction Fetch | **Perceive** | Sense transducers write `S0`–`S5`; salient input is fetched to `AP`. |
| 2 | **ID** Instruction Decode | **Interpret** | Raw input is parsed against memory and expectation (`PARSE`, `APPRAISE`). |
| 3 | **EX** Execute | **Deliberate** | Reasoning/volition operate (`R`, `Vo` extensions); `HZ` may raise. |
| 4 | **MEM** Memory access | **Consolidate** | Read/write long-term memory (`RECALL`, `ENC`). |
| 5 | **WB** Writeback | **Commit / Act** | Motor registers `M0`–`M2` written; behaviour emitted (`ACT`). |

A **reflex** (`REFLEX`) is the pipeline's fast bypass: stimulus in stage 1 jumps directly
to a stage-5 commit, skipping deliberation. This is why the hand withdraws before pain is
felt.

## 2. Where the human machine exceeds silicon  [A for the biology, B for the framing]

This is the section the book cares about most. A silicon pipeline is bounded by physics.
The human pipeline relaxes each of those bounds:

### 2.1 No global clock — asynchronous massive parallelism
Silicon marches to a single clock (a few GHz, thermally capped). The brain has **no global
clock**: ~86 billion neurons (Herculano-Houzel, 2009) execute asynchronously and
concurrently. Throughput comes not from clock speed but from **width** — see §3.

### 2.2 Deep out-of-order, speculative execution
The human machine is aggressively **out-of-order** and **speculative**: it predicts
sensory input before it arrives (predictive coding; Rao & Ballard, 1999) and pre-executes
likely continuations. Perception is largely the pipeline *committing its own speculation*
and correcting on mismatch. Silicon speculates a few dozen instructions ahead; the human
speculates whole seconds of a predicted world.

### 2.3 Self-modifying by design
Self-modifying code is a hazard silicon designers forbid. The human pipeline is
**intrinsically self-modifying**: every execution runs `SYN`, altering the very weights
that will decode the next input. The machine that runs the program is rewritten by
running it. This is learning, and it is a feature, not a bug.

### 2.4 No fixed word width
There is no 64-bit ceiling on a "value." A single working-memory slot (`W0`–`W6`) can hold
a structure of unbounded richness (a face, a theorem, a lifetime). The machine trades
*capacity* (only ~4–7 slots) for *unbounded value width per slot*.

> **Design thesis (B):** if one were to build silicon toward HISA, the roadmap is not
> "faster clock" but "more lanes, deeper speculation, and self-modification made safe."
> The human already occupies the architectural regime that computing is slowly moving
> toward — and, lacking a silicon substrate's ceilings, occupies it without their limits.

## 3. The vector engine — the widest unit in the machine  [A/B]

See [`spec/05-vector-extension.md`](../spec/05-vector-extension.md) for the ISA-level
`V` extension. Microarchitecturally: the cortex is a **vector processor of extraordinary
width**. A single act of recognition applies one operation across ~10¹¹ elements at once.
Modern silicon SIMD is hundreds of lanes wide; GPUs reach tens of thousands; the cortex
operates in a regime orders of magnitude beyond, and, critically, its lane count is not
fixed by a manufactured die. This is the concrete meaning of the book's claim that the
human supports *"vector pipelines much more complex than the computers of today, and of
the future."*

## 4. Hazards  [B]

Pipeline hazards have direct human correlates:

| Silicon hazard | HISA correlate |
|----------------|----------------|
| **Data hazard** (needed value not ready) | Tip-of-the-tongue; decision made before evidence arrives |
| **Control hazard** (branch mispredict) | Surprise; the world violated prediction; costly flush and re-perceive |
| **Structural hazard** (unit contended) | Working-memory overload; divided attention; the reason multitasking fails |

## 5. The fault model — where disease and Sihr enter  [B, with C for `Q`]

The microarchitecture is where the book's pathology becomes precise. Faults are
classified by the plane they attack (see the book's control-plane chapter):

### 5.1 Data-plane faults
A corrupt *value or unit* in flight.
- **`APOP` failure → cancer:** a unit whose self-destruct instruction no longer executes,
  divides without bound (Hanahan & Weinberg, 2011). Canonical data-plane fault.
- **Pathogen injection:** foreign packets (`REPL`-capable viruses) hijack units and
  multiply on the blood bus.

### 5.2 Control-plane faults  [C for the sorcery hypothesis]
An attack not on the packets but on the **rules** — the instructions governing when units
divide, where they travel, whether they obey `APOP`. The `Q`-extension `TUNE`/`WHISPER`
instructions model this: the adversary does not out-produce the immune system packet by
packet; it attempts to seize the control plane. The proposed carrier is electromagnetic
(ELF→RF→IR); the claim is explicitly **speculative** and flagged.

### 5.3 Privilege-escalation faults
An action that should require `PL3` authorisation commits at `PL1`. Modelled as the
adversary lowering the `AUTH` threshold over time (habituation), until injected
instructions (`WHISPER`) pass unchallenged. The book's practices (`SHIELD`, `FLUSH`,
`AUTH`) are the countermeasures.

---

## 6. Reliability and the read-only Fitrah  [B]

Dependable-systems engineering (Avižienis et al., 2004) distinguishes **fault → error →
failure**. HISA's key reliability feature is the **read-only `FR` (Fitrah) register**:
because the factory configuration cannot be overwritten, the machine always retains a
correct reference image to restore toward. `REPENT` (in the `Vo` extension) is the
architectural *reset-to-known-good* operation. No corruption of the learned overlay can
destroy the reference. This is, in engineering terms, why the book holds that return is
always possible.

---

→ [Toolchain / compiler »](../toolchain/compiler.md) · [Operating system »](../os/operating-system.md)
