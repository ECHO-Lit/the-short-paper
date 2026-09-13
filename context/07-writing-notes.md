# Writing notes

## Voice

IEEE conference short paper, ~4 pages plus references. Methods + systems. Hedged qualitative claims. No invented metrics.

## Author block

Draft lists the ECHO-Lit group at the University of Moratuwa. Edit names and emails before submission. Corresponding author in the current draft: Januda Lelwala.

## Claims that are allowed

- The implementation matches the LLM J-lens *construction* (same-space, causal average, unembed with \(E\)).
- Encoder-pooling is the wrong object, for the reasons in `03-method.md`.
- CTC is excluded because there is no decoder residual to unembed.
- The ECHO UI exposes a `(position × layer)` top-\(k\) grid.
- Unit tests recover a linear closed form.
- Qualitative patterns listed in `04-qualitative-readings.md` have been observed in the lab.

## Claims that are not allowed without new experiments

- A measured workspace band (layer-wise top-1 agreement curve).
- Causal mediation, steering, or ablation results.
- “Whisper has a global workspace” as a scientific finding (that is Gurnee et al.'s claim about LLMs).
- Numerical superiority over logit lens / tuned lens / DecoderLens.
- Any WER, latency, or probe-accuracy table.

## Notation in the TeX

- \(h_{\ell,t}\) decoder residual at layer \(\ell\), position \(t\)
- \(h_{\mathrm{final},t'}\) `last_hidden_state`
- \(E\) tied output projection
- \(J_\ell\) fitted map
- \(T\) decoder length, \(d\) width, \(L\) lens-layer count (7 on base)

## Build

```bash
cd paper && make
```

## Venue (undecided)

The class is `IEEEtran` conference, which is a reasonable ICASSP / Interspeech / IEEE SLT draft. ACL short (4 pages) would need a style swap; the text should still fit.
