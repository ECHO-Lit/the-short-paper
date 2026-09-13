# Limitations (must appear in the paper)

1. **First-order linearization.** Error grows when a position's trajectory is far from the average context used to fit \(J\). Trust rankings more than absolute scores.
2. **Averaged \(J\) erases pairwise alignment.** The lens says *what* is verbalizable, not *which audio frame* a token reads. Cross-attention remains the alignment tool.
3. **Teacher-forced fit.** \(J\) reflects “given this prefix,” not free-running generation dynamics.
4. **Uncalibrated softmax.** Probabilities are for display.
5. **Frequent-token geometry.** Common words dominate rankings more often than rare ones.
6. **Probe noise.** 4–32 Rademacher probes leave residual estimation error.
7. **Model-bound artifact.** Refit on any weight or revision change.
8. **Read-only.** No steering, ablation, or patching (the LLM J-lens write mode).
9. **Readout axis is text, not time.** A deliberate trade-off of the decoder-only redesign.
10. **No large-scale census yet.** Workspace-band statistics, intervention experiments, and multi-size Whisper comparisons are open.

## Known non-goals

- CTC encoder readout (removed with the redesign).
- Replacing attention or saliency; the three tools answer different questions.
