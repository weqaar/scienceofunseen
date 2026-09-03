# HISA Spec 02 - Instruction Formats

> Modelled on RISC-V's fixed-length, regular encoding formats (R/I/S/B/U/J). Real silicon
> uses fixed bit-fields; HISA is not implemented in bits, so "format" here means the
> **operand shape** of an instruction - how many sources, whether an immediate is present,
> whether it branches. The regularity is the point: like RISC-V and the PDP-11, HISA aims
> for **orthogonality** - any register usable by any operation of the right shape.

| Format | Shape | Fields | Example instruction |
|--------|-------|--------|---------------------|
| **R** (register) | reg-reg-reg | `rd, rs1, rs2` | `MERGE rd, rs1, rs2` |
| **I** (immediate/unary) | reg + immediate | `rd, rs1` or `rs1, imm` | `REPL rs1` · `EXPR rs1, imm` |
| **S** (store/adjust) | two src + immediate, no dest reg | `rd, rs1, imm` | `SYN rd, rs1, imm` |
| **B** (branch) | two src + target | `rs1, rs2, imm` | `BThr rs1, rs2, imm` |
| **U** (upper/pointer) | dest + large immediate | `rd/AP, imm` | `ATTN imm` |
| **J** (jump/intent) | target only | `imm` / special reg | `INTEND NR, rs1` |

**Conventions (borrowed from RISC-V):**
- `rd` is always written; `rs1`,`rs2` always read.
- Immediates are sign-extended where meaningful (e.g. `SYN` weight change may be negative:
  synaptic *depression*).
- One register in each file reads as an inert zero-equivalent (cf. RISC-V `x0`) - a "no
  effect" target used to discard results.

**Orthogonality caveat.** Three registers are *not* fully orthogonal, by design:
- `FR` (Fitrah) is **read-only** - it can never be a `rd`.
- `NR` (Niyyah) is **privileged** - only `INTEND`/`WILL` at `PL2`+ may write it.
- `QC` (Qarin channel) is **read-only** and available only under the [C] `Q` extension.

→ [Base instruction set (LISA) »](03-base-instruction-set-LISA.md)
