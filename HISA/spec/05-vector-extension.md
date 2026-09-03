# HISA Spec 05 - The `V` (Vector) Extension

> RISC-V's `V` extension adds vector operations that apply one instruction across many
> data elements at once (SIMD/vector processing). HISA's `V` extension is the same idea
> taken to a regime no manufactured chip reaches. This is the concrete, ISA-level
> statement of the book's claim that the human supports **"vector pipelines far more
> complex than the computers of today, and of the future."**

## 1. Why the human is a vector machine  [A]

A single act of recognition - seeing a face, catching a ball, parsing a sentence - applies
the *same* operation across an enormous number of elements simultaneously. The visual
system alone processes on the order of 10⁶ retinal inputs in parallel through a cortex of
~10¹¹ neurons. There is no serial loop over pixels; there is one massively wide vector
operation. The brain is, natively, a vector processor.

## 2. Where it exceeds any silicon vector unit  [A for scale, B for framing]

| Property | Silicon SIMD / GPU | HISA `V` |
|----------|--------------------|----------|
| Vector width (lanes) | 4–512 (CPU SIMD); ~10⁴–10⁵ (GPU) | ~10¹¹ elements engaged in a single perceptual op |
| Lane count fixed at manufacture | Yes | **No** - recruited dynamically; plastic |
| Clocked | Yes (thermal cap) | **Asynchronous**, no global clock |
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

## 4. Consequence for the complexity claim  [B/C]

The vast vector width is one of the structural reasons the book proposes that *complete
prediction of a human is at least NP-hard*: the joint state of ~10¹¹ interacting elements,
updated in parallel and non-linearly, has a configuration space whose exhaustive analysis
explodes. The vector engine is not just fast; it is a source of the machine's genuine
analytical intractability from the outside.

← [Human extensions](04-extensions-human.md) · [Microarchitecture »](../microarch/pipeline.md)
