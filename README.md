# Beat Tracker

A beat tracking system for ballroom dance music, written from scratch in Python with NumPy and librosa. It follows the approach of Simon Dixon's BeatRoot (2007): spectral flux onset detection, inter-onset interval (IOI) clustering for tempo induction, and a set of beat tracking agents that compete to find the best beat sequence.

## How it works

1. **Load audio**: resample to 44.1 kHz, convert to mono, normalise to [-1, 1].
2. **Spectral flux**: STFT with a 2048-sample Hamming window and hop size 512; sum of positive magnitude differences between consecutive frames, normalised.
3. **Peak picking**: an onset must be a local maximum, exceed an adaptive threshold (local mean + delta), and pass a smoothing condition.
4. **IOI clustering**: inter-onset intervals are grouped into clusters within a relative tolerance.
5. **Tempo induction**: median IOI converted to BPM and folded into the 60 to 190 BPM range.
6. **Beat tracking agents**: one agent starts from each onset, predicts beats at the estimated tempo and snaps to nearby onsets; the best-scoring agent's sequence is returned.

## Files

| File | Description |
| --- | --- |
| `beat_tracker.ipynb` | Final version (tested on Google Colab). Exposes `beatTracker(inputFile)` and asks for an audio path when run. |
| `beat_tracker_step_by_step.ipynb` | Earlier Jupyter version with the plots for each stage (waveform, spectral flux, peak picking, IOI clusters, beats). Set `audio_path` at the top. |

## Running

```bash
pip install -r requirements.txt
jupyter notebook beat_tracker.ipynb
```

Or open the notebook in Google Colab, run the cell and enter the path to a `.wav` file when prompted.

```python
beats, onsets = beatTracker("path/to/track.wav")
```

Audio from the ballroom dataset is not included in this repository.
