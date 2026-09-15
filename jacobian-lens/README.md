# The Short Paper

Short paper on a **decoder-only Jacobian lens** for speech-to-text models, implemented in [ECHO-LIT](https://github.com/ECHO-Lit/ECHO-LIT). Companion to the ECHO systems / demo paper in [`../echo-systems/`](../echo-systems/).

The construction follows Gurnee et al. (2026), *Verbalizable Representations Form a Global Workspace in Language Models*, applied to Whisper's decoder residual stream rather than to an LLM.

## Layout

| Path | Role |
|---|---|
| [`context/`](context/) | Source notes for authors and writing assistants. Claims, method, findings, and citation keys live here — not in the TeX alone. |
| [`paper/`](paper/) | IEEE conference short-paper source (`IEEEtran`). |
| [`paper/main.pdf`](paper/main.pdf) | Compiled draft. |

Start with [`context/00-overview.md`](context/00-overview.md), [`context/07-writing-notes.md`](context/07-writing-notes.md), [`context/08-verification.md`](context/08-verification.md) (claim-by-claim check against the ECHO-LIT implementation), and [`context/09-use-cases.md`](context/09-use-cases.md) (what an analyst actually does with the grid).

## Build the PDF

Requires a TeX distribution with `IEEEtran`, `tikz`, `amsmath`, `booktabs`, and `cite`.

```bash
cd paper
make
```

Clean aux files with `make clean`. The PDF is written to `paper/main.pdf`.

## Code

The lens is implemented in ECHO-LIT:

- Fit / apply: `Backend/app/services/jacobian_lens_service.py`
- Tests: `Backend/tests/test_jacobian_lens.py`
- Technical reference in that repo: `JACOBIAN_LENS.md`
