# HISA Spec 06 - The `N` (Neural Network) Extension

> A silicon processor has a **static** instruction set: the operations are fixed when the
> chip is manufactured, and software can only arrange them in new orders. The human
> nervous system does not work like that. It is a **neural network** that keeps
> rewiring itself, and most of what it can do was **learned**, not built in. This
> document adds that fact to the specification. It explains which parts of the human
> machine behave like a fixed ISA, which parts behave like a trained network, and where
> the ISA picture stops being useful.

---

## 1. Why a static ISA is not enough  [A]

You were not born able to read. Nothing in your genes contains a "read" instruction,
and writing was invented only a few thousand years ago, far too recently for evolution to
have added hardware for it. Yet you are reading this sentence quickly and without
effort. Learning to read did not give you a new brain region. It retrained part of the
visual system that originally recognises objects and faces until it recognised letters
and words (Dehaene & Cohen, 2007).

On a silicon chip that would be impossible: software cannot add an opcode to the
processor. In the human machine it happens all the time. Reading, arithmetic, driving,
playing an instrument, and memorising a long text are all capabilities that the network
**installs in itself** through practice.

So the human ISA has two kinds of operation:

| Kind | Fixed or learned? | Example | Where specified |
|------|-------------------|---------|-----------------|
| **Substrate operations** | Largely fixed by biology; the same in every person and stable across life | A neuron fires; a synapse strengthens | LISA ([03](03-base-instruction-set-LISA.md)) and §3 below |
| **Learned operations** | Built by training the network; different in every person and still changing | Reading, a skill, a habit, a way of reasoning | The extensions in [04](04-extensions-human.md), read as learned programs (§5) |

---

## 2. Three tiers  [B]

The `N` extension describes the nervous system as three tiers stacked on one another.

```
┌──────────────────────────────────────────────────────────────┐
│ Tier 3  LEARNED FUNCTIONS   reading, language, skills, habits│  ← different in each person
├──────────────────────────────────────────────────────────────┤
│ Tier 2  MODULATED NETWORK   same wiring, different behaviour │  ← set by chemical "mode"
│                             depending on neuromodulators     │
├──────────────────────────────────────────────────────────────┤
│ Tier 1  SUBSTRATE OPS       spike, integrate, transmit,      │  ← close to a fixed ISA
│                             strengthen, grow, prune          │
└──────────────────────────────────────────────────────────────┘
```

- **Tier 1** is the closest thing the brain has to a fixed instruction set. It is the
  small set of physical operations every neuron and synapse can perform.
- **Tier 2** is unusual: chemical signals change *what the same wiring computes*. A
  silicon chip has nothing quite like it.
- **Tier 3** is everything a person has learned. It is stored in connection strengths
  and wiring, not in opcodes.

---

## 3. Tier 1 - substrate operations  [A]

These refine the LISA signalling instructions `FIRE` and `SYN`
([03 §3](03-base-instruction-set-LISA.md)), which are summaries of several distinct
biological operations.

| Mnemonic | Operands | Action | Biological correlate | Class |
|----------|----------|--------|----------------------|-------|
| `INTEG`  | `rd, vs1` | Combine thousands of synaptic inputs `vs1` into the membrane state `rd`, non-linearly, in the dendrites | Dendritic integration (London & Häusser, 2005) | [A] |
| `SPIKE`  | `rs1` | Emit an all-or-none action potential when the state of `rs1` crosses threshold | Action potential (Hodgkin & Huxley, 1952) | [A] |
| `XMIT`   | `rd, rs1` | Release transmitter from `rs1` onto `rd`; delivery is **probabilistic**, not certain | Synaptic transmission | [A] |
| `STDP`   | `rd, rs1, Δt` | Strengthen the connection if `rs1` fired just before `rd`; weaken it if just after | Spike-timing-dependent plasticity (Markram et al., 1997; Bi & Poo, 1998) | [A] |
| `SPROUT` | `rd, rs1` | Grow a new connection from `rs1` to `rd` | Structural plasticity (Holtmaat & Svoboda, 2009) | [A] |
| `PRUNE`  | `rs1` | Remove an unused connection | Synaptic pruning in development (Huttenlocher & Dabholkar, 1997) | [A] |
| `GLIA`   | `rs1, imm` | A support cell adjusts the synapse `rs1` it surrounds | The "tripartite synapse" (Araque et al., 1999) | [A] |
| `SYNC`   | `vs1, imm` | Lock a population `vs1` to a rhythm in frequency band `imm` | Neuronal oscillations (Buzsáki & Draguhn, 2004) | [A] |

**How big is one unit?** Much bigger than an artificial neuron. When researchers trained
an artificial deep network to copy the input-output behaviour of a single cortical
neuron, it needed five to eight layers to do so (Beniaguev, Segev & London, 2021). One
biological "lane" in the [`V` extension](05-vector-extension.md) is therefore a small
network in its own right.

**Scale.** The human brain has about 86 billion neurons: about 16 billion in the
cerebral cortex and about 69 billion in the cerebellum (Azevedo et al., 2009). The
neocortex alone has on the order of 10¹⁴ synapses (Pakkenberg et al., 2003).

---

## 4. Tier 2 - the modulated network  [A, with B for the mapping]

On a silicon chip an `ADD` always adds. In a nervous system the same circuit, with the
same wiring, can produce **different behaviour** depending on which neuromodulators are
present. Studies of small, fully mapped circuits found that changing the chemical
environment reconfigures what the circuit does without changing a single connection
(Marder, 2012). In ISA terms, neuromodulators are **mode registers** that change the
meaning of the instructions that follow.

| Reg | Neuromodulator | Proposed role in learning and choice | Class |
|-----|----------------|--------------------------------------|-------|
| `DA` | Dopamine | Signals **reward prediction error**: the gap between what was expected and what happened | [A] (Schultz, Dayan & Montague, 1997) |
| `5HT` | Serotonin | Sets how far ahead rewards are weighed (patience against impulse) | [B] (Doya, 2002) |
| `NE` | Noradrenaline | Sets how widely the machine explores rather than repeats the familiar choice | [B] (Doya, 2002) |
| `ACh` | Acetylcholine | Sets the learning rate: how quickly new input overwrites old memory | [B] (Doya, 2002) |

| Mnemonic | Operands | Action | Class |
|----------|----------|--------|-------|
| `MODUL`  | `mN, imm` | Set mode register `mN` (one of `DA`, `5HT`, `NE`, `ACh`) to level `imm`; changes gain and plasticity across many circuits at once | [A] |
| `RPE`    | `rd, rs1, rs2` | Compute the difference between outcome `rs1` and expectation `rs2` into `rd`; drives `STDP` toward what turned out better than expected | [A] |
| `PREDICT`| `rd, rs1` | Generate the expected next input `rd` from the current model `rs1` | [B] (Rao & Ballard, 1999) |
| `PERR`   | `rd, rs1, rs2` | Pass upward only the part of input `rs1` that prediction `rs2` did not explain | [B] (Rao & Ballard, 1999) |

> **Why this matters to the book's argument.** Sleep, pain, stress, illness, medication,
> and addiction change these mode registers. They do not remove a person's ability to
> choose, but they change how hard a choice is. This is the ISA-level form of the book's
> statement that "difficulty differs from impossibility, and influence differs from
> compulsion."

---

## 5. Tier 3 - learned functions  [B]

Everything in [04](04-extensions-human.md) (language, memory, reasoning, self-reference,
time-binding, volition, affect) is better read as **learned programs running on Tiers 1
and 2** than as hardwired opcodes. Each person's set is different, and each is still
being trained.

| Mnemonic | Operands | Action | Correlate | Class |
|----------|----------|--------|-----------|-------|
| `TRAIN`  | `rd, rs1` | Repeat experience `rs1` so that `STDP`, `SPROUT` and `PRUNE` reshape network `rd` | Practice; learning | [A] |
| `RECYCLE`| `rd, rs1` | Retrain an existing network `rs1` for a new cultural skill `rd` | Neuronal recycling: reading, arithmetic (Dehaene & Cohen, 2007) | [A] |
| `CPG`    | `rs1` | Run a rhythmic motor pattern locally in the spinal cord or brainstem, without the brain issuing each step | Central pattern generators (Grillner, 2006) | [A] |

**The ISA is plastic.** This is the main change `N` makes to HISA: the human machine's
instruction set is not closed. A new capability is added by `TRAIN` or `RECYCLE`, and an
old one fades if it is not used. A silicon ISA is versioned by its manufacturer; the
human one is versioned by its owner's daily life.

---

## 6. The nervous system is not only the brain  [A]

A specification that stopped at the skull would miss much of the machine:

| Subsystem | What it does | Reference |
|-----------|--------------|-----------|
| Spinal cord | Runs reflexes and rhythmic patterns (walking, breathing) locally | Grillner, 2006 |
| Enteric nervous system | A network of several hundred million neurons in the gut wall that can control digestion on its own | Furness, 2012 |
| Autonomic nervous system | Regulates heart, breathing, and glands (`PL0` in the [OS](../os/operating-system.md)) | |
| Circadian clock | A master clock in the hypothalamus (the suprachiasmatic nucleus) keeps daily time and synchronises clocks in the body's organs | Mohawk, Green & Takahashi, 2012 |

The human machine is therefore a **distributed** system with several semi-independent
controllers, not a single central processor.

---

## 7. Does a neural network have an ISA?  [A for silicon, B for the comparison]

Yes, in silicon. Chips built to run artificial neural networks have published
instruction sets. Google's first Tensor Processing Unit used about a dozen high-level
instructions such as reading weights, matrix multiply, and applying an activation
function (Jouppi et al., 2017). Cambricon was proposed as an ISA designed specifically
for neural-network work (Liu et al., 2016).

But notice where the *intelligence* lives in those systems. The accelerator's ISA is
small and fixed. What the network actually does (recognise a face, translate a sentence)
lives in its **trained weights**, not in its opcodes. The same is true of the brain, with
two differences that go beyond any current artificial network:

1. **The hardware rewires itself.** An artificial network changes its weights during
   training, but its wiring diagram is fixed by the programmer. A brain grows and removes
   connections (`SPROUT`, `PRUNE`) throughout life.
2. **The rules of learning are themselves modulated.** Neuromodulators (§4) change how
   fast and in what direction the network learns, moment by moment.

So the answer the biology supports to "does the brain have an ISA?" is: it has a small,
mostly fixed **substrate ISA** (Tier 1), and on top of it a large, personal, constantly
retrained **learned function** (Tier 3) that no fixed instruction list can capture.

---

## 8. Where the ISA picture stops working  [B]

The comparison is useful, and it has limits. State them before relying on it:

- **There is no opcode decoder.** A processor reads an instruction and selects a circuit.
  A brain has no central place where "instructions" are read; computation is spread
  across populations of neurons.
- **Instructions and data are not separate.** The same synapses store what is known and
  perform the computation on it.
- **One function, many circuits; one circuit, many functions.** Different structures can
  produce the same result, and the same structure can serve different results. Biologists
  call this *degeneracy* (Edelman & Gally, 2001). A silicon ISA has a fixed map from opcode
  to circuit; the brain does not.
- **Execution is noisy.** Transmission at a synapse is probabilistic (`XMIT`). The
  machine computes reliably *in spite of* unreliable parts, by using many of them.
- **A person is more than the network.** The book that this repository accompanies
  states that "a human being remains more than a machine executing instructions." The
  `N` extension describes the biological machinery a person uses. It makes no claim to
  describe the person, the soul (*ruh*), or the will that holds responsibility
  (`PL3`, the *Amanah*).

---

## References

- Araque, A., Parpura, V., Sanzgiri, R. P. & Haydon, P. G. (1999). Tripartite synapses:
  glia, the unacknowledged partner. *Trends in Neurosciences*, 22(5), 208-215.
- Azevedo, F. A. C. et al. (2009). Equal numbers of neuronal and nonneuronal cells make
  the human brain an isometrically scaled-up primate brain. *Journal of Comparative
  Neurology*, 513(5), 532-541.
- Beniaguev, D., Segev, I. & London, M. (2021). Single cortical neurons as deep
  artificial neural networks. *Neuron*, 109(17), 2727-2739.
- Bi, G. & Poo, M. (1998). Synaptic modifications in cultured hippocampal neurons.
  *Journal of Neuroscience*, 18(24), 10464-10472.
- Buzsáki, G. & Draguhn, A. (2004). Neuronal oscillations in cortical networks.
  *Science*, 304(5679), 1926-1929.
- Dehaene, S. & Cohen, L. (2007). Cultural recycling of cortical maps. *Neuron*, 56(2),
  384-398.
- Doya, K. (2002). Metalearning and neuromodulation. *Neural Networks*, 15(4-6), 495-506.
- Edelman, G. M. & Gally, J. A. (2001). Degeneracy and complexity in biological systems.
  *PNAS*, 98(24), 13763-13768.
- Furness, J. B. (2012). The enteric nervous system and neurogastroenterology. *Nature
  Reviews Gastroenterology & Hepatology*, 9(5), 286-294.
- Grillner, S. (2006). Biological pattern generation: the cellular and computational
  logic of networks in motion. *Neuron*, 52(5), 751-766.
- Hodgkin, A. L. & Huxley, A. F. (1952). A quantitative description of membrane current
  and its application to conduction and excitation in nerve. *Journal of Physiology*,
  117(4), 500-544.
- Holtmaat, A. & Svoboda, K. (2009). Experience-dependent structural synaptic plasticity
  in the mammalian brain. *Nature Reviews Neuroscience*, 10(9), 647-658.
- Huttenlocher, P. R. & Dabholkar, A. S. (1997). Regional differences in synaptogenesis in
  human cerebral cortex. *Journal of Comparative Neurology*, 387(2), 167-178.
- Jouppi, N. P. et al. (2017). In-datacenter performance analysis of a tensor processing
  unit. *Proceedings of ISCA 2017*, 1-12.
- Liu, S. et al. (2016). Cambricon: an instruction set architecture for neural networks.
  *Proceedings of ISCA 2016*, 393-405.
- London, M. & Häusser, M. (2005). Dendritic computation. *Annual Review of
  Neuroscience*, 28, 503-532.
- Marder, E. (2012). Neuromodulation of neuronal circuits: back to the future. *Neuron*,
  76(1), 1-11.
- Markram, H., Lübke, J., Frotscher, M. & Sakmann, B. (1997). Regulation of synaptic
  efficacy by coincidence of postsynaptic APs and EPSPs. *Science*, 275(5297), 213-215.
- Mohawk, J. A., Green, C. B. & Takahashi, J. S. (2012). Central and peripheral circadian
  clocks in mammals. *Annual Review of Neuroscience*, 35, 445-462.
- Pakkenberg, B. et al. (2003). Aging and the human neocortex. *Experimental
  Gerontology*, 38(1-2), 95-99.
- Rao, R. P. N. & Ballard, D. H. (1999). Predictive coding in the visual cortex.
  *Nature Neuroscience*, 2(1), 79-87.
- Schultz, W., Dayan, P. & Montague, P. R. (1997). A neural substrate of prediction and
  reward. *Science*, 275(5306), 1593-1599.

← [Vector extension](05-vector-extension.md) · [Worked example: learning to read »](../examples/learning-to-read.md)
