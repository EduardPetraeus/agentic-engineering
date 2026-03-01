# Architecture Diagram

Visual representation of how the five repositories in the agentic engineering ecosystem connect.

## Ecosystem Flow

```mermaid
graph TD
    A[agentic-engineering<br/>Umbrella Docs] --> B[ai-governance-framework<br/>What may agents do?]
    A --> C[ai-project-management<br/>What to do?]
    A --> D[ai-engineering-standards<br/>How to do it?]
    B --> E[ai-project-templates<br/>Scaffolder]
    C --> E
    D --> E
    E --> F[Your Project Repo]
    F -->|reads| B
    F -->|reads| C
    F -->|reads| D
```

## Project Repo Internal Structure

```mermaid
graph LR
    subgraph "Your Project Repo"
        CM[CLAUDE.md<br/>Constitution]
        T[tasks/<br/>Work Items]
        S[standards/<br/>Quality Rules]
        SRC[src/<br/>Source Code]
        TST[tests/<br/>Test Suite]
    end

    subgraph "Agent Session"
        AG[AI Agent]
    end

    AG -->|reads at session start| CM
    AG -->|picks up work from| T
    AG -->|follows rules from| S
    AG -->|writes code to| SRC
    AG -->|writes tests to| TST

    CM -.->|derived from| GOV[ai-governance-framework]
    T -.->|format from| PM[ai-project-management]
    S -.->|content from| ES[ai-engineering-standards]
```

## Governance Layer Detail

```mermaid
graph TB
    subgraph "7-Layer Governance Model"
        L1[Layer 1: Constitution<br/>CLAUDE.md]
        L2[Layer 2: Orchestration<br/>Agent Definitions]
        L3[Layer 3: Enforcement<br/>Pre-commit Hooks + CI]
        L4[Layer 4: Observability<br/>Logging + Health Score]
        L5[Layer 5: Knowledge<br/>Patterns + Standards]
        L6[Layer 6: Team Governance<br/>Multi-agent Rules]
        L7[Layer 7: Evolution<br/>Version + Migration]
    end

    L1 --> L2 --> L3 --> L4 --> L5 --> L6 --> L7
```
