---
name: system-design-bytebytego
description: Comprehensive ByteByteGo-style system design workflow for scalable application architecture. Use when designing distributed systems, three-tier architectures, high-scale backends, API gateways, queues, caches, databases, real-time updates, reliability, scalability, fault tolerance, observability, interview-ready diagrams, architecture decision records, or trade-off write-ups inspired by ByteByteGoHq/system-design-101.
---

# System Design ByteByteGo

## Overview

Use this skill to produce architecture work that is simple enough to explain in an interview and rigorous enough to guide implementation.

The style target is ByteByteGoHq/system-design-101: visual-first explanations, explicit trade-offs, back-of-the-envelope capacity estimates, simple starting architecture, then clear evolution paths for scale and reliability.

## Required First Step

For every non-trivial architecture task, load `references/system-design-workflow.md` before drafting the answer.

Then load extra references only when relevant:

- `references/pattern-library.md`: API gateway, load balancing, queues, caching, database scaling, realtime, event sourcing, resiliency.
- `references/diagram-playbook.md`: when the output needs Mermaid diagrams, flowcharts, sequence diagrams, ERDs, or C4-style diagrams.
- `references/scaling-reliability-rubric.md`: when the user asks for Netflix-scale, high availability, fault tolerance, low latency, peak traffic, multi-region, or correctness under failure.
- `references/output-template.md`: when writing a design document, ADR, take-home assignment answer, or long-form system design response.
- `references/source-notes.md`: when citing the ByteByteGo repository or explaining provenance.

## Operating Principles

- Start with requirements and constraints, not technology.
- Estimate load before choosing scale patterns.
- Prefer a simple, shippable architecture first, then show how it evolves.
- Use append-only raw events when business facts must be auditable or recomputable.
- Put a durable queue or stream between unreliable producers and state-changing workers.
- Make every retried write idempotent.
- Cache derived read models for speed, never as the source of truth.
- Name failure modes explicitly and design recovery for each one.
- Use diagrams to compress complexity, not decorate the document.
- Be honest when "Netflix-scale" patterns are aspirational rather than required by current traffic.

## Default Deliverable Shape

When the user asks for architecture, produce:

1. Scope and assumptions
2. Functional requirements
3. Non-functional requirements
4. Capacity estimate
5. High-level architecture diagram
6. Component responsibilities
7. API and event contracts
8. Data model
9. Main data flows
10. Failure handling
11. Scalability plan
12. Security and privacy notes
13. Observability
14. Trade-offs and rejected alternatives
15. Phased task breakdown

For short answers, compress the same structure. Do not skip capacity, failure modes, or trade-offs when correctness depends on them.

## ByteByteGo-Style Quality Bar

A good output should let a reader answer:

- What exact problem is this system solving?
- What is the current scale and future scale?
- Which component owns each responsibility?
- What happens when the network drops, a message duplicates, a worker crashes, or the database slows down?
- What data is source of truth, and what data is derived?
- Which reads are hot, and how are they made fast?
- What can be simplified for MVP?
- What must not be simplified because it protects correctness or data loss?

## Common Architecture Moves

- Three-tier system: presentation, application, data.
- Event-driven ingestion: producer, gateway, durable queue, idempotent worker, source-of-truth store, derived read model.
- CQRS-lite: write normalized/auditable facts, read denormalized summaries.
- Cache-aside: app checks cache, falls back to DB, refreshes cache.
- Push updates: WebSocket or SSE for realtime client updates, polling fallback.
- Reliability loop: local buffer, retry, dedupe, process, dead-letter, replay.
- Scale path: single service, stateless horizontal replicas, read replicas/cache, partitioning, stream processing, multi-region only when justified.

## Final Response Discipline

For interview artifacts, explicitly distinguish:

- **Must-have for the brief**
- **Pragmatic MVP**
- **Scale-ready extension**
- **Overkill for now**

This prevents architecture from looking either under-designed or performatively over-engineered.
