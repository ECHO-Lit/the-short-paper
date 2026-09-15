# ECHO — Systems Paper

IEEE conference paper on **ECHO** (Explainable Computation for Hearing Outputs) as a whole: an interactive interpretability workbench for speech models. Companion to the Jacobian-lens short paper in [`../jacobian-lens/`](../jacobian-lens/).

This paper is a **systems / demo** draft. It surveys the platform, not a single method. Claims must match the ECHO-LIT implementation; do not invent user studies or metric tables.

## Layout

| Path | Role |
|---|---|
| [`context/`](context/) | Source notes for authors. Allowed claims and code pointers live here. |
| [`paper/`](paper/) | IEEE conference source (`IEEEtran`). |
| [`paper/main.pdf`](paper/main.pdf) | Compiled draft. |

Start with [`context/00-overview.md`](context/00-overview.md) and [`context/01-writing-notes.md`](context/01-writing-notes.md).

## Build the PDF

Requires a TeX distribution with `IEEEtran`, `tikz`, `amsmath`, `booktabs`, and `cite`.

```bash
cd paper
make
```

Clean aux files with `make clean`. The PDF is written to `paper/main.pdf`.

## Code

The system is implemented in [ECHO-LIT](https://github.com/ECHO-Lit/ECHO-LIT).
