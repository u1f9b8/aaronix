# Phase 1 Epic 08: MINIX-Style IPC Seed

## Purpose

Introduce the smallest request/reply message path inspired by MINIX, so Aaronix can grow through services instead of placing every behavior directly in the kernel.

## Human-Visible Result

A user task sends a message to a tiny service, the service replies, and the result is visible in the output log.

## Learning Goal

Learn the core MINIX idea that operating-system behavior can be decomposed into cooperating tasks and services.

## MINIX Comparison

MINIX uses message passing between processes and servers as a central design mechanism. Aaronix should begin with one message type and one service, then expand only when later epics need more messages.

## Rust Design Focus

Create explicit message boundary types with stable layout only where needed. Kernel-internal representations and ABI-visible message layouts should stay separate.

Do not create a complete message catalog.

## Decisions Required Now

- First message layout.
- First service identity.
- Blocking or polling semantics for the first request/reply path.
- Error behavior for unknown message types.

## Deferred

- Full IPC matrix.
- Deadlock detection.
- Service restart policy.
- Filesystem server protocol.
- Process manager protocol.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 08.1 | First IPC message type defined. | Source review. |
| 08.2 | User task can send a request. | Emulator log. |
| 08.3 | Service can reply. | Emulator log. |
| 08.4 | Unknown message behavior is tested. | Host or emulator test. |

## Test Strategy

Use host tests for message encoding and routing logic where possible. Use emulator evidence for end-to-end request/reply.

## Next Unlock

Epic 09 can place filesystem behavior behind a service-like boundary instead of hard-coding every file operation into the kernel path.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0003](../../arch/adrs/0003-kernel-userspace-and-tooling-boundaries.md)
- [ADR-0009](../../arch/adrs/0009-explicit-and-versioned-abi-boundaries.md)
- [ADR-0010](../../arch/adrs/0010-supervision-and-restart-semantics.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
