# features

Feature extraction for the transcription pipeline. **In development** — being built alongside a ground-up study of audio signal processing for machine learning.

Planned modules:

- Amplitude envelope extraction (onset-sensitive channel)
- STFT and spectrogram computation
- Mel spectrogram
- Constant-Q transform (CQT)
- Feature normalisation and sequence framing for the encoder

The working question for every feature: how does it behave on *polyphonic* audio, where overlapping notes smear exactly the structure single-note analysis relies on? Exploration lives in `notebooks/`; settled code lands here.
