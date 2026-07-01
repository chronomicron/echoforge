<!--
===============================================================================
EchoForge Architecture Diagrams
-------------------------------------------------------------------------------

Purpose:

Visual documentation of the EchoForge framework.

These diagrams complement the textual documentation found in
architecture.md, plugin_api.md, and data_model.md.

The diagrams should be treated as architectural specifications.
Whenever the implementation changes, these diagrams should be updated
to remain consistent with the source code.

===============================================================================
-->

## 1. System Architecture

```mermaid
flowchart TB

    User([User])

    Engine["EchoForge Engine"]

    Orchestrator["Pipeline Orchestrator"]

    Scheduler["Job Scheduler"]

    EventBus["Event System"]

    User --> Engine

    Engine --> Orchestrator
    Engine --> Scheduler
    Engine --> EventBus

    Scheduler --> P1["Video Plugin"]
    Scheduler --> P2["Audio Plugin"]
    Scheduler --> P3["Subtitle Plugin"]

    P1 --> EventBus
    P2 --> EventBus
    P3 --> EventBus

    EventBus --> Engine

    Engine --> Project["Project Metadata"]
    Engine --> Database["Database"]
    Engine --> Artifacts["Artifacts"]

    click Engine "architecture.md"
    click Project "data_model.md"
    click P1 "plugin_api.md"
```

## 2. Core Data Model

```mermaid
classDiagram

class Project

class Media

class Target

class Artifact

class Evidence

class ExecutionContext

class PluginResult

Project --> Media
Project --> Target
Project --> Artifact

ExecutionContext --> Target
ExecutionContext --> Media

PluginResult --> Artifact
PluginResult --> Evidence
```

## 3. Plugin Execution Sequence

```mermaid
sequenceDiagram

participant Engine

participant Plugin

Engine->>Plugin: Execute(Context)

loop Processing

Plugin-->>Engine: Progress Event

end

Plugin-->>Engine: PluginResult

Engine->>Engine: Store Artifacts

Engine->>Engine: Update Metadata

Engine->>Engine: Schedule Next Plugin
```

## 4. Processing Dependency Graph

```mermaid
flowchart TB

    Media["Media Resource"]

    Video["Video Analysis"]

    Audio["Audio Analysis"]

    Subtitle["Subtitle Analysis"]

    Face["Face Recognition"]

    Speaker["Speaker Recognition"]

    Transcript["Transcript Interpretation"]

    Fusion["Evidence Fusion"]

    Extract["Clip Extraction"]

    Conditioning["Audio Conditioning"]

    Dataset["Dataset Export"]

    Media --> Video
    Media --> Audio
    Media --> Subtitle

    Video --> Face
    Audio --> Speaker
    Subtitle --> Transcript

    Face --> Fusion
    Speaker --> Fusion
    Transcript --> Fusion

    Fusion --> Extract

    Extract --> Conditioning

    Conditioning --> Dataset

    click Dataset "toolchain.md"
```

project/

├── project.yaml

├── database/

├── metadata/

├── cache/

├── artifacts/

├── reports/

├── logs/

└── exports/
