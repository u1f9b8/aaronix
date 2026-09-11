# Phase 1 Epic 04: Frame Allocation Basics

## Purpose

Create the first minimal physical-frame allocator from the validated memory map.

## Human-Visible Result

Aaronix allocates a few frames, reports their addresses, and proves it does not allocate reserved memory.

## Learning Goal

Learn the difference between knowing memory exists and safely claiming ownership of part of it.

## MINIX Comparison

MINIX has mature memory-management code and system-wide assumptions. Aaronix should begin with a tiny frame allocator that supports only the next steps and can be tested without a full process manager.

## Rust Design Focus

Use Rust types to make frame identity and usable ranges explicit. Unsafe access, if required, should stay close to the hardware boundary and be explained in the epic or ADR.

This epic should not introduce a general heap unless a milestone proves it is needed immediately.

## Decisions Required Now

- Frame size for the first target.
- Frame address representation.
- Allocator ownership model.
- Failure behavior when frames run out.

## Deferred

- Kernel heap allocator.
- Virtual memory manager.
- Page-table abstraction.
- Swapping.
- Per-process address spaces.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 04.1 | Frame type and usable-range iterator exist. | Host tests. |
| 04.2 | First allocator returns deterministic frames. | Host tests. |
| 04.3 | Kernel reports sample allocations. | Emulator log. |
| 04.4 | Reserved-memory exclusion is tested. | Host test output. |

## Test Strategy

Favor host tests with fake memory maps for allocator behavior, then confirm one emulator run prints real allocations.

## Next Unlock

Epic 05 can set up interrupt and timing structures with clearer memory ownership assumptions.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0004](../../arch/adrs/0004-simulation-and-test-double-discipline.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
