# HISA Operating System - The Layers That Manage the Machine

> An ISA needs an operating system to allocate its resources, schedule its work, isolate
> its processes, and defend it. The human machine runs one continuously from before birth
> until death. This document specifies it in OS terms: kernel, scheduler, memory manager,
> device drivers, privilege/protection, and security. It corresponds to **Layers 6 (OS)
> and 7 (the supervising will)** of the book's stack.

---

## 1. The kernel - autonomic core (`PL0`)  [A]

The always-resident, real-time kernel: cardiac rhythm, respiration, blood chemistry,
reflexes, thermoregulation. It runs at the highest reliability and the lowest latency, is
never swapped out, and requires no conscious process. In the book's stack this is the
firmware/brainstem layer. It cannot, by design, be halted by user processes - you cannot
*decide* to stop your heartbeat - which is a safety property, not a limitation.

## 2. The scheduler - attention (`AP`)  [A/B]

The scarcest resource in the machine is **conscious working memory** (`W0` to `W6`, ~4 to 7
slots). The scheduler that allocates it is **attention**, and the `AP` register is its
run-pointer.

- **Single foreground context.** Genuine parallel *conscious* execution is not supported;
  what feels like multitasking is fast **context switching**, and each switch has a cost
  (switch latency; Monsell, 2003).
- **Interrupts.** Salient stimuli (a loud noise, one's own name, pain) raise hardware
  interrupts that pre-empt the scheduler - an evolved safety feature, and an attack
  surface (why notifications and outrage hijack attention).
- **Priority inversion.** Low-value but loud processes can starve high-value quiet ones -
  the computational description of a distracted life.

> The book's practices that "discipline attention" (*khushu'* in prayer, *dhikr*) are, in
> OS terms, **reclaiming the scheduler** from interrupt-driven chaos toward intended
> priority (`NR`).

## 3. Memory management - consolidation and forgetting  [A]

- **Allocation:** encoding (`ENC`) writes new traces.
- **Paging to durable store:** consolidation (`CONS`) during sleep moves working traces to
  long-term storage.
- **Garbage collection:** **forgetting is a feature.** Active pruning of unused traces
  keeps recall tractable; a machine that forgot nothing would be unable to generalise
  (cf. the case of "S." in Luria's *Mind of a Mnemonist*).
- **Cache coherence problems:** false memories arise because `RECALL`+`RECON` rebuild
  rather than copy - reads can corrupt writes.

## 4. Device drivers - the sensory/motor interfaces  [A]

The senses (`S0` to `S5`) and actuators (`M0` to `M2`) are the device layer (book Layer 3). The
OS calibrates them continuously (e.g. adapting to lighting, to a shifted centre of
gravity) - driver auto-tuning. Sensory illusions are driver bugs exposed by inputs outside
the calibrated range.

## 5. Privilege and protection  [A/B]

The privilege ladder (`PL0` to `PL3`, see [registers](../spec/01-registers-and-state.md)) is
the protection model:

| Level | Role | Can override |
|-------|------|--------------|
| `PL0` Autonomic | Vital regulation | - (protected) |
| `PL1` Habitual | Compiled skills, appetites | lower |
| `PL2` Deliberative | Conscious reasoning | `PL1` |
| `PL3` Volitional (Nafs) | The will; sets/holds `NR` | all above |

**Self-control is `PL3` retaining the scheduler.** Loss of control (addiction, rage) is a
**privilege-escalation exploit**: a `PL1` appetite executing actions that `PL3` would
refuse. The book's entire moral model is this contest, and its practices are hardening
techniques for keeping `PL3` in authority.

## 6. Security - the spiritual firewall  [B, with C for the threat model]

The OS's security subsystem defends against the fault model in
[`microarch/pipeline.md`](../microarch/pipeline.md).

| Threat (from the `Q` extension) | Countermeasure instruction | Book's practice |
|---------------------------------|----------------------------|-----------------|
| `WHISPER` (injected candidate, *waswas*) | `FLUSH` + `AUTH` | Turning away; seeking refuge (*isti'adhah*) |
| Threshold lowered by habituation | Raise `AUTH` threshold | Consistency of small good deeds |
| `TUNE` (proposed external control signal) [C] | `SHIELD` | Morning/evening *adhkar*; *Ruqyah* |
| Corrupt overlay accumulation | `REPENT` (clean rebuild) | *Tawbah*; *radd al-mazalim* |

**Defence-in-depth principle:** no single practice is the firewall; layered, habitual,
low-cost defences (analogous to defence-in-depth in security engineering) keep the `AUTH`
threshold high enough that injected instructions fail to commit. This is the OS-level
statement of the book's claim that daily practice, not occasional heroics, is what
protects the machine.

## 7. Boot and shutdown  [A/B]

- **Boot:** firmware (`FR`/DNA) initialises `PL0`; drivers (`S`) come online over months
  (critical periods; Hubel & Wiesel, 1970); the OS (`L6`) is compiled over years from
  environmental input.
- **Shutdown:** `HALT` at the organism scale - the one process this specification does not
  model past its edge, and the one condition (death) the book names as the single disease
  without a cure.

---

← [Back to overview](../spec/00-overview.md) · [The book's reference note »](../book/reference-note.md)
