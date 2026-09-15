# Related work — citation map

BibTeX keys match `paper/references.bib`.

## Direct ancestor

- `gurnee2026workspace` — Jacobian lens and J-space in LLMs. We copy the estimator (average causal Jacobian into the final residual, unembed with \(E\)) and *not* the encoder-side experiments. We do not claim to have reproduced the global-workspace causal suite (write, ablate, hold).

## Lenses on language models

- `nostalgebraist2020logitlens` — decode intermediate residuals with the unembedding directly (identity transport).
- `belrose2023tuned` — learned affine transport to the final layer (tuned lens).
- J-lens vs tuned lens: J-lens is the *average derivative* of the true map, not a regression onto final states.

## Encoder–decoder interpretation

- `langedijk2024decoderlens` — DecoderLens: skip encoder layers into the decoder to see what the encoder has computed. Complementary: they interpret the **encoder** via the decoder; we interpret the **decoder** via its own Jacobian.

## Probes

- `alain2017probes`, `hewitt2019structural`, `belinkov2022probing` — linear probes diagnose linearly available features; they are not causal maps to the model's own unembedding.

## Speech models and tools

- `radford2023whisper` — substrate.
- `baevski2020wav2vec2` — CTC family; excluded from the lens.
- `tenney2020lit` — LIT; ECHO is the audio analogue.
- Attention/saliency in ASR: cite as the *other* ECHO tools, not as competing lenses.

## Randomized linear algebra

- `hutchinson1989trace` — Rademacher/Hutchinson estimator we use for the averaged Jacobian.

## Do not cite

- “Hvingelby et al. (2023) Encoder Jacobian Lenses…” appears in an internal ECHO note as a label for the *superseded in-repo encoder transport*, not as a verified publication. Do not put it in the bibliography.
