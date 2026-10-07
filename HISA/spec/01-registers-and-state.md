# HISA Spec 01 - Registers and Architectural State

> Style note: this document follows the *register-file* conventions of RISC-V (a small
> named set of fast registers plus a large memory) and the *orthogonality* of the
> PDP-11 (any register usable by any operation, save documented exceptions).

The **architectural state** is everything the machine can hold and operate on. In a
silicon CPU this is a handful of registers plus main memory. In HISA the state is larger
and more structured, but the same idea holds: a small set of fast, named **registers**
the machine computes with directly, backed by a vast **memory**.

---

## 1. Register files

### 1.1 Sensory input registers - `S0` to `S5` (the Layer-3 ports) - [A]

Read-mostly registers into which the sense transducers write. One per primary modality.
Unlike silicon input ports, each holds a *rich structured value*, not a fixed-width word.

| Reg | Modality | Native range (the "bus width" of the sense) |
|-----|----------|---------------------------------------------|
| `S0` | Touch / somatosensation | pressure, temperature, nociception |
| `S1` | Vision | electromagnetic 380 to 750 nm |
| `S2` | Audition | acoustic 20 Hz to 20 kHz |
| `S3` | Olfaction | airborne chemistry |
| `S4` | Gustation | dissolved chemistry |
| `S5` | Vestibular / proprioception | acceleration, joint & body position |

> **Note (ties to the book):** each register's range is a *narrow tuned band*. What
> falls outside all of `S0` to `S5` is, to this machine, part of the **Unseen**. A proposed
> extra port `S6` (ambient electromagnetic field, ELF→IR) is defined only under the
> **[C]** `Q` extension.

### 1.2 Working registers - `W0` to `W6` - [A]

The general-purpose computational registers: the contents of conscious working memory.
Famously *small* - about seven items (Miller, 1956), later revised toward four chunks
(Cowan, 2001). This is the single tightest resource in the machine and the reason
attention must be scheduled (see the OS scheduler).

- `W0` to `W6`: general working registers (orthogonal; any faculty may read/write them).
- Overflow of the working set forces eviction to long-term memory or loss - the origin
  of *forgetting under load*.

### 1.3 Affective state registers - `E0` to `E3` - [A]

Hold the current emotional state, which biases every downstream operation (emotion is
not decorative; it is an input to decoding and execution). Modelled on a low-dimensional
affect space (valence, arousal, dominance; Russell, 1980) plus a discrete-emotion tag.

| Reg | Holds |
|-----|-------|
| `E0` | Valence (pleasant ↔ unpleasant) |
| `E1` | Arousal (calm ↔ activated) |
| `E2` | Dominance (in-control ↔ controlled) |
| `E3` | Discrete-emotion tag (fear, anger, joy, grief, …) |

### 1.4 Motor / actuation registers - `M0` to `M2` - [A]

Write registers whose contents are dispatched to the actuators (muscles, glands,
vocal tract). The *commit* stage of the pipeline writes here.

---

## 2. Special registers

These carry the concepts that make HISA more than an animal ISA. Several are the direct
subject of the book.

| Reg | Name | Access | Meaning | Class |
|-----|------|--------|---------|-------|
| `AP` | **Attention Pointer** | R/W | The "program counter" of cognition: what the machine is currently attending to / executing. | [A] |
| `NR` | **Niyyah Register** | R/W, privileged | Current **intention** - the deferred, committed goal that gates whether an action counts as chosen. The book's *Niyyah*; see the `T` extension. | [B] |
| `FR` | **Fitrah Register** | **Read-only** | Models the *Fitrah*, the original disposition on which every child is born (Sahih al-Bukhari 1385). Cannot be overwritten, only overlaid by learned state. DNA is modelled separately, at L2, as biology. | [B] |
| `DA` `5HT` `NE` `ACh` | **Neuromodulator mode registers** | Set by `MODUL` | Chemical "modes" that change what the same circuit computes. Defined in the [`N` extension](06-neural-network-extension.md). | [A/B] |
| `PL` | **Privilege Level** | R, set by mode switch | Current privilege mode (see §4). | [A] |
| `QC` | **Qarin Channel** | R (proposed) | Interface register for the attached observer (*Qarin*); a proposed read side-channel. | **[C]** |
| `HZ` | **Hazard/Conscience flag** | R/W | Sets when a pending action conflicts with `NR`/`FR` - the felt warning before wrongdoing. | [B] |

> `FR` being **read-only** is a deliberate design choice. It models a religious
> teaching, not a laboratory measurement: the *Fitrah* is overlaid by learning but never
> erased, and the door of repentance stays open (Quran 39:53). That is why the
> specification models *return* (repentance, *fitrah* re-exposure) as always possible.

---

## 3. Memory model

| Tier | Silicon analogue | HISA | Class |
|------|------------------|------|-------|
| Registers | Register file | `W0` to `W6` working memory (~4 to 7 items) | [A] |
| L1 cache | Fast cache | Short-term / phonological & visuospatial buffers | [A] |
| Main memory | DRAM | **Long-term memory** - effectively unbounded, **content-addressable** (associative), not address-indexed | [A] |
| Persistent store | Disk | Consolidated memory after sleep-dependent consolidation | [A] |

Key differences from a von Neumann machine, all **[A]**:

1. **No memory/processing separation.** Storage and computation are the *same* synapses;
   there is no von Neumann bottleneck.
2. **Content-addressable.** Recall is by association (a smell retrieves a memory), not by
   numeric address.
3. **Write requires consolidation.** A durable write is not instantaneous; it needs
   rehearsal and sleep (systems consolidation). "Saving" is a background process.
4. **Lossy, reconstructive reads.** Recall rebuilds rather than copies; each read can
   alter the stored value (reconsolidation).

---

## 4. Privilege modes

Like PDP-11 kernel/user modes and the RISC-V machine/supervisor/user hierarchy, HISA
defines privilege levels. Higher levels can override lower ones.

| Level | Name | Controls | Analogue |
|-------|------|----------|----------|
| `PL0` | **Autonomic** | Involuntary regulation (heartbeat, breathing, reflex) | Firmware / real-time kernel |
| `PL1` | **Habitual** | Overlearned automatic routines (skills compiled to reflex) | Cached microcode |
| `PL2` | **Deliberative** | Conscious reasoning and choice; can set `NR` | Supervisor |
| `PL3` | **Volitional / Nafs** | The will that can override habit and appetite; the seat of the *Amanah* | Highest supervisor / root | 

> The book's moral drama is, in this model, a **privilege contest**: appetite and habit
> (`PL1`) attempting to execute actions that the volitional level (`PL3`) would refuse,
> with the `HZ` conscience flag raised in between. "Self-control" is `PL3` retaining
> control of the `AP`. The proposed adversarial `Q` channel targets exactly this contest.

---

Next: [instruction formats »](02-instruction-formats.md)
