---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0001
---

# ADR-0001 - Record Architecture Decisions

## Context

Aaronix aims to implement MINIX as close as possible to the original source and intent, while using Rust as much as practical. That creates recurring architectural choices:

- when to follow MINIX literally,
- when to translate a C/header/#define pattern into a Rust type or module,
- when a small amount of assembly is justified,
- when a decision must be made now to unblock Phase 1,
- and when a decision should be deferred until a later phase has real pressure.

Without written decision records, the project will lose why a Rust shape differs from MINIX C, why a milestone was ordered in a particular way, and which future compatibility commitments were intentionally delayed.

Aaronix is also intended to support a future course on building an OS in Rust. The decision trail is therefore not only project governance; it is teaching material.

## Decision

Every meaningful architectural decision is recorded as an ADR under `docs/arch/adrs/`.

ADRs use this structure:

- **Status**: Proposed, Accepted, Superseded by ADR-NNNN, or Rejected.
- **Date**: ISO date, `yyyy-mm-dd`.
- **Phase**: the phase where the decision applies.
- **Context**: the forces at play.
- **Decision**: what Aaronix will do.
- **Consequences**: what becomes easier and what becomes harder.
- **Deferral**: what is intentionally not decided yet, and until which phase or milestone.
- **MINIX comparison**: when relevant, how the original MINIX source/book handles the concern.
- **Rust rationale**: when relevant, why the Aaronix Rust design matches or differs from the C design.

ADRs are numbered monotonically using four digits and are never renumbered. Superseded ADRs stay in place and point to the replacing ADR.

A decision is meaningful when it:

- changes kernel/userspace/module boundaries,
- chooses a boot, architecture, filesystem, process, memory, driver, syscall, or IPC strategy,
- introduces or freezes an ABI,
- translates a MINIX C idiom into a Rust idiom,
- changes the order of Phase 1 milestones,
- changes a testability rule,
- or imposes a constraint future contributors would trip over without context.

## Consequences

Good:

- Future work can cite decisions instead of rediscovering them.
- MINIX-to-Rust translation choices remain teachable.
- Deferrals stay explicit instead of becoming accidental gaps.
- Phase 1 epics and PM files can point to the architecture decisions they implement.

Costs:

- Design work has a small documentation tax.
- A decision that seems obvious while coding may still need a short ADR if it shapes later implementation.

## Deferral

This ADR does not decide any kernel design. It decides only the project rule for recording such decisions.

## Aaronix Review

The Argaile source ADR was useful and mostly reusable. Argaile-specific references to product replaceability and multi-agent business development were removed. Aaronix-specific requirements were added: MINIX comparison, Rust rationale, phase deferral, and course value.
