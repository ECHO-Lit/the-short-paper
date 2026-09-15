# ECHO — the host platform

ECHO (Explainable Computation for Hearing Outputs) is a Learning Interpretability Tool for **audio** models: the speech counterpart of Google's LIT for text and tabular models.

## What a user can do

- Upload and browse audio (Common Voice, RAVDESS, LibriSpeech-1000, L2-ARCTIC, SAA, custom datasets with transcript manifests).
- Run Whisper ASR and Wav2Vec2 emotion inference.
- Visualize self- and cross-attention, embeddings (PCA / t-SNE / UMAP), saliency (GradCAM, IG, LIME, SHAP, LRP), and perturbations.
- Fit and apply a **Jacobian lens** on seq2seq ASR models (this paper).

## Runtime (one paragraph for the paper)

A FastAPI control plane owns sessions and job metadata. Celery workers run model code. Redis is the broker. The lens fit is CPU/GPU-bound inside the worker: frozen encoder under `no_grad`, Hutchinson VJPs through the decoder, artifact stored as `lens.pt` (format v2). Apply re-runs the decoder (greedy generation or a provided transcript) and returns a `(position × layer)` top-\(k\) grid.

## Why the lens lives here

Attention says *where the decoder looked* in the encoder (audio frames). Saliency says *which waveform regions moved a scalar*. Neither says *what the decoder is poised to say* at a given depth. That is the J-lens cell.

## Code map (do not dump into the paper)

| Concern | Path |
|---|---|
| Fit / apply | `Backend/app/services/jacobian_lens_service.py` |
| Architecture gate (`"decoder"` iff `SEQ2SEQ_ASR`) | `Backend/app/worker/model_adapters.py` |
| Job schemas | `Backend/app/schemas/jobs.py` |
| Worker | `Backend/app/worker/executor.py` |
| UI | `Frontend/src/pages/JacobianLensLab.tsx` |
| Tests | `Backend/tests/test_jacobian_lens.py` |

Repo: https://github.com/ECHO-Lit/ECHO-LIT
