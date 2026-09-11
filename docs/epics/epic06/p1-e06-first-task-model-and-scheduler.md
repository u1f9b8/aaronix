# Phase 1 Epic 06: First Task Model and Scheduler

## Purpose

Introduce the smallest task representation and scheduler needed to run more than one unit of kernel work.

## Human-Visible Result

Two tiny tasks visibly alternate, yield, or complete in a predictable order.

## Learning Goal

Learn what a schedulable task is before adding user programs, process managers, or POSIX process semantics.

## MINIX Comparison

MINIX has process tables and scheduling integrated with IPC, drivers, and system tasks. Aaronix should start with a minimal task model that teaches execution state without pulling in the full MINIX process surface.

## Rust Design Focus

Represent task state explicitly and make scheduler ownership clear. Begin with cooperative scheduling unless a previous interrupt milestone gives a strong reason to introduce preemption now.

No `fork`, `exec`, signals, permissions, or file descriptors are needed in this epic.

## Decisions Required Now

- Minimal task state enum.
- Whether the first scheduler is cooperative or timer-driven.
- How tasks report completion or failure.

## Deferred

- User-mode process table.
- Process hierarchy.
- Preemptive fairness policy.
- Signals.
- Resource limits.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 06.1 | Minimal task type exists. | Source review. |
| 06.2 | Scheduler can run one task. | Host or emulator test. |
| 06.3 | Scheduler can alternate two tasks. | Emulator log. |
| 06.4 | Task completion is visible and testable. | Test output and log. |

## Test Strategy

Test scheduling decisions with host fakes where possible. Use the emulator to prove the scheduler runs in the kernel context.

## Next Unlock

Epic 07 can run a small userspace program because the kernel now has an execution model to attach it to.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0010](../../arch/adrs/0010-supervision-and-restart-semantics.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
