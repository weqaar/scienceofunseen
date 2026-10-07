# Changelog

## v0.2

**New: the `N` (Neural Network) extension** ([`spec/06`](spec/06-neural-network-extension.md)).
The human nervous system is not a static ISA. It is a neural network that rewires itself,
and most human capabilities are learned rather than built in. The extension:

- splits the nervous system into three tiers: a largely fixed substrate ISA, a
  network whose behaviour is changed by neuromodulators, and learned functions;
- adds substrate operations (`INTEG`, `SPIKE`, `XMIT`, `STDP`, `SPROUT`, `PRUNE`, `GLIA`,
  `SYNC`), neuromodulator mode registers (`DA`, `5HT`, `NE`, `ACh`) with `MODUL`, `RPE`,
  `PREDICT` and `PERR`, and learned-function operations (`TRAIN`, `RECYCLE`, `CPG`);
- covers the nervous system beyond the brain (spinal cord, enteric nervous system,
  circadian clock);
- compares the brain with neural-network accelerators that do have ISAs (TPU, Cambricon);
- states where the ISA picture stops working.

A worked example, [`examples/learning-to-read.md`](examples/learning-to-read.md), follows the
instruction set growing through training. The other documents now cross-link to `N`, and
the extensions in `spec/04` are described as learned functions running on it.

**Review corrections**

- Aligned with the current book, *A Mathematical and Scientific Approach to the Unseen*:
  title and chapter updated; HISA is presented as an analogy [B]; DNA and the *Fitrah*
  are treated as different kinds of claim; the NP-hard prediction claim is withdrawn, as
  the book states that a person is not a precisely defined computational task; the
  read-only `FR` and always-available `REPENT` are described as modelling a religious
  teaching (Quran 39:53), not as an engineering guarantee.
- Corrected numbers: the cerebral cortex has about 1.6 x 10^10 neurons, not 10^11
  (Azevedo et al., 2009).
- Corrected silicon comparisons: out-of-order cores speculate a few hundred instructions
  ahead, and self-modifying code is avoided rather than forbidden. "No global clock" is
  refined: there is no single clock, but local rhythms and a circadian master clock.
- Removed references to the "piety factor", which the book no longer uses.
- Reference note: real repository URL, current chapter, and labels matching the book.
- Wording made plainer in a few places.

## v0.1

Initial draft for public comment.

**Repository clean-up.**

- Removed the repository-root copies of five HISA files, the root `files.zip` archive
  and its nested `HISA_repo.zip`. Each was an exact copy of a file already committed
  under `HISA/`, so nothing is lost.
- Replaced the root `README.md` (a copy whose links broke at the root) with a short
  landing page that points to `HISA/` and states which licence covers which folder.
- Stated that `HISA/LICENSE` (CC BY 4.0 and MIT) applies to the whole `HISA/` folder
  in place of the root GPL v3 licence.
- Wrote numeric ranges with "to" instead of a dash.
