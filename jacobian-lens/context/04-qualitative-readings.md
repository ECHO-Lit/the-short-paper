# Qualitative readings of the grid

These are a **reading protocol**, not measured results. Do not write “we find” or “we recover” unless a figure from a real apply job is in the paper.

Layer index \(0\) is the first decoder **block** (UI “Layer 1”). On whisper-base there are **six** lens layers. The final pre-logit state is the fit *target* and is never a readout layer.

A column = one position, all layers (depth trajectory). A row = one layer, all positions.

## 1. Crystallization depth

Before a content word, early layers rank frequent tokens; mid layers surface syntactic candidates; upper layers converge on the spoken word. The row of convergence is where acoustic evidence is integrated into the verbal decision. A column that never converges marks transcription against the model's internal preference.

## 2. SOT is an audio-only workspace readout

At `<|startoftranscript|>` the decoder has consumed no transcript tokens. Everything in the residual arrived through cross-attention. Top tokens are an acoustic gist of the clip (likely first words, Whisper's silence convention `"you"`, or background-speech artifacts).

## 3. Language-model prior vs. acoustic evidence

Compare mid vs. late layers at the same position.

- Agreement on a collocation \(\Rightarrow\) prior-driven.
- Mid layers generic, top rows flip to an unusual word \(\Rightarrow\) audio-driven.

## 4. Hallucination / repetition precursors

Under silence, Whisper's repetition loop is often visible in mid-layer rankings **before** generation commits. The grid is a leading indicator.

## 5. Homophone competition

Top-5 at ambiguous audio shows the contest (`their` / `there` / `they're`). Wide spread \(\Rightarrow\) under-determined acoustics; narrow spread \(\Rightarrow\) acoustics decided.

## 6. Endpoint planning

Near the end of speech, readouts drift toward `EOT`, `.`, or `"thank you"`. Premature stop-direction in upper layers while speech continues is an early-cutoff signature.

## 7. Workspace-range (hypothesis, not a plotted result)

By analogy with Gurnee et al.: early-layer readouts are noise; a mid-upper band is verbalizable; the last step is closer to “motor” output selection. Confirming this with a top-1 agreement curve across a corpus is **future work**.

## Interpretation contract (copy into the paper)

- `score` is a first-order logit; `probability` is display softmax, not calibrated confidence.
- Top-\(k\) is a ranking of verbalizable content, not a phrase.
- The lens does not name the supporting audio frame.
- \(J_\ell\) is an average over contexts: a cell is what the decoder is *disposed* to say, not a guaranteed continuation.
