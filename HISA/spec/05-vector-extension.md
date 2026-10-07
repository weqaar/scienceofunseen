# HISA Spec 05 - The `V` (Vector) Extension

> RISC-V's `V` extension adds vector operations that apply one instruction across many
> data elements at once (SIMD/vector processing). HISA's `V` extension is the same idea
> taken to a regime no manufactured chip reaches. This is the concrete, ISA-level
> statement of the book's claim that the human supports **"vector pipelines far more
> complex than the computers of today, and of the future."**

## 1. Why the human is a vector machine  [A]

A single act of recognition - seeing a face, catching a ball, parsing a sentence - applies
the *same* operation across an enormous number of elements simultaneously. The visual
system alone takes in about 10⁶ retinal nerve fibres in parallel, feeding a cerebral
cortex of about 1.6 x 10¹⁰ neurons (Azevedo et al., 2009). There is no serial loop over pixels; there is one massively wide vector
operation. The brain is, natively, a vector processor.

## 2. Where it exceeds any silicon vector unit  [A for scale, B for framing]

| Property | Silicon SIMD / GPU | HISA `V` |
|----------|--------------------|----------|
| Vector width (lanes) | 4 to 512 (CPU SIMD); about 10⁴ to 10⁵ (GPU) | up to billions of cortical neurons engaged together, each a small network in its own right ([`N` §3](06-neural-network-extension.md)) |
| Lane count fixed at manufacture | Yes | **No** - recruited dynamically; plastic |
| Clocked | Yes (thermal cap) | **Asynchronous**; coordinated by local rhythms (`SYNC`), not one clock |
| Elements are fixed-width | Yes (e.g. 32-bit floats) | **No** - each element an arbitrarily rich value |
| Precision | Fixed | **Mixed/adaptive** (sharp where attended, coarse elsewhere) |

> The design point of the book: to move silicon *toward* the human, you do not raise the
> clock - you add lanes, make lane-count dynamic, and drop the fixed word width. The human
> already sits in that regime, and, having no silicon substrate, without its ceilings.

## 3. Vector instructions  [A/B]

| Mnemonic | Operands | Action |
|----------|----------|--------|
| `VLOAD`  | `vd, rs1` | Load a wide sensory field (e.g. a whole visual scene) into vector register `vd` |
| `VMATCH` | `vd, vs1, vs2` | Apply a template `vs2` across all lanes of `vs1` in parallel (recognition) |
| `VREDUCE`| `rd, vs1` | Collapse a vector to a scalar summary (gist extraction) |
| `VMASK`  | `vd, vs1, imm` | Attend to a subset of lanes (selective attention as a vector mask) |
| `VBIND`  | `vd, vs1, vs2` | Bind features across lanes into an object (the binding operation) |

> `VMASK` is attention expressed as a vector predicate: the machine does not process one
> thing at a time; it processes *everything* at once and **masks** to a subset. This
> inverts the naive picture of attention as a spotlight scanning a dark room - the room is
> already fully lit in parallel; attention is a mask over an all-at-once computation.

## 4. Consequence for prediction  [B]

The vector width helps you picture why predicting a person in full is so hard: the joint
state of billions of interacting elements, updated in parallel and non-linearly, has a
configuration space far too large to search exhaustively. The comparison stops there. The
book is explicit that a person "is not a precisely defined computational task such as
those classified as NP-hard," and that difficulty in predicting someone proves neither free
will nor a human instruction set. HISA therefore makes no complexity-class claim about
people.

← [Human extensions](04-extensions-human.md) · [Neural network extension »](06-neural-network-extension.md) · [Microarchitecture »](../microarch/pipeline.md)
