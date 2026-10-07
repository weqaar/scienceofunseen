# Example - A Decision Under Temptation, in HISA

A worked "program" demonstrating how the instructions, registers, and privilege levels compose.
The scenario: an opportunity for unlawful gain appears (e.g. an easy, untraceable act of
*riba* or taking a withheld right). This is deliberately chosen because the book treats
wealth taken unlawfully as a serious wrong (Quran 2:279).

```asm
; --- Stimulus arrives on the perception pipeline ---
SENSE   S1, opportunity        ; visual/situational input transduced (Layer 3)
ATTN    opportunity            ; AP <- opportunity  (attention locks on)
PARSE   W0, S1                 ; interpret: "easy gain, low risk of being caught"
APPRAISE E, W0                 ; affective appraisal: E0 valence rises (desire)

; --- Habitual/appetitive path tries to run at PL1 ---
RECALL  W1, similar_past       ; content-addressable: past times this felt good
MODULATE deliberate            ; desire raises gain on the 'take it' candidate

; --- Adversarial injection (Q extension, [C]) ---
WHISPER take_it                ; candidate instruction pushed into decode queue (waswas)

; --- The privilege contest ---
DED     W2, {riba = harb}      ; reasoning: recalls Quran 2:279 (a matter of war)
HZ <- set                      ; conscience flag raised: pending act conflicts with FR/NR
AUTH    take_it                ; require PL3 + NR authorisation before commit
        ; PL3 (Nafs) evaluates against NR (intention to remain within the law of the Deen)

; --- Two possible commits ---
; Path A - the machine keeps PL3 in authority:
FLUSH                          ; clear the injected candidate (turn away from waswas)
INTEND  NR, remain_lawful      ; reaffirm intention
OVERRIDE take_it               ; PL3 overrules the PL1 appetite
        ; result: action refused; FR preserved

; --- OR ---

; Path B - repeated small concessions have lowered the AUTH threshold:
; (no FLUSH; AUTH passes because PL3 threshold was eroded by habituation)
ACT     take_it                ; the unlawful act commits at PL1
SYN     habit, take_it, +1     ; the choice writes weight: next time is easier
        ; result: corrupt overlay grows

; --- Recovery is always available (design guarantee) ---
REPENT                         ; discard corrupt overlay; re-expose read-only FR (Fitrah)
        ; radd al-mazalim: return the wrongfully taken right
```

**What the example demonstrates:**

1. **`WHISPER` cannot force `ACT`.** An injected candidate must still pass `AUTH`. The
   adversary's real strategy is the slow lowering of the `AUTH` threshold (Path B's
   premise), not direct control.
2. **`SYN ..., +1`** explains why sin compounds: every commit rewrites the weights that
   decode the next temptation, making the corrupt path faster (the toolchain's optimiser
   working against you).
3. **`REPENT` is always reachable** because `FR` is read-only - the model's way of
   stating the teaching that return is possible no matter how large the overlay (Quran
   39:53).
4. The whole moral event is expressible as an **access-control problem**: who holds the
   `AP`, and whether `PL3` retains authority over `PL1`.
