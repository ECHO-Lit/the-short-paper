# Writing notes

## Voice

IEEE conference systems / demonstration paper, ~5–6 pages plus references. Hedged. No invented metrics. The Jacobian-lens short paper (`../jacobian-lens/`) owns the estimator; this paper names the lens as one panel.

## Author block

Same as the short paper: Januda Lelwala, Anas Hussaindeen, Chandupa Ambepitiya, Dewmike Amarasinghe. University of Moratuwa. Mentor in acknowledgments: Dr. Uthayasanker Thayasivam.

## Claims that are allowed

- ECHO is the audio analogue of LIT (Tenney et al., 2020).
- FastAPI has no PyTorch/Transformers dependency; Celery workers load models.
- Built-in models: `whisper-base`, `whisper-large` (`openai/whisper-large-v3`), `wav2vec2` (`r-f/wav2vec-english-speech-emotion-recognition`).
- Custom HF models: seq2seq ASR, CTC ASR, audio classification; `trust_remote_code=False`.
- Job operations listed in `JobOperation`.
- Queue routing in `queue_for`.
- Datasets listed in `DATASET_PATHS`.
- Layer probes report accuracy, majority baseline, and control-task selectivity.
- Fairness uses percentile bootstrap CIs, not BCa.
- J-lens is decoder-only, seq2seq only, six maps on whisper-base.
- Saliency job API: GradCAM, LIME, SHAP. Service also implements Integrated Gradients and LRP.
- Perturbation job types: noise, time masking, pitch shift, time stretch. FR-7 sweeps also include frequency masking.
- Whisper must load with `attn_implementation="eager"` for attention jobs.

## Claims that are not allowed without new experiments

- User-study time-to-insight or “analysts prefer ECHO.”
- Numeric WER / fairness-gap / probe-accuracy tables.
- “First interpretability tool for speech.”
- “Whisper has a global workspace.”
- BCa confidence intervals.
- Streaming ASR, write/steer/ablate, or a plugin marketplace (roadmap only).

## Build

```bash
cd paper && make
```

## Venue (undecided)

`IEEEtran` conference class is a reasonable ICASSP / Interspeech / IEEE SLT / EMNLP demo draft.
