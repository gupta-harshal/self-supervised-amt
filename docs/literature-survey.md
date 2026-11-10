# Literature Survey

Fifteen core works, in the order the field developed. The pattern across them: the strongest transcription systems are fully supervised, and self-supervised pretraining for AMT itself remains underexplored. That gap is what this project works on.

| # | Work | Year | Key contribution | Gap it leaves |
|---|------|------|------------------|----------------|
| 1 | Benetos et al. | 2013 | Survey of AMT challenges and open problems | Framed the field; no model |
| 2 | Sigtia et al. | 2016 | End-to-end neural network for polyphonic piano transcription | Supervised; RNN era |
| 3 | Vaswani et al. | 2017 | The Transformer architecture | General architecture, not audio |
| 4 | Hawthorne et al. | 2018 | Onsets and Frames: dual onset + frame objective | Frame-wise; fully supervised |
| 5 | Hawthorne et al. | 2019 | MAESTRO dataset: ~200 hrs aligned piano audio + MIDI | Piano only |
| 6 | Huang et al. | 2019 | Music Transformer: relative attention for music sequences | Generation, not transcription |
| 7 | Thickstun et al. | 2018 | Learning features of music from scratch: learned representations rival hand-crafted features | Feature learning, not transcription itself |
| 8 | Baevski et al. | 2020 | wav2vec 2.0: self-supervised learning for audio | Speech domain, not music |
| 9 | Raffel et al. | 2020 | T5: unified text-to-text transformer framework | Base framework used by MT3 |
| 10 | Kong et al. | 2020 | GiantMIDI-Piano: large-scale transcribed piano corpus | Labels are model-generated |
| 11 | Kong et al. | 2021 | High-resolution piano transcription with pedals, regressing onset/offset times | Supervised; piano only |
| 12 | Hsu et al. | 2021 | HuBERT: SSL by masked prediction of hidden units | Speech domain, not music |
| 13 | Gardner et al. | 2022 | MT3: multi-instrument transcription framed as seq2seq | Fully supervised |
| 14 | Bittner et al. | 2022 | Basic Pitch (Spotify): lightweight, instrument-agnostic transcription | Supervised; fixed label set |
| 15 | Li et al. | 2023 | MERT: self-supervised music audio representations | Understanding tasks, not AMT |

## Reading notes

- **Frame-wise line** (Onsets and Frames, Kong et al.): predict a piano-roll per frame, then form notes with thresholds. Strong numbers, but note formation is post-processing, and offsets are fragile under pedal.
- **Seq2seq line** (MT3): notes are emitted as event tokens. Output structure is learned, which is the property this project builds on.
- **Self-supervised line** (wav2vec 2.0, HuBERT, MERT): masked-prediction pretraining works for speech and for music understanding tasks. It has not been systematically evaluated as the front end of an AMT system; that evaluation is the contribution here.
- **Data line** (MAESTRO, GiantMIDI-Piano): labelled data quality (MAESTRO, Disklavier-captured) beats labelled data quantity with machine-generated labels (GiantMIDI-Piano) for evaluation purposes.
