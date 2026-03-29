# VoiceAnalysis

> The source code for this project is in a private repository. This showcase highlights the design, features, and technical decisions behind it.

An audio filler sound detection and removal tool built for voice-over, podcast, and audio post-production workflows. Upload audio, detect unwanted sounds using three independent algorithms, review detections on an interactive waveform, and export clean audio — all from a single web interface.

---

## Preview

### Detection Mode — Acoustic Fingerprinting
Upload audio and run acoustic detection against your custom sound profiles. The idea is to be able to capture profiles from clients and use them to detect repeated sounds on different projects. 
Detections appear as labeled regions on the waveform with confidence scores. The whole idea is to easily detect sounds and noises that I want to remove and easily remove them from the file. 

![Acoustic Detection Results](https://github.com/user-attachments/assets/d24a38d6-b1f0-4e07-bbb6-e86c4f61ce95)

### Detection Mode — Whisper Transcription
Use OpenAI's Whisper model to transcribe audio and find spoken filler words by keyword matching.Configurable filler word list, confidence threshold, and VAD-based hallucination filtering.

![Whisper Transcription Mode](https://github.com/user-attachments/assets/49efeeb5-4526-411b-8a13-02c4fe484601)

### Detection Mode — Breath Detection
Detect breath sounds in non-speech regions using Silero VAD inversion combined with spectral flatness analysis. Adjustable sensitivity slider controls detection aggressiveness.

![Breath Detection Mode](https://github.com/user-attachments/assets/a6f4d8e5-791a-4e9e-ae6a-0cfc33526402)

### Word Search
Upload multiple audio takes, transcribe them all, then search for specific words or phrases across every take.

![Word Search Tab](https://github.com/user-attachments/assets/cbb67b4b-b6d7-4a1d-ad06-69eb617ca432)

In each case, you can download a clean file after selecting what to remove.

---

## Key Features

### Three Detection Algorithms
- **Acoustic** — Build a reference database of sounds you want to detect. The system extracts 39-dimensional MFCC features and uses Dynamic Time Warping to find similar sounds in your audio, regardless of speed or pitch variations.
- **Transcription** — Runs faster-whisper to transcribe the full audio, then matches your filler word list against the transcript. Silero VAD validates each detection to filter out hallucinations.
- **Breath Detection** — Inverts Silero VAD output to find non-speech regions, filters by energy to exclude silence, then uses spectral flatness to confirm the noise-like character of breath sounds.

### Interactive Waveform Review
- Waveform visualization powered by wavesurfer.js v7
- Click any detection to jump to it and hear the audio
- Drag region boundaries to fine-tune start/end points
- Zoom, auto-play on click, and stop-at-next-selection controls

### Audio Editing
- Silence selected detections with configurable crossfade transitions
- Non-destructive: silencing preserves total audio duration (no cuts)
- Extract specific regions and download as separate files

### Word Search
- Upload multiple audio takes (WAV, MP3, FLAC)
- Automatic transcription of all takes via Whisper
- Search for words or phrases across all takes simultaneously
- Per-take waveform selections with persistent region state
- Batch download selected results with checkbox controls

### Sound Profiles
- Create named profiles for different sound types
- Upload reference samples to train acoustic fingerprints
- "Remove" mode flags detections, "Keep" mode suppresses false positives
- Profiles persist in SQLite database
---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python 3.10+, FastAPI, uvicorn |
| Database | SQLAlchemy 2.0 async, aiosqlite (SQLite) |
| Audio Processing | librosa, soundfile, pydub, numpy, scipy |
| Acoustic Matching | fastdtw + scipy cosine distance |
| Transcription | faster-whisper (CTranslate2), Silero VAD |
| Frontend | Vanilla HTML/CSS/JS, wavesurfer.js v7 (ESM) |

---

## Status

This project is under active development.
