# EchoForge Architecture

> **Status:** Design Document (Draft)
>
> This document describes the architectural vision of EchoForge. It serves as a long-term reference for contributors and is intended to evolve alongside the project.

---

# Project Philosophy

EchoForge is not a wrapper around existing AI tools.

It is an orchestration framework for building reproducible, high-quality speech datasets from audiovisual media.

The framework is responsible for coordinating independent processing stages, preserving intermediate data, managing metadata, and combining evidence from multiple analysis sources.

Individual AI tools are interchangeable plugins.

---

# Core Principles

## Modular

Every processing stage is independent.

Each module receives clearly defined inputs and produces clearly defined outputs.

Any module should be replaceable without requiring changes to the remainder of the pipeline.

---

## Local First

EchoForge is designed to operate completely offline.

Every major processing stage should have a fully open-source implementation.

Commercial APIs may optionally be configured by users but are never required.

---

## Preserve Everything

Original media is never modified.

Intermediate processing results are never discarded.

Metadata should always be preserved whenever practical.

Storage is inexpensive.

Source media and extracted information are valuable.

The project motto is:

> **Data is expensive. Storage is cheap. Preserve everything.**

---

## Evidence-Based

EchoForge never relies on a single source of truth.

Instead, independent plugins contribute evidence.

Examples include:

* Face recognition
* Speaker recognition
* Speech transcription
* Subtitle analysis
* Voice identification
* Future plugins

Evidence is combined probabilistically rather than deterministically.

---

## Human-Assisted

Automation is preferred.

Human interaction is optional.

When user input is requested, EchoForge should ask only those questions that provide the greatest improvement to the overall confidence of the dataset.

---

# High-Level Architecture

```text
                 Project

                    │

                    ▼

             SQLite Database

                    │

     ┌──────────────┼──────────────┐

     ▼              ▼              ▼

 Video         Audio         Transcript

     ▼              ▼              ▼

 Evidence     Evidence      Evidence

      └─────────────┬─────────────┘

                    ▼

           Evidence Fusion Engine

                    ▼

             Candidate Artifacts

                    ▼

             Dataset Exporters
```

---

# Layered Design

EchoForge consists of four logical layers.

```text
GUI

↓

CLI

↓

Core Framework

↓

Plugins
```

The GUI should never contain processing logic.

The CLI is the primary interface.

The Core Framework coordinates execution.

Plugins perform all analysis.

---

# Core Objects

The framework is built around several fundamental objects.

| Object        | Description                                    |
| ------------- | ---------------------------------------------- |
| Project       | Self-contained workspace                       |
| Media         | Input video or audio source                    |
| Segment       | Time interval within media                     |
| Evidence      | Probabilistic observation produced by a plugin |
| Person        | Known identity or target                       |
| Speaker       | Speaker cluster or identified voice            |
| Artifact      | Extracted output such as WAV, MP4, or image    |
| Plugin        | Independent processing module                  |
| Pipeline      | Ordered collection of processing stages        |
| Exporter      | Converts artifacts into dataset formats        |
| Task          | Executable processing operation                |
| Configuration | Project settings                               |

---

# Suggested Repository Layout

```text
EchoForge/

.github/

docs/

examples/

tests/

echoforge/

    core/

    plugins/

    database/

    utils/

pyproject.toml

README.md

LICENSE
```

---

# Development Standards

## Python

* Python 3.12+

---

## Package Management

Recommended:

* uv

---

## Formatting

Recommended:

* Ruff

---

## Type Checking

Recommended:

* Pyright

---

## Documentation

Functions should use Google-style docstrings.

Every public API should include type hints.

---

# Plugin Architecture

Plugins should perform a single responsibility.

Examples include:

* Audio extraction
* Speech transcription
* Speaker diarization
* Face detection
* Face tracking
* Voice identification
* Subtitle import
* Audio cleanup
* Dataset export

Each plugin should operate independently and communicate only through the shared project data model.

---

# Pipeline Philosophy

Rather than embedding processing logic into a monolithic workflow, EchoForge executes a configurable sequence of stages.

Typical processing flow:

```text
Source Media

↓

Index

↓

Evidence Generation

↓

Evidence Fusion

↓

Human Review (Optional)

↓

Artifact Extraction

↓

Dataset Export
```

Future versions may allow custom pipeline definitions through configuration files.

---

# Project Structure

Every EchoForge project is self-contained.

Example:

```text
project/

    echoforge.yaml

    source/

    cache/

    dataset/

    logs/

    reports/
```

This makes projects portable, reproducible, and easy to share.

---

# Versioning Strategy

EchoForge follows Semantic Versioning.

Early development is expected to evolve through several iterations before reaching version 1.0.

Initial milestones:

* v0.1 – Framework skeleton
* v0.2 – Media scanning
* v0.3 – Transcription
* v0.4 – Speaker diarization
* v0.5 – Face detection
* v0.6 – Evidence fusion
* v0.7 – Artifact extraction
* v0.8 – Dataset exporters
* v0.9 – Graphical interface
* v1.0 – Complete end-to-end pipeline

---

# Long-Term Vision

EchoForge is intended to become a general-purpose framework for multimodal dataset generation.

Although the initial focus is speech extraction for Text-to-Speech applications, the architecture is designed to support future use cases including:

* Automatic Speech Recognition (ASR)
* Speaker Recognition
* Voice Conversion
* Emotion Recognition
* Subtitle Generation
* Linguistic Research
* Multimedia Indexing
* Future multimodal AI workflows

EchoForge is designed to remain tool-agnostic.

As new AI models become available, contributors should be able to replace or improve individual plugins without modifying the core framework.

The framework itself—not any individual AI model—is the long-term product.
