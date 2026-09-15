# Overview — what this paper is

> Companion notes for the IEEE systems paper in `paper/`. Keep claims honest; the TeX should not invent results that are not in this folder or in ECHO-LIT.

## One-sentence thesis

ECHO is a LIT-style interactive workbench for **speech** models: a FastAPI control plane and Celery workers that put complementary interpretability tools—prediction, attention, embeddings, saliency, perturbations, layer probes, grouped fairness, and a decoder Jacobian lens—on the same clip, dataset, and session.

## What the paper is (and is not)

**Is:** a systems / demonstration paper.

- The gap: audio is temporal and (for ASR) encoder–decoder; text LIT does not cover it.
- The architecture: API without PyTorch; workers own models; Redis + object storage; capability-gated adapters.
- The toolkit: one question per tool, used together.
- The Jacobian lens is **one** tool, not the paper's centre.

**Is not:** a large-scale empirical study, a user study, or a methods paper about the J-lens (that is [`../jacobian-lens/`](../jacobian-lens/)). Do not report numeric WER, probe accuracies, fairness gaps, or workspace-band curves unless a measured table is added later.

## Contributions (use this wording)

1. **Audio LIT workbench.** Browser UI over Whisper ASR, wav2vec 2.0 emotion classification, and user-registered Hugging Face speech models (seq2seq, CTC, classification).
2. **Split runtime.** Control plane validates sessions, uploads, and jobs without importing the ML stack; workers run capability-gated adapters on `cpu` / `gpu-fast` / `gpu-large` queues.
3. **Complementary tools on one contract.** Attention (where), saliency (which waveform region), embeddings and HDBSCAN (which clips cluster), perturbations and FR-7 sweeps (how brittle), layer probes (what is linearly available at each encoder depth), FR-10 fairness and saliency grounding (who is harmed; whether explanations sit on speech), decoder J-lens (what the decoder is poised to say).
4. **Datasets and custom models.** Built-in Common Voice, RAVDESS, LibriSpeech-1000, L2-ARCTIC, Speech Accent Archive, plus session-scoped custom datasets and Hugging Face repos validated with `trust_remote_code=False`.

## Suggested title

**ECHO: An Interactive Interpretability Workbench for Speech Models**
