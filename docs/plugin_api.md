<!--
===============================================================================
EchoForge Plugin API
-------------------------------------------------------------------------------

File:
    plugin_api.md

Purpose:
    Defines the interface contract between the EchoForge framework and all
    plugins.

    Every plugin, regardless of its purpose, should conform to this interface.
    The goal is to ensure that plugins remain interchangeable, predictable,
    and independent from one another.

    This document defines the conceptual API. The Python implementation may
    evolve, but the behavior described here should remain stable.

Status:
    Living Technical Specification

Created:
    2026-06-29

License:
    Apache License 2.0

===============================================================================
-->

# EchoForge Plugin API

## Philosophy

EchoForge is built around modular plugins.

Each plugin performs one clearly defined task.

Plugins communicate with the framework through standardized interfaces rather
than directly interacting with one another.

This allows plugins to be replaced, improved, or disabled without affecting
the overall architecture.

---

# Plugin Responsibilities

Every plugin should:

* Consume one or more project resources.
* Produce new information or new artifacts.
* Preserve existing artifacts.
* Append provenance rather than overwrite history.
* Report measurable results whenever possible.
* Fail gracefully.

Plugins should **never** modify the outputs of previous plugins.

---

# Standard Plugin Lifecycle

Every plugin follows the same conceptual execution sequence.

```text
Load

↓

Validate Inputs

↓

Execute

↓

Measure Results

↓

Generate Artifacts

↓

Append Metadata

↓

Report Status

↓

Finalize
```

The framework is responsible for coordinating execution.

Plugins are responsible only for their own task.

---

# Standard Inputs

Every plugin receives the following objects from the framework.

## Project

Contains:

* project configuration
* database connection
* workspace paths
* logging
* project settings

---

## Target

Describes the individual being searched for.

May include:

* display name
* aliases
* reference images
* reference audio
* user hints
* plugin-specific hints

---

## Resources

Resources may include:

* Media
* Artifacts
* Segments
* Evidence
* Metadata

Plugins consume only the resources required for their task.

---

## Configuration

Every plugin receives its own configuration.

Configuration may originate from:

* project defaults
* plugin defaults
* command-line options
* GUI settings

---

# Standard Outputs

A plugin may produce one or more of the following.

## Artifacts

Examples:

* extracted audio
* cleaned dialogue
* video clips
* transcripts

Artifacts are immutable.

---

## Evidence

Examples:

* face recognition confidence
* speaker confidence
* subtitle confidence

Evidence is probabilistic.

---

## Quality Metrics

Examples:

* SNR
* loudness
* speech probability
* clipping detection

Metrics should be objective whenever possible.

---

## Reports

Plugins should summarize their execution.

Possible information includes:

* execution time
* warnings
* skipped resources
* failures
* statistics

---

# Metadata

Plugins never overwrite existing metadata.

Instead they append a new processing record.

Each processing record should contain:

* plugin name
* plugin version
* execution timestamp
* configuration used
* input resources
* output resources
* measured statistics
* observations
* notes

This creates a complete provenance history for every artifact.

---

# Plugin Capabilities

Each plugin should declare its capabilities to the framework.

Examples include:

Consumes:

* Media
* Audio Artifact
* Video Artifact
* Evidence

Produces:

* Artifacts
* Evidence
* Quality Metrics
* Reports

The framework may use this information to construct execution pipelines.

---

# Plugin Independence

Plugins should never directly invoke other plugins.

Instead they communicate only through shared project resources.

This allows:

* optional plugins
* replacement implementations
* parallel execution
* easier testing

---

# Error Handling

Plugins should fail gracefully.

Failures should generate structured reports rather than terminate the entire
pipeline whenever possible.

Warnings and recoverable errors should be recorded as metadata.

---

# Human Interaction

Some plugins may require user assistance.

Examples include:

* selecting the target person
* confirming speaker identity
* correcting subtitle alignment
* resolving ambiguous detections

Human observations should be recorded as evidence alongside automated
analysis.

---

# Design Goals

Every EchoForge plugin should strive to be:

* Modular
* Deterministic
* Reproducible
* Replaceable
* Observable
* Measurable

A plugin should perform one task well and expose its results through
standardized artifacts, evidence, and metadata.

The framework coordinates the workflow.

The plugin performs the work.
