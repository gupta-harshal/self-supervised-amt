# Methodology and Design

## Problem formulation

Given a waveform x(t), or its time-frequency representation X, estimate the set of note events

N = { (pitch_i, onset_i, offset_i, velocity_i) }

for every note in the recording. Polyphony makes this hard: simultaneous notes share the spectrum, harmonics collide (most severely at octaves and fifths), and sustain pedal plus room reverberation blur note offsets.

## Approach

### 1. Feature extraction
Audio is converted to a time-frequency representation: CQT or log-mel spectrogram, with the amplitude envelope as an auxiliary onset-sensitive channel. Feature behaviour on polyphonic audio (smearing of onsets, harmonic stacking in CQT bins) is studied explicitly in `notebooks/` before the choice is locked.

### 2. Self-supervised pretraining
The encoder is pretrained on unlabelled audio with a masked-prediction pretext task: mask spans of the feature sequence and predict the missing content (continuous features or quantised units, in the wav2vec 2.0 / HuBERT family). The design risk is explicit: a careless pretext task learns timbre and recording conditions instead of note structure, so the pretext target is chosen to force pitch and onset information to be represented.

### 3. Sequence-to-sequence fine-tuning
On MAESTRO, the pretrained encoder is paired with a transformer decoder that generates note events autoregressively. Each note is tokenised into pitch, onset, offset and velocity tokens. Training objective: cross-entropy over event tokens.

### 4. Evaluation
mir_eval note-wise precision / recall / F1 at 50 ms onset tolerance, note F1 with offsets, and frame-level F1, on the standard MAESTRO test split, under the same protocol as the baselines.

## Comparisons and ablations

| Question | Comparison |
|----------|------------|
| Does SSL pretraining help AMT? | Identical model, with vs without pretraining (core claim) |
| Which input feature? | CQT vs log-mel |
| Where does the design stand? | Against Onsets and Frames and an MT3-style T5 baseline, same split, same tolerance |

## Known failure modes to watch

- Octave errors from harmonic collisions
- Soft onsets missed under sustain pedal
- Offset errors in reverberant passages

## Roadmap

1. Data and feature pipeline; reproduce the Onsets and Frames baseline
2. Self-supervised pretraining on unlabelled audio
3. Fine-tuning on MAESTRO; pretraining and feature ablations
4. Full evaluation table; demo: audio in, MIDI out, on a short polyphonic clip
5. Documentation and write-up
