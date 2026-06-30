<!--
===============================================================================
EchoForge Getting Started
-------------------------------------------------------------------------------

File:
    getting_started.md

Purpose:
    Introduces new users to EchoForge.

    This guide explains the intended workflow, project creation, media
    organization, pipeline execution, and general usage of the framework.

    It is written from the perspective of an end user rather than a developer.

Status:
    Living User Guide

Created:
    2026-06-29

License:
    Apache License 2.0

===============================================================================
-->


# EchoForge User Guide (Draft)

> **Status:** Early design document (Version 0.1)
>
> This document describes the intended workflow of EchoForge. It is a design guide and roadmap rather than a description of currently implemented functionality.

---

# Introduction

EchoForge is an open-source framework for building high-quality speech datasets from audiovisual media.

Rather than focusing solely on Text-to-Speech (TTS), EchoForge aims to provide a modular, extensible pipeline capable of extracting, analyzing, organizing, and exporting speech data from movies, television, interviews, podcasts, and other recorded media.

Every stage of the pipeline is independent, allowing contributors to improve or replace individual components without affecting the rest of the framework.

---

# Design Philosophy

EchoForge follows several core principles.

## Modular

Every processing stage is independent.

Each stage has clearly defined inputs and outputs and may be replaced by alternative implementations.

## Evidence-Based

EchoForge never assumes a single source of information is correct.

Instead, multiple independent sources contribute evidence:

* Video analysis
* Audio analysis
* Speech recognition
* Speaker recognition
* Face recognition
* Transcript analysis
* Future plugins

These sources are combined into confidence scores rather than binary decisions.

## Human-Assisted

Automation is preferred.

Human input is optional.

When requested, EchoForge asks the user only for information that provides the greatest improvement to the overall dataset.

Examples include:

* Identifying a speaker cluster
* Identifying a face cluster
* Confirming uncertain dialogue
* Providing reference images or recordings

## Local First

EchoForge is designed to operate entirely offline.

All major pipeline stages should have fully open-source implementations.

Optional cloud or commercial APIs may be configured by users but are never required.

## Preserve Everything

EchoForge never discards information.

Original media remains untouched.

Intermediate results are preserved.

Metadata is stored whenever practical.

This allows processing to resume at any point and enables future improvements without repeating earlier work.

---

# Typical Workflow

## Step 1 – Create a Project

A project contains all configuration, source material, intermediate files, metadata, and exported datasets.

```
echoforge init my-project
```

---

## Step 2 – Add Source Media

Place source media inside the project's source directory.

Examples:

* Movies
* Television episodes
* Interviews
* Podcasts with video

Optional files include:

* Subtitle (.srt) files
* Closed captions
* Chapter metadata

---

## Step 3 – Scan Media

EchoForge indexes all available media.

The scan records:

* Duration
* Resolution
* Audio streams
* Subtitle streams
* Language
* Metadata

No processing occurs during this stage.

---

## Step 4 – Generate Evidence

Independent plugins analyze the media.

Examples include:

* Audio extraction
* Speech transcription
* Face detection
* Face tracking
* Speaker diarization
* Speaker identification
* Subtitle alignment
* Transcript analysis

Each plugin contributes evidence to the project database.

---

## Step 5 – Fuse Evidence

Evidence from multiple plugins is combined into confidence scores.

Example:

Face Recognition

95%

Speaker Recognition

98%

Transcript Analysis

84%

Overall Confidence

97%

No plugin is considered the absolute source of truth.

---

## Step 6 – Review (Optional)

If requested, EchoForge presents uncertain cases.

Examples:

* Unknown speaker
* Ambiguous face
* Low confidence transcription
* Multiple possible identities

User feedback becomes additional evidence.

---

## Step 7 – Extract Artifacts

After confidence thresholds are satisfied, EchoForge extracts artifacts.

Examples include:

* Audio clips
* Video clips
* Frame images
* Waveforms
* Metadata

Artifacts remain linked to their original source media.

---

## Step 8 – Export Datasets

Exporters convert artifacts into formats suitable for downstream applications.

Examples:

* Style-Bert-VITS2
* Fish Speech
* Coqui TTS
* RVC
* Custom JSON
* CSV
* Future formats

---

# Processing Pipeline

```
Source Media

↓

Discovery

↓

Evidence Generation

↓

Human Review (Optional)

↓

Evidence Fusion

↓

Artifact Extraction

↓

Conditioning

↓

Quality Control

↓

Dataset Export


```

---

# Local-First Philosophy

Every stage should have an open-source implementation.

Examples include:

* FFmpeg
* Whisper
* Faster-Whisper
* pyannote
* InsightFace

Users may optionally configure commercial APIs or cloud services.

The framework itself remains fully functional without them.

---

# Project Goals

EchoForge is intended to become a general-purpose framework for multimodal speech dataset generation.

The project is not limited to:

* Text-to-Speech
* Movies
* Actors

Future applications may include:

* Speech recognition
* Speaker recognition
* Linguistic research
* Voice conversion
* Emotion recognition
* Subtitle generation
* Multimedia indexing

---

# Guiding Principle

EchoForge does not attempt to determine absolute truth.

Instead, it gathers independent evidence, measures confidence, preserves intermediate results, and enables users to build high-quality datasets through reproducible, transparent processing pipelines.

Maintainer:
    EchoForge Contributors

Repository:
    https://github.com/chronomicron/echoforge

This document is part of the EchoForge project and should be kept in
sync with the implementation whenever possible.
