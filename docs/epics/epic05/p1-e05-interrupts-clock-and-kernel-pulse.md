# Phase 1 Epic 05: Interrupts, Clock, and Kernel Pulse

## Purpose

Introduce the smallest interrupt and clock path needed to prove the kernel can respond to asynchronous events.

## Human-Visible Result

Aaronix prints or records a heartbeat, tick count, or controlled interrupt event.

## Learning Goal

Learn why an OS needs interrupt handling before it can fairly schedule work or react to devices.

## MINIX Comparison

MINIX uses interrupts and clock handling as part of its process and driver model. Aaronix should first isolate the minimum interrupt setup and a clock pulse before expanding into scheduling.

## Rust Design Focus

Keep interrupt-table setup, handler entry, and shared state narrow. Any unsafe code should be localized and justified by the hardware interface.

Use only the constants required by the selected architecture and timer source.

## Decisions Required Now

- Interrupt descriptor strategy for the first architecture.
- Timer source for the first heartbeat.
- Shared state rules between handlers and normal kernel code.

## Deferred

- Device drivers beyond the clock source.
- Signal delivery.
- POSIX timers.
- Preemptive scheduling policy.
- Multiprocessor interrupt routing.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 05.1 | Interrupt entry strategy documented. | ADR or epic note. |
| 05.2 | Minimal interrupt table is installed. | Emulator evidence. |
| 05.3 | Timer event increments a visible counter. | Serial log or screenshot. |
| 05.4 | Fault path remains diagnosable. | Panic/fault evidence. |

## Test Strategy

Use emulator evidence for hardware interrupt behavior and host tests for pure state transitions.

## Next Unlock

Epic 06 can introduce tasks and scheduling on top of a working kernel pulse.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0008](../../arch/adrs/0008-observability-and-human-visible-progress.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
