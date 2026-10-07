# Example - Learning to Read, in HISA

A worked "program" for the [`N` (Neural Network) extension](../spec/06-neural-network-extension.md).
It follows a child learning to read, because reading is the clearest case of a human
**installing a new capability in existing hardware**. No gene codes for reading; writing
is too recent for that. The network retrains part of its visual system instead
(Dehaene & Cohen, 2007).

```asm
; --- Before reading: the visual network recognises objects and faces ---
VLOAD   v0, S1               ; a page arrives as a wide visual field (V extension)
VMATCH  v1, v0, shapes       ; the network sees marks and shapes, not letters
        ; there is no READ instruction yet: the capability does not exist

; --- Teaching begins: a letter is named, again and again ---
ATTN    letter_b             ; AP <- the letter on the page
SYMB    W0, letter_b         ; L extension: bind the shape to the sound /b/
MODUL   ACh, high            ; attention raises the learning rate (Tier 2 mode register)
TRAIN   ventral_visual, W0   ; repetition drives STDP, SPROUT, PRUNE in the visual network

; --- Feedback: a correct answer is rewarded ---
RPE     W1, praised, expected ; outcome better than expected -> positive error
MODUL   DA, W1               ; dopamine signal strengthens what was just done
STDP    letter_map, S1, +dt   ; connections that fired together are reinforced

; --- Months later: the network has been recycled ---
RECYCLE word_form_area, ventral_visual
        ; part of the object-recognition network now responds to written words
        ; result: a new learned function exists (Tier 3)

; --- After reading is learned ---
VLOAD   v0, S1               ; the same page arrives
VMATCH  v1, v0, words        ; words are recognised in parallel, without effort
PARSE   W2, v1               ; L extension: meaning is extracted
        ; the reader cannot now look at a word and NOT read it:
        ; the learned function runs automatically (compiled to PL1)
```

**What the example demonstrates:**

1. **The instruction set grew.** Before training, the machine had no way to read. After
   training, it does. A silicon processor cannot add an opcode to itself; the human
   machine does it routinely. This is what "the ISA is plastic" means.
2. **No new hardware was needed.** `RECYCLE` retrained an existing network. That is why
   cultural skills that appeared only recently in history can be learned by every child.
3. **Chemistry set the pace.** `MODUL ACh` and `MODUL DA` did not carry the lesson; they
   changed how fast and in which direction the network learned it (Tier 2).
4. **Once learned, it runs on its own.** You cannot see a word in your own language and
   choose not to read it. The learned function has moved from effortful (`PL2`) to
   automatic (`PL1`), as described in the [toolchain](../toolchain/compiler.md).
5. **The same process explains habits.** Replace "letter" with any repeated action, and
   "praise" with any reward, and the same program describes how a habit forms, good or
   harmful. The [decision-under-temptation example](decision-under-temptation.md) covers
   the other side: how the will (`PL3`) can still override what training has made easy.
