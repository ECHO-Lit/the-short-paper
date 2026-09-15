# Verification log (checked against ECHO-LIT, 2026-09-13)

Every scientific claim in `paper/main.tex` must match this file. Sources: `jacobian_lens_service.py`, `test_jacobian_lens.py`, `model_adapters.py`, `schemas/jobs.py`, a fitted `lens.pt` metadata record, and current Hugging Face `modeling_whisper.py`.

## Confirmed

| Claim | Evidence |
|---|---|
| Architecture gate is `"decoder"` only for `SEQ2SEQ_ASR` | `AudioModelAdapter.jacobian_lens_architecture`; whisper-base → `"decoder"`; wav2vec2 and CTC → `None` |
| Encoder runs under `no_grad` | `_encoder_features` |
| Fit is teacher-forced | `_decoder_input_ids` shifts labels right past start token |
| Sources = `hidden_states` except identity-equal `last_hidden_state` | `_decoder_states` |
| Target is `last_hidden_state` (post final LN) | decoder `self.layer_norm` then `last_hidden_state` |
| Hutchinson: `outer(r, sum_t g_t) / (T(T+1)/2)`, then `/ (N P)` | `fit_decoder_jacobian_lens` |
| Stored `J` is `(d_out, d_in)`; apply is `h @ J.T @ E.T` | `torch.outer(probe, summed)`; `values = state @ matrix.T`; `logits = values @ weight.T` |
| Softmax is display-only; ranking from logits | `topk` on logits; `probability` from softmax of full logits at those ids |
| No extra RMS-norm at readout | apply path has none |
| `E` is `get_output_embeddings().weight`, tied on Whisper | `proj_out.weight` tied to `embed_tokens` (`_tied_weights_keys`) |
| Artifact format v2, method `hutchinson-decoder-vjp`, no baselines | fit return dict; test asserts `"baselines" not in artifact` |
| Defaults: `probe_count=4`, `top_k=5`, `max_new_tokens=64`, samples 2–1000, audio ≤30 s (cap 60) | `JacobianLensFitParameters` / `ApplyParameters` |
| Apply positions from provided transcript or `model.generate` | `transcript_source` `"provided"` / `"generated"` |
| Linear stub closed form `J = 2M/(T+1)` when the map is position-wise (only `t=t'` terms) | `test_decoder_jacobian_lens_fit_matches_linear_closed_form`; relative Frobenius error < 0.1 at 512 probes |
| Apply matches independent `(J h) E^T` | `test_decoder_jacobian_lens_apply_ranks_through_model_head` |
| Whisper-base: `d=512`, 6 decoder blocks, `max_source_positions=1500` | WhisperConfig; conv stride 2 on 3000-mel → 1500 |
| A real fit on this checkout: whisper-base, 1000 samples, 4 probes, **6** matrices | `jacobian-lenses/.../metadata.json` `layer_count: 6` |
| UI labels “Layer {i+1}” for 0-based `layer` | `JacobianLensVisualization.tsx` |
| CTC excluded | catalog capabilities omit jacobian ops for CTC |

## Corrected (were wrong in the first draft)

| Old claim | Fact |
|---|---|
| Whisper-base has **7** lens layers (embed + 6 blocks) | Current HF Whisper records `hidden_states` from `WhisperDecoderLayer` only (`_can_record_outputs`). Embedding output is not in the tuple. `last_hidden_state` is a distinct post-LN tensor, so it is not dropped by `is not`. **Six sources** on whisper-base. |
| Layer 0 = embedding output | Layer 0 = first decoder **block** output |
| “Greedy generation” | Apply calls `model.generate(**inputs, max_new_tokens=…)`. Whisper uses `WhisperGenerationMixin` (language detection, special tokens). Say “the model’s `generate()`”, not “greedy”. |
| SOT residual is audio-only | Position 0 still has the SOT embedding and positional embedding; cross-attention adds audio. Prefix contains no *transcribed words*. |
| Qualitative “recoveries” (crystallization, loops, …) | Documented as a *reading protocol* in `JACOBIAN_LENS_INSIGHTS.md`. No saved apply-grid, no WER, no agreement curve in the repo. Must not be reported as results. |

## Not claimed

- Workspace-band statistics, steering, ablation, write mode
- Numerical comparison to logit lens, tuned lens, or DecoderLens
- That Whisper “has a global workspace”
