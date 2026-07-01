<!--
===============================================================================
EchoForge Toolchain
-------------------------------------------------------------------------------

File:
    toolchain.md

Purpose:
    Documents the external software, libraries, AI models, and utilities that
    EchoForge may use throughout its processing pipeline.

    This document serves as a technology reference for contributors and
    describes the preferred tools, optional alternatives, and future
    possibilities for each processing stage.

    EchoForge is designed to remain tool-agnostic. Every component should be
    replaceable provided it conforms to the plugin interfaces defined by the
    framework.

Status:
    Living Technology Reference

Created:
    2026-06-29

License:
    Apache License 2.0

Maintainer:
    EchoForge Contributors

Repository:
    https://github.com/chronomicron/echoforge

This document is part of the EchoForge project and should be kept in
sync with the implementation whenever possible.

===============================================================================
-->

# EchoForge Toolchain

## Introduction

EchoForge is an orchestration framework.

Rather than implementing every AI algorithm itself, EchoForge coordinates
specialized open-source tools through a modular plugin architecture.

Whenever practical, EchoForge should favor:

* Free software
* Open-source implementations
* Local execution
* GPU acceleration when available
* Replaceable components

Commercial APIs may be supported as optional plugins but should never be
required to use the framework.

EchoForge plugins invoke these technologies.
The EchoForge Engine remains independent of any individual AI model or library.

---

# Stage 1 — Media Decomposition

## Purpose

Inspect source media and extract all available streams without modifying the
original files.

Typical outputs include:

* Video streams
* Audio streams
* Subtitle tracks
* Chapter markers
* Container metadata

### Preferred

* FFmpeg
* ffprobe

### Alternatives

* MediaInfo

### Status

Preferred implementation

---

# Stage 2 — Subtitle Parsing

## Purpose

Read subtitle files and convert them into structured dialogue segments.

Supported formats may include:

* SRT
* ASS
* SSA
* WebVTT

### Preferred

* Native Python parser

### Alternatives

* pysubs2

### Status

Preferred implementation

---

# Stage 3 — Transcript Interpretation

## Purpose

Analyze dialogue and infer conversational structure.

Possible tasks include:

* Estimating the number of speakers
* Identifying dialogue turns
* Detecting conversations
* Inferring speaker changes
* Recognizing character names
* Generating confidence estimates

### Preferred

* Ollama (local LLM)

### Alternatives

* llama.cpp
* GPT-5 API
* Claude API
* Gemini API

### Status

Optional plugin

---

# Stage 4 — Speech Transcription

## Purpose

Generate timestamped transcripts from audio when subtitles are unavailable.

### Preferred

* Faster-Whisper

### Alternatives

* Whisper
* WhisperX
* NVIDIA NeMo

### Status

Preferred implementation

---

# Stage 5 — Speaker Diarization

## Purpose

Determine when different speakers are talking.

Output consists of anonymous speaker clusters rather than identified
individuals.

### Preferred

* pyannote.audio

### Alternatives

* NVIDIA NeMo
* SpeechBrain

### Status

Preferred implementation

---

# Stage 6 — Speaker Identification

## Purpose

Associate speaker clusters with the target individual using voice embeddings.

### Preferred

* pyannote.audio embeddings

### Alternatives

* ECAPA-TDNN
* SpeechBrain
* NVIDIA NeMo

### Status

Planned

---

# Stage 7 — Face Detection

## Purpose

Locate human faces within video frames.

### Preferred

* InsightFace

### Alternatives

* OpenCV
* MediaPipe

### Status

Preferred implementation

---

# Stage 8 — Face Recognition

## Purpose

Associate detected faces with known identities or reference images.

### Preferred

* InsightFace

### Alternatives

* DeepFace
* face_recognition (dlib)
* CompreFace

### Status

Planned

---

# Stage 9 — Face Tracking

## Purpose

Track detected faces across consecutive video frames, reducing unnecessary
re-detection and providing temporal continuity.

### Preferred

* ByteTrack

### Alternatives

* DeepSORT
* OpenCV Trackers

### Status

Planned

---

# Stage 10 — Audio Extraction

## Purpose

Extract audio streams from source media.

Possible outputs include:

* WAV
* FLAC
* MP3
* AAC

### Preferred

* FFmpeg

### Alternatives

* GStreamer

### Status

Preferred implementation

---

# Stage 11 — Dialogue Isolation

## Purpose

Separate spoken dialogue from music and sound effects.

### Preferred

* Demucs

### Alternatives

* Ultimate Vocal Remover (UVR)
* Asteroid
* SpeechBrain

### Status

Experimental

---

# Stage 12 — Noise Reduction

## Purpose

Reduce background noise while preserving speech quality.

### Preferred

* DeepFilterNet

### Alternatives

* RNNoise
* Demucs
* SpeexDSP

### Status

Experimental

---

# Stage 13 — Audio Conditioning

## Purpose

Prepare dialogue for downstream machine learning.

Possible processing includes:

* Loudness normalization
* Equalization
* Silence trimming
* Resampling
* Channel conversion

### Preferred

* FFmpeg
* librosa

### Alternatives

* SoX
* pydub

### Status

Planned

---

# Stage 14 — Quality Analysis

## Purpose

Measure objective characteristics of processed artifacts.

Examples include:

* Loudness (LUFS)
* Peak levels
* RMS
* Estimated signal-to-noise ratio
* Speech probability
* Music probability
* Clipping detection
* Silence percentage

### Preferred

* librosa
* pyloudnorm
* NumPy

### Alternatives

* Essentia

### Status

Planned

---

# Stage 15 — Evidence Fusion

## Purpose

Combine observations from multiple plugins into probabilistic confidence
estimates.

Evidence may originate from:

* Face recognition
* Speaker recognition
* Subtitle analysis
* Speech transcription
* Human review

### Preferred

* Native EchoForge implementation

### Alternatives

Future research

### Status

Core framework

---

# Stage 16 — Dataset Export

## Purpose

Generate datasets suitable for downstream machine learning applications.

Examples include:

* Style-Bert-VITS2
* Fish Speech
* Coqui TTS
* RVC
* CSV
* JSON
* Custom exporters

### Preferred

* Native EchoForge exporters

### Alternatives

Community-developed exporters

### Status

Planned

---

# Supporting Libraries

The following Python libraries are expected to be useful throughout the
project.

## Scientific Computing

* NumPy
* SciPy

## Data Processing

* pandas

## Machine Learning

* PyTorch

## Computer Vision

* OpenCV

## Audio Processing

* librosa
* soundfile

## Image Processing

* Pillow

## Configuration

* PyYAML

## Database

* SQLite (Python standard library)

---

# Guiding Philosophy

EchoForge is intentionally tool-agnostic.

Every external dependency should be isolated behind a plugin interface.

The framework should depend on standardized artifacts, metadata, and
evidence—not on the internal behavior of any specific AI model.

As new open-source models become available, contributors should be able to
replace individual plugins without modifying the overall architecture.

The framework coordinates the pipeline.

The plugins perform the work.
