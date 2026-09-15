# Overview — what this paper is

> Companion notes for the IEEE short paper in `paper/`. Keep claims here honest; the TeX should not invent results that are not in this folder.

## One-sentence thesis

The LLM Jacobian lens transfers to encoder–decoder speech-to-text **if and only if it is fitted on the decoder residual stream**, position-resolved and causally averaged; pooled encoder transports are the wrong object.

## What the paper is (and is not)

**Is:** a methods + systems short paper.

- Method: decoder-only J-lens for Whisper, matching Gurnee et al. (2026).
- Negative result: why a pooled encoder→decoder transport fails.
- System: the lens ships in ECHO, an audio interpretability workbench.
- Qualitative readings of the `(position × layer)` grid.

**Is not:** a large-scale empirical study. Do not report numeric WER, probe accuracies, or “workspace band” curves unless a measured table is added later. Current findings are **observational** from the ECHO lab.

## Contributions (use this wording)

1. **Decoder-only construction.** One Jacobian \(J_\ell\) per decoder layer, \(J_\ell = \mathbb{E}_{t,\,t'\ge t}[\partial h_{\mathrm{final},t'}/\partial h_{\ell,t}]\), estimated with Hutchinson VJPs. Encoder runs under `no_grad` and is never lensed.
2. **Failure analysis of encoder transports.** Mean-pooling before differentiation erases \((t,t')\) structure, linearizes around clip means while being applied to local buckets, and drops the readout onto a space that is not the model's logit scale.
3. **Same-space readout.** \(\mathrm{lens}(h)=\mathrm{softmax}(E\,J_\ell h)\) with the frozen unembedding \(E\). Positions are decoder tokens, not audio-time buckets.
4. **ECHO integration.** Fit/apply jobs, a `(position × layer)` visualization, and tests against a linear-decoder closed form. Whisper-base has **six** lens layers (decoder blocks), not seven.

## Scope of the implementation

| Item | Status |
|---|---|
| Substrate | Hugging Face seq2seq ASR (Whisper family) |
| Default model | `whisper-base` (6 decoder blocks, \(d=512\)) |
| CTC / wav2vec2 | Excluded — no decoder to lens |
| Write / steer / ablate | Not implemented |
| Audio-frame alignment | Out of scope (use cross-attention) |

## Source of truth in the code repo

- `ECHO-LIT/Backend/app/services/jacobian_lens_service.py`
- `ECHO-LIT/JACOBIAN_LENS.md` — full technical reference
- `ECHO-LIT/JACOBIAN_LENS_INSIGHTS.md` — how to read a grid
- Branch that landed the decoder-only design: `jlense-decorderonly-test` (merged to `main` as PR #9)

## Suggested title

**A Decoder-Only Jacobian Lens for Speech-to-Text Models**
