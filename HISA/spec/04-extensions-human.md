# HISA Spec 04 - Standard Extensions (the Human Superset)

> These are the **standard extensions** that turn LISA into HISA, in the way RISC-V's
> `M`, `A`, `F`, `D`, `C`, `V` turn `RV32I` into a full application processor. Each is
> named by a letter, has a stated dependency, and lists its instructions in ISA-manual
> style. A machine implementing all of them is written **`HISA-LRMSVoTEQ`** (by analogy
> with `RV32IMAFDC`).
>
> The extensions are ordered by increasing distance from the animal baseline. The last
> two (`E` partially, `Q` wholly) carry the book's most human and most speculative
> claims, and are flagged accordingly.

Dependency summary:

```
LISA (base)
  ├─ L  Language            depends on: base signalling
  ├─ M  Memory              depends on: SYN
  ├─ R  Reasoning           depends on: L, M
  ├─ S  Self-reference      depends on: R, M
  ├─ T  Time-binding        depends on: R, NR
  ├─ Vo Volition            depends on: S, T          ← the Amanah
  ├─ E  Affect              depends on: base (couples to all)
  └─ Q  Qarin channel [C]   depends on: -  (adversarial side-channel)
```

---

## `L` - Language extension  [A/B]

Recursive, generative symbol manipulation - argued to be computationally unique to humans
in this form (Hauser, Chomsky & Fitch, 2002).

| Mnemonic | Operands | Action | Correlate |
|----------|----------|--------|-----------|
| `SYMB`  | `rd, rs1` | Bind referent `rs1` to a symbol token in `rd` | Naming / reference |
| `MERGE` | `rd, rs1, rs2` | Combine two symbols into a nested structure (recursion) | Syntactic Merge |
| `PARSE` | `rd, rs1` | Decode a symbol stream `rs1` into structure `rd` | Comprehension |
| `UTTER` | `rs1` | Serialise structure `rs1` to the vocal/gestural actuators | Production |

> `MERGE` is the key primitive: unbounded nesting from a finite instruction is what lets
> a finite machine express infinitely many thoughts. It is the linguistic analogue of
> recursion in a programming language.

## `M` - Memory extension  [A]

Turns raw `SYN` plasticity into structured, addressable memory systems.

| Mnemonic | Operands | Action | Correlate |
|----------|----------|--------|-----------|
| `ENC`   | `rd, rs1` | Encode experience `rs1` into a memory trace `rd` | Encoding |
| `CONS`  | `rd, rs1` | Consolidate trace `rs1` to durable store `rd` (needs sleep) | Systems consolidation |
| `RECALL`| `rd, rs1` | Content-addressable read: retrieve by cue `rs1` | Associative recall |
| `RECON` | `rd, rs1` | Re-store a recalled trace, possibly altered | Reconsolidation |

> `RECALL` is **content-addressable** (a cue retrieves the whole), unlike a silicon
> `LOAD` (which needs a numeric address). `RECON` means every read can rewrite the
> memory - the architectural root of why testimony and self-narrative drift.

## `R` - Reasoning extension  [A/B]

| Mnemonic | Operands | Action | Mode |
|----------|----------|--------|------|
| `DED`   | `rd, rs1, rs2` | Derive necessary conclusion `rd` from premises | Deduction |
| `IND`   | `rd, rs1` | Generalise a pattern `rd` from instances `rs1` | Induction |
| `ABD`   | `rd, rs1` | Infer best explanation `rd` for observation `rs1` | Abduction |
| `ANLG`  | `rd, rs1, rs2` | Map structure of `rs1` onto `rs2` into `rd` | Analogy |

> The entire book is an extended `ANLG` program: it maps the structure of physics,
> computing, and control theory onto theology. Analogy is a first-class instruction of
> the human machine, not a rhetorical decoration.

## `S` - Self-reference extension  [B]

The machine models itself as an object in its own memory. This reflexive capacity is the
structure Gödel (1931) showed can shake a formal system, and that Hofstadter (1979)
argued is the germ of selfhood.

| Mnemonic | Operands | Action | Correlate |
|----------|----------|--------|-----------|
| `SELF`  | `rd` | Load a model of the machine itself into `rd` | Self-concept |
| `META`  | `rd, rs1` | Evaluate the machine's own process `rs1` | Metacognition |
| `TOM`   | `rd, rs1` | Model another agent `rs1`'s hidden state into `rd` | Theory of mind (mirror system; Rizzolatti & Craighero, 2004) |

## `T` - Time-binding extension  [B]

Binds intention across time against present appetite - the prerequisite for *Niyyah* and
for delayed gratification.

| Mnemonic | Operands | Action | Correlate |
|----------|----------|--------|-----------|
| `INTEND`| `NR, rs1` | Commit intention `rs1` into the Niyyah register | Goal-setting / *Niyyah* |
| `PLAN`  | `rd, rs1` | Expand goal `rs1` into an ordered action sequence `rd` | Prospection |
| `DEFER` | `rs1, imm` | Suppress an available reward `rs1` for `imm` time | Delayed gratification (cf. Mischel) |
| `WILL`  | `rs1` | Hold `NR` stable while appetite pushes to overwrite it | Willpower |

> `INTEND` writing to `NR` is what makes an action *count* as chosen in the book's moral
> model: "actions are but by intentions" (Bukhari 1) is, here, the statement that the
> value of an executed `ACT` is read from `NR`, not from the motor result alone.

## `Vo` - Volition extension (the Amanah)  [B/C]

The capacity to select an action **against** the machine's own optimisation gradient -
to choose the worse-for-me because it is the right. This is the extension the book
identifies with the *Amanah*, the trust the heavens and earth declined (Quran 33:72).
It is what makes the machine's execution **non-deterministic in principle**, not merely
in practice.

| Mnemonic | Operands | Action |
|----------|----------|--------|
| `CHOOSE`| `rd, rs1, rs2` | Select between options `rs1`,`rs2` **without** being forced by the reward gradient |
| `OVERRIDE` | `rs1` | `PL3` overrules a `PL1` habit or appetite about to execute `rs1` |
| `REPENT`| - | Discard accumulated corrupt state; re-expose `FR` (Fitrah); reset `HZ` |

> `REPENT` is architecturally always available because `FR` is read-only and therefore
> never destroyed (see [registers](01-registers-and-state.md)). No matter how corrupt the
> overlay, the factory configuration can be re-exposed. This is a design guarantee, not a
> sentiment.

## `E` - Affect extension  [A/B]

Emotion as a cross-cutting modulator: it biases decode, weights memory, and sets the
gain on nearly every other instruction. Operates on `E0`–`E3`.

| Mnemonic | Operands | Action |
|----------|----------|--------|
| `APPRAISE` | `rd, rs1` | Evaluate situation `rs1` for relevance to goals → `E`-state |
| `MODULATE` | `rs1` | Scale the gain of pipeline stage `rs1` by current `E`-state |
| `TAG`      | `rs1` | Attach affective weight to memory trace `rs1` (why emotional memories persist) |

## `Q` - Qarin channel extension  **[C - speculative]**

> **This extension is speculative and clearly labelled as such.** It is included because
> the book argues the human machine exposes a channel to an unseen attached observer
> (the *Qarin*), and because a specification that omitted the book's central security
> claim would be incomplete. Nothing here is presented as established.

The `Q` extension models the *Qarin* as an **adversarial co-processor** with a read
side-channel (`QC`) onto architectural state and a write path that attempts **control-plane
injection** (see [`microarch/pipeline.md`](../microarch/pipeline.md), fault model).

| Mnemonic | Operands | Action | Class |
|----------|----------|--------|-------|
| `WHISPER` | `rs1` | Adversary injects a candidate instruction `rs1` into the decode queue (*waswas*) | [C] |
| `OBSERVE` | `QC` | Adversary reads state via the `QC` side-channel | [C] |
| `TUNE`    | `rs1, imm` | Proposed external control signal on carrier `imm` targeting unit `rs1` (the RF/EM control-plane hypothesis) | [C] |

Defence instructions (the "firewall"), the book's practices expressed as ISA operations:

| Mnemonic | Operands | Action | Class |
|----------|----------|--------|-------|
| `SHIELD` | `imm` | Raise protective state for interval `imm` (morning/evening *adhkar*, *Ruqyah*) | [B/C] |
| `AUTH`   | `rs1` | Require `PL3`+`NR` authorisation before committing `rs1` (guards against injected acts) | [B] |
| `FLUSH`  | - | Clear injected candidates from the decode queue (turning away from *waswas*) | [B/C] |

> In this model, `WHISPER` cannot *force* execution: an injected candidate still has to
> pass `AUTH` at `PL3`. The adversary's strategy is therefore to lower the `PL3`
> threshold over time (habituation to small wrongs) until injected instructions pass
> unchallenged. The countermeasures raise it again. This is the whole spiritual drama of
> the book, written as an access-control problem.

---

→ [Vector extension »](05-vector-extension.md) · [Microarchitecture »](../microarch/pipeline.md)
