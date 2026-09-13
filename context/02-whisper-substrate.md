# Whisper substrate (facts for the method section)

Whisper is an encoder–decoder Transformer. Encoder and decoder share width \(d\) within a size.

## whisper-base (default)

| | Encoder | Decoder |
|---|---|---|
| Layers | 6 | 6 |
| Width \(d\) | 512 | 512 |
| Heads | 8 | 8 |
| Audio context | 1500 frames (~20 ms, 30 s pad) | — |
| Text context | — | \(T \le 448\) |
| Vocab | — | 51,865 (tied `proj_out`) |

## Tensor flow that the lens actually sees

1. Log-Mel \(\to\) convs \(\to\) encoder blocks \(\to\) \((1, 1500, d)\). This tensor is computed **once, no grad**, and fed as cross-attention K/V.
2. Decoder input ids (teacher-forced, labels shifted right past SOT).
3. Current Hugging Face Whisper records `hidden_states` from each `WhisperDecoderLayer` (six on base). The embedding output is **not** in that tuple.
4. `last_hidden_state` is `layer_norm` of the last block — a distinct tensor. The identity filter in `_decoder_states` therefore keeps all six block outputs.
5. **Target** = `last_hidden_state`, the exact tensor `proj_out` consumes.

## Why the encoder is not the workspace

The encoder residual stream is acoustic and is never unembedded. Verbalization happens only after cross-attention has written audio into the **decoder** stream and the LM head is applied. A lens on encoder states is a probe of acoustics, not of what the model is poised to say.

CTC models (wav2vec2) have no decoder; the implementation refuses them (`jacobian_lens_architecture() is None`).

## Hard constraint

Do not describe the readout axis as audio time. After the decoder-only redesign, positions are **transcript tokens**. Audio-frame support is a cross-attention question.
