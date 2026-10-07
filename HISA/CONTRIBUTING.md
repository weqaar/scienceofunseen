# Contributing to HISA

HISA is built in the open, by the same method as the science of Hadith and modern
open-source software: many independent contributors, each auditing the others, every
claim traceable to its source.

## Ground rules

1. **Tag every non-trivial claim** with a class:
   - `[A]` established (cite peer-reviewed science or standard engineering),
   - `[B]` model/mapping (a defensible structural analogy),
   - `[C]` speculative (internally consistent, awaiting proof).
   Mislabelling `[C]` as `[A]` is the one thing this project treats as a defect.
2. **Cite.** Any `[A]` claim needs a real, verifiable reference.
3. **Keep the ISA-manual voice.** Instructions are listed with mnemonic, operands,
   format, action, and biological/cognitive correlate.
4. **Respect the discipline of the book.** This is a serious model, not satire; and it is
   a *model*, not a reduction of the human to a machine. The top privilege level is, by
   design, the part no machine has.

## How to propose a change

- Open an issue describing the instruction/register/mechanism and its claim class.
- For a new extension, state its dependency and give at least one worked example in
  `examples/`.
- Pull requests should update the relevant `spec/`, `microarch/`, `toolchain/`, or `os/`
  document and cross-link.

## Open questions (good first contributions)

- Is HISA **Turing-complete**, sub-Turing, or super-Turing? Argue rigorously.
- Is the human **"truly complete"** (able to express any operation a living system could)?
  The book leaves this open on purpose - *are we?*
- Formalise the `SYN`-scheduling optimiser against known results in learning theory.
- Extend the [`N` extension](spec/06-neural-network-extension.md): which further
  operations does the nervous system learn rather than inherit, and how would you test
  whether a proposed operation is substrate (Tier 1) or learned (Tier 3)?
- Propose testable predictions for any `[C]` claim to move it toward `[B]` or `[A]`.
