# HISA Spec 03 - The Base Instruction Set (LISA)

> **LISA** is the base integer set of the living machine - the operations shared, in
> subsets, by *all* life. It is to HISA what `RV32I` is to RISC-V: the mandatory core on
> which every extension builds. Every instruction below is listed in ISA-manual style:
> **mnemonic**, **operands**, **format** (see [02](02-instruction-formats.md)), a
> one-line **action**, and the **biological correlate** it abstracts. Claim class in
> brackets; the base set is uniformly **[A]** unless noted.

Notation: `rd` destination register, `rs1`/`rs2` source registers, `imm` immediate,
`[x]` "contents of x", `←` assignment.

---

## 1. Maintenance & lifecycle group

| Mnemonic | Operands | Fmt | Action | Biological correlate |
|----------|----------|-----|--------|----------------------|
| `HOMEO`  | `rd, rs1` | R | Drive a regulated variable toward its set-point: `rd ← setpoint(rs1)` | Homeostasis (thermo-, osmo-, glucoregulation) |
| `METAB`  | `rd, rs1` | R | Convert substrate `rs1` to usable energy into `rd` | Metabolism / respiration |
| `REPL`   | `rs1` | I | Duplicate the unit identified by `rs1` (mitosis) | Cell division |
| `APOP`   | `rs1` | I | **Programmed self-destruct** of unit `rs1` for the good of the whole | Apoptosis (Kerr et al., 1972) |
| `REPAIR` | `rd, rs1` | R | Attempt to restore `rs1` to its `FR`-specified state | DNA repair, wound healing |

> **`APOP` is the most important instruction in the base set for this book.** Cancer is
> precisely the state in which a unit's `APOP` has been corrupted and no longer executes
> (see the Diseases chapter and `microarch` fault model). Its corruption is the canonical
> **data-plane fault**.

## 2. Genetic-execution group (the firmware pipeline)

| Mnemonic | Operands | Fmt | Action | Correlate |
|----------|----------|-----|--------|-----------|
| `TXN`  | `rd, rs1` | R | Transcribe code segment `rs1` (DNA) to working copy `rd` (mRNA) | Transcription |
| `TRL`  | `rd, rs1` | R | Translate working copy `rs1` (mRNA) into machine part `rd` (protein) | Translation |
| `FOLD` | `rd, rs1` | R | Fold linear part `rs1` into functional 3-D form `rd` | Protein folding |
| `EXPR` | `rs1, imm` | I | Set expression level of gene `rs1` to `imm` (regulation) | Gene regulation / epigenetics |

> These four implement the **central dogma** (Crick, 1970) as a firmware execution
> pipeline: `TXN → TRL → FOLD`. `EXPR` is the control input that decides *which*
> instructions run - the biological equivalent of conditional compilation.

## 3. Signalling group (the interconnect)

| Mnemonic | Operands | Fmt | Action | Correlate |
|----------|----------|-----|--------|-----------|
| `FIRE`  | `rs1` | I | Emit an action potential from unit `rs1` | Neuron firing |
| `SYN`   | `rd, rs1, imm` | S | Adjust synaptic weight from `rs1` to `rd` by `imm` | Synaptic plasticity (Hebb, 1949) |
| `HORM`  | `rs1, imm` | I | Broadcast chemical signal `rs1` at level `imm` over the bus | Endocrine signalling |
| `IMMUN` | `rd, rs1` | R | Tag `rs1` as self/non-self; dispatch response to `rd` | Immune recognition |

> `SYN` is the **write instruction of the learning hardware** - the single primitive
> whose repeated execution *is* learning. The toolchain's optimiser (see
> [`toolchain/compiler.md`](../toolchain/compiler.md)) is, at bottom, a scheduler of
> `SYN` operations.

## 4. Transduction & actuation group (the Layer-3/Layer-1 boundary)

| Mnemonic | Operands | Fmt | Action | Correlate |
|----------|----------|-----|--------|-----------|
| `SENSE` | `rd, rs1` | R | Transduce external energy on port `rs1` into value `rd` | Sensory transduction (writes `S0`–`S5`) |
| `ACT`   | `rs1` | I | Dispatch motor command `rs1` to an actuator | Muscle contraction (reads `M0`–`M2`) |
| `SECR`  | `rs1, imm` | I | Secrete substance `rs1` at level `imm` | Glandular secretion |

## 5. Control-flow group

| Mnemonic | Operands | Fmt | Action | Correlate |
|----------|----------|-----|--------|-----------|
| `ATTN`  | `imm` | U | Load the attention pointer: `AP ← imm` (attend to target) | Attention shift |
| `BThr`  | `rs1, rs2, imm` | B | Branch if `[rs1]` crosses threshold `rs2` to target `imm` | Threshold-gated response |
| `REFLEX`| `rs1, imm` | I | Fast fixed path: on stimulus `rs1`, `ACT imm` bypassing higher layers | Reflex arc (spinal) |
| `HALT`  | - | - | Cease execution of the unit | Death (of a cell; at the organism scale, the one disease with no cure) |

> `REFLEX` is the machine's real-time interrupt: it commits an action *before* the
> deliberative pipeline can run, which is why a hand leaves a hot surface before the pain
> is consciously felt. Skills "compiled to reflex" (see toolchain) install new
> `REFLEX` paths.

---

## 6. What LISA deliberately lacks

LISA is a complete *animal-grade* machine. It **cannot**, by itself:

- represent an unlimited hierarchy of nested symbols (no recursive language),
- model *itself* as an object (no self-reference),
- bind an intention across time against present appetite (no deferred volition),
- choose against its own optimisation gradient (no moral freedom).

Those four gaps are exactly what the **HISA standard extensions** add. A machine running
only LISA is, in the book's terms, a magnificent animal. It is not yet *Ashraf
ul-Makhlooqat*.

→ [HISA standard extensions »](04-extensions-human.md)
