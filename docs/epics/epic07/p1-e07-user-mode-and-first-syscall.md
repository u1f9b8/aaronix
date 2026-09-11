# Phase 1 Epic 07: User Mode and First Syscall

## Purpose

Cross the first real kernel/userspace boundary with one tiny userspace program and one syscall.

## Human-Visible Result

A userspace program asks the kernel to print a short message and then exits.

## Learning Goal

Learn why an OS separates user code from kernel code, and how a controlled boundary lets them cooperate.

## MINIX Comparison

MINIX exposes many POSIX-like headers, calls, and error conventions. Aaronix should study those ideas here, but implement only the first syscall or two required for the demonstration.

For example, MINIX error-number handling becomes relevant when the first syscall can fail. The whole `errno` catalog does not need to exist yet.

## Rust Design Focus

Define a narrow ABI boundary for the first syscall. Keep kernel domain types separate from userspace ABI values.

This epic should avoid a full libc, full syscall table, file descriptors, and process inheritance.

## Decisions Required Now

- First syscall calling convention.
- Minimal syscall numbers required now.
- User pointer validation strategy for the first call.
- Error representation for the first boundary.

## Deferred

- POSIX `unistd` surface.
- File descriptors beyond what the first call needs.
- `fork` and `exec`.
- Signals.
- Linux ABI compatibility.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 07.1 | Syscall ABI decision recorded. | ADR link. |
| 07.2 | Static userspace payload enters user mode. | Emulator evidence. |
| 07.3 | `write`-like syscall prints user text. | Captured output. |
| 07.4 | `exit`-like syscall returns task status. | Scheduler log. |

## Test Strategy

Use contract tests for pure ABI encoding and emulator evidence for the actual user/kernel transition.

## Next Unlock

Epic 08 can build MINIX-style request/reply behavior on top of a working user/kernel boundary.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0005](../../arch/adrs/0005-boundary-type-separation-policy.md)
- [ADR-0009](../../arch/adrs/0009-explicit-and-versioned-abi-boundaries.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
