# Output Template

Use this for architecture documents and interview take-home answers.

````markdown
# <System Name> Architecture

## Executive Summary

One paragraph describing the system, the main architecture choice, and the most important trade-off.

## Scope

### In Scope

- ...

### Out of Scope

- ...

### Assumptions

- ...

## Requirements

### Functional Requirements

- ...

### Non-Functional Requirements

- ...

## Capacity Estimate

| Dimension | Estimate | Design Impact |
|---|---:|---|
| Writes/sec average | | |
| Writes/sec peak | | |
| Reads/sec peak | | |
| Storage growth | | |
| Freshness target | | |

## High-Level Architecture

```mermaid
flowchart LR
```

## Tier Responsibilities

### Presentation Tier

- ...

### Application Tier

- ...

### Data Tier

- ...

## API and Event Contracts

### APIs

| Endpoint | Purpose | Notes |
|---|---|---|

### Events

| Event | Producer | Consumer | Idempotency Key | Ordering Key |
|---|---|---|---|---|

## Data Model

```mermaid
erDiagram
```

## Core Flows

### Happy Path

```mermaid
sequenceDiagram
```

### Failure / Retry Path

```mermaid
flowchart TD
```

## Scalability

- Current-load design:
- Growth path:
- Overkill for now:

## Reliability and Fault Tolerance

- No-loss strategy:
- Duplicate handling:
- Out-of-order handling:
- Recovery/replay:
- Backpressure:

## Security and Privacy

- Authentication:
- Authorization:
- Transport security:
- PII handling:
- Audit:

## Observability

- Metrics:
- Logs:
- Traces:
- Alerts:

## Trade-Offs

| Decision | Chosen | Rejected | Reason |
|---|---|---|---|

## Phased Work Breakdown

| Phase | Difficulty | Work | Output |
|---|---|---|---|

## Open Questions

- ...
````
