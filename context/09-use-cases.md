# Practical use — what the grid is for

The paper section ``What the analysis provides'' is the source of truth. Keep these as *procedures*, not results.

## The object

One apply job → `(position × layer)` top-\(k\) rankings on the model's logit scale.

- Not a second transcript
- Not calibrated WER confidence
- Not audio-frame alignment (that is cross-attention)

## Complements in ECHO

| Tool | Question |
|---|---|
| Attention | Where did this decoder position look in the 1500 encoder frames? |
| Saliency | Which waveform region moved a scalar? |
| J-lens | What is the decoder poised to *say* at this depth? |

## Workflows (copy into demos)

1. **Substitution audit.** Generated transcript wrote B, human heard A. Read the column at B. A never in top-\(k\) → listening/alignment issue. A in mid layers, B only at the top → prior overrode evidence.
2. **Silence loop.** Scan cells before the first repeat. Loop tokens ranked in mid blocks while the input token is unrelated → precursor.
3. **High-stakes token.** Sort positions by top-\(k\) gap. Wide cells first for a reviewer (names, numbers, homophones).
4. **Robustness.** Same reference transcript, clean vs perturbed clip. Did crystallization depth move? Did a ranking flip?
5. **Generated vs gold.** Two apply jobs. Column disagreement → free-run vs teacher-forced discrepancy.
6. **Probe targeting.** Cheap apply; probe the row where top-1 first matches.

Do not report false-positive rates or workspace-band curves until they exist.
