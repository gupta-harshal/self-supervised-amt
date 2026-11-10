# Self-Supervised AMT

**Teaching machines to listen to music and write it down.**

Self-supervised sequence-to-sequence audio transformers for automatic polyphonic music transcription, where mathematics meets art: signal processing and deep learning decode raw sound into structured notes. The model is pretrained on unlabelled audio, fine-tuned on MAESTRO, and evaluated with note-level F1.

## Overview

Automatic Music Transcription (AMT) converts a raw audio recording into a symbolic score: a set of note events, each described by pitch, onset time, offset time and velocity. Polyphonic audio, where many notes sound at once and their harmonics overlap, is the hard case, and it is the case this project works on.

Two ideas define the approach:

1. **Sequence-to-sequence modelling.** Instead of labelling every time frame independently, a transformer encoder-decoder generates note events as a token sequence, the way a translator generates words. The model learns note durations and musical structure jointly instead of bolting them on afterwards with thresholds.
2. **Self-supervised pretraining.** Audio aligned with exact note labels is rare and expensive to capture; unlabelled audio is abundant. Pretraining on structure in unlabelled audio first, then fine-tuning on a small labelled set, attacks the real bottleneck of AMT: labels, not audio.

## Pipeline

```
Audio (waveform)
      |
Feature extraction      CQT / log-mel spectrogram + amplitude envelope
      |
SSL pretraining         Masked feature prediction on unlabelled audio
      |
Transformer enc-dec     Fine-tuned on MAESTRO
      |
Note events             Pitch, onset, offset, velocity  ->  MIDI
```

## Dataset

**MAESTRO (v3.0.0)** — roughly 1,276 performances and 200 hours of piano audio with note-aligned MIDI, captured on Disklavier pianos at international competitions. It is the standard AMT benchmark, which makes results directly comparable with published baselines. The standard train / validation / test split is used unchanged.

MAESTRO is piano-only, so the supervised task is piano transcription. The self-supervised stage is where broader unlabelled audio enters the design.

## Evaluation

Evaluation follows the protocol of the baselines, using `mir_eval`:

- Note-wise precision, recall and F1 with a 50 ms onset tolerance
- Note F1 with offsets (onset + offset both scored)
- Frame-level F1

**Core ablation:** the identical model with vs without self-supervised pretraining. That single comparison is the project's central claim.
**Secondary ablation:** CQT vs log-mel input features.

Baselines for comparison: Onsets and Frames (Hawthorne et al., 2018) and an MT3-style T5 seq2seq model (Gardner et al., 2022).

## Repository structure

```
docs/          Literature survey and methodology / design documents
notebooks/     Feature exploration notebooks (DSP groundwork)
src/
  features/    Feature extraction: CQT, log-mel, amplitude envelope   [in development]
  data/        MAESTRO loading, tokenisation of note events
  models/      Transformer encoder-decoder
  train/       Self-supervised pretraining, then fine-tuning
  eval/        mir_eval based note-level evaluation
references.bib BibTeX for all cited work
```

## Status

- Literature survey: complete (15 core works, see `docs/literature-survey.md`)
- Musical feature groundwork (DSP): in progress — amplitude envelope, spectrogram, mel, MFCC, chroma, CQT
- Architecture and pipeline design: finalised (see `docs/methodology.md`)
- Dataset and evaluation protocol: fixed
- Implementation phases: data/feature pipeline and baseline reproduction -> SSL pretraining -> fine-tuning and ablations -> evaluation and demo (audio in, MIDI out)

## Tech stack

Python, PyTorch, librosa, mir_eval

## References

Full survey in [docs/literature-survey.md](docs/literature-survey.md); BibTeX in [references.bib](references.bib).

## Author

Harshal Gupta — B.Tech, Computer Science & Engineering (Data Science & AI), Indian Institute of Information Technology Ranchi
