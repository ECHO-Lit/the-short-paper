# Method — decoder-only Jacobian lens

Notation matches `jacobian_lens_service.py` and Gurnee et al. (2026).

## Frozen vs fitted

| Object | Status |
|---|---|
| Encoder, decoder, unembedding \(E\) | pretrained, frozen |
| \(J_\ell \in \mathbb{R}^{d\times d}\) per decoder layer | fitted (estimated, not trained) |

No optimizer, no baselines, no weight updates. `torch.autograd.grad` only *measures* VJPs.

## Estimator

Teacher-forced decoder pass on a transcript of length \(T\). For layer \(\ell\), source position \(t\), future position \(t'\ge t\):

\[
J_\ell \;=\; \mathbb{E}_{t,\,t'\ge t,\,\text{transcript},\,\text{probe}}
\Bigl[\frac{\partial h_{\mathrm{final},t'}}{\partial h_{\ell,t}}\Bigr].
\]

Causal mask zeroes \(t>t'\). Averaging uses the triangular count \(T(T+1)/2\).

## Hutchinson (Rademacher) probes

For \(r\in\{\pm 1\}^d\), one backward of \(\sum_{t'}\langle h_{\mathrm{final},t'}, r\rangle\) yields, at every source \(t\) at once, \(\sum_{t'\ge t} J_\ell(t\to t')^\top r\). Accumulate \(\mathrm{outer}(r, \sum_t g_t)\) and divide by \(N_{\text{samples}}\cdot N_{\text{probes}}\cdot T(T+1)/2\).

Defaults in the product: `probe_count=4` (1–32), 2–1000 transcripts, audio truncated at 30 s (max 60). Whisper still pads Mel to 30 s.

## Apply

1. Encoder, no grad.
2. Positions from greedy `generate` or a provided transcript.
3. Teacher-forced decoder pass collects \(h_{\ell,t}\).
4. Per cell:

\[
\mathrm{logits}_{\ell,t} \;=\; E\,(J_\ell h_{\ell,t}),
\qquad
\mathrm{lens}(h_{\ell,t})=\mathrm{softmax}(\mathrm{logits}_{\ell,t}).
\]

Whisper's final LayerNorm sits inside the decoder, so it is absorbed into \(J\). No extra RMS-norm at readout.

## Artifact (format v2)

- `architecture: "decoder"`
- `method: "hutchinson-decoder-vjp"`
- `matrices`: \(L\) square maps \((d,d)\)
- Bound to `model_id` + revision; apply refuses a mismatch

## Closed-form test

For a linear decoder \(h_{\mathrm{final}} = H M\) with causal averaging, the fitted map must recover \(J=2M/(T+1)\). Apply is checked against an independent \((Jh)E^\top\) recomputation. See `Backend/tests/test_jacobian_lens.py`.

## Why not encoder transports (use this table)

| | Pooled encoder transport (superseded) | Decoder-only (this paper) |
|---|---|---|
| Source | mean over 1500 encoder frames | residual at decoder position \(t\) |
| Target | mean over \(T\) decoder positions | \(h_{\mathrm{final},t'}\) for \(t'\ge t\) |
| Differentiation | after pooling | full sequence, then average |
| Spaces | encoder \(\to\) decoder | decoder \(\to\) decoder |
| Readout | \((h_b-\bar h)J^\top E^\top\), constant dropped | \(\mathrm{softmax}(EJh)\) on logit scale |
| Apply units | 0.3 s buckets | decoder tokens |
| CTC | yes (encoder) | no |
