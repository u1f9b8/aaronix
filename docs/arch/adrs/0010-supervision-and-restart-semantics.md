---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0068 and ADR-0115
---

# ADR-0010 - Supervision and Restart Semantics

## Context

Argaile had service lifecycle and graceful-restart ADRs for operating multiple host services. Aaronix is an operating system, so the equivalent question is not how a web service restarts under systemd. It is how the kernel, system processes, drivers, servers, init, and user programs start, fail, stop, and recover.

MINIX's architecture makes this especially relevant because it separates responsibilities across kernel mechanisms, system tasks, process management, filesystem service, drivers, and userland. Aaronix should preserve that supervision mindset, but Phase 1 must still begin with the smallest bootable path.

## Decision

Aaronix treats process and service lifecycle as an explicit design surface.

Phase 1 progression:

1. Early boot may halt or panic on fatal errors, but the failure must be visible.
2. Once an initial scheduler exists, process states and transitions become documented behavior.
3. Once init/userspace exists, startup order becomes a contract.
4. Once IPC exists, long-lived servers and drivers must define what happens on crash, restart, and message loss.
5. Once storage exists, recovery behavior can include persisted state.

Rules:

1. Critical lifecycle behavior belongs in epics and PM milestones, not only in code comments.
2. A process/server that cannot be restarted safely must say so explicitly.
3. A restartable server must define what state survives and what state is rebuilt.
4. Kernel-owned failure modes must be deterministic: panic, halt, restart task, kill process, or report error.
5. User-visible commands or tests should expose process/server state as soon as such state exists.
6. Host-level supervision of emulator runs and build tools is separate from Aaronix runtime supervision.

## Consequences

Good:

- Process lifecycle becomes teachable instead of incidental.
- MINIX-style separation can be introduced incrementally.
- Later compatibility runtimes have a lifecycle model to plug into.

Costs:

- Some lifecycle decisions will be deferred until the kernel has enough machinery to make them meaningful.
- Restart semantics can add complexity to early subsystem APIs if introduced too soon.

## Deferral

Automatic restart of failed servers is deferred until Aaronix has IPC and at least one long-lived userspace or server process whose restart can be tested.

Linux compatibility process semantics are deferred to Phase 2. Windows runtime process semantics are deferred to Phase 3. GUI session management is deferred to Phase 4.

## Aaronix Review

The Argaile ADRs were useful for asking what survives restart, what owns supervision, and how lifecycle actions become visible. All systemd, Windows service, web API, database, installer, polkit, product-service, and enterprise upgrade content was removed.
