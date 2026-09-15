# ECHO papers

IEEE drafts for [ECHO-LIT](https://github.com/ECHO-Lit/ECHO-LIT). This repository holds **both** papers; they are companions, not replacements.

| Paper | Folder | PDF |
|---|---|---|
| **A Decoder-Only Jacobian Lens for Speech-to-Text Models** (methods short paper) | [`jacobian-lens/`](jacobian-lens/) | [`jacobian-lens/paper/main.pdf`](jacobian-lens/paper/main.pdf) |
| **ECHO: An Interactive Interpretability Workbench for Speech Models** (systems / demo paper) | [`echo-systems/`](echo-systems/) | [`echo-systems/paper/main.pdf`](echo-systems/paper/main.pdf) |

## Jacobian lens (methods)

Decoder-only Jacobian lens for Whisper: one \(J_\ell\) per decoder block, Hutchinson VJPs, same-space readout through the model's unembedding. Negative result on pooled encoder transports. Qualitative \((\text{position}\times\text{layer})\) readings.

```bash
cd jacobian-lens/paper && make
```

## ECHO systems (workbench)

The full platform: FastAPI control plane, Celery workers, and the complementary toolkit (prediction, attention, embeddings, saliency, perturbations, layer probes, fairness/grounding, and the Jacobian lens as one panel).

```bash
cd echo-systems/paper && make
```

## Code

Implementation: [ECHO-Lit/ECHO-LIT](https://github.com/ECHO-Lit/ECHO-LIT).
