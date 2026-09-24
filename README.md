# system-design-bytebytego

**A ByteByteGo-style system-design workflow for AI agents — pick patterns, score scalability, draw the diagram, and ship an Architecture Decision Record your team can actually review.**

Turns "design me a scalable backend" from a wall-of-text answer into a disciplined, repeatable workflow. The agent walks requirements through a pattern library, scores the design against a scaling & reliability rubric, drafts diagrams from a playbook, and finishes with a structured ADR including trade-offs.

## What you get

- `SKILL.md` — the master workflow
- `references/pattern-library.md` — load-balancing, caching, queueing, sharding, CDN, multi-region patterns with selection guidance
- `references/scaling-reliability-rubric.md` — score any design on scalability, fault tolerance, and observability
- `references/diagram-playbook.md` — interview-ready diagram conventions
- `references/output-template.md` + `system-design-workflow.md` — the deliverable format
- `agents/openai.yaml` — agent runtime profile

## Use it when

- Designing APIs, gateways, queues, caches, real-time updates, or data layers
- Reviewing an architecture for scale, resilience, or observability gaps
- You need a decision record with explicit trade-offs — not vibes

## Install

```bash
npx skills add fancyism/system-design-bytebytego
```

Or copy this folder into your agent's skill directory and start a new session.

> Inspired by the public notes in [ByteByteGoHq/system-design-101](https://github.com/ByteByteGoHq/system-design-101); all content here is an original workflow written for agent execution.

## License

[MIT](./LICENSE) — © 2026 Affan Samaeng
