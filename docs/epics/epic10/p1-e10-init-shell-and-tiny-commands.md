# Phase 1 Epic 10: Init, Shell, and Tiny Commands

## Purpose

Boot Aaronix into a minimal userspace operating loop: an init task starts a shell-like prompt, and the shell runs a few tiny commands.

## Human-Visible Result

The user sees a prompt and can run a short command set such as `help`, `echo`, `ls`, `cat`, and `run`.

## Learning Goal

Learn how earlier primitives combine into something that feels like an operating system: boot, output, tasks, syscalls, IPC, and named files.

## MINIX Comparison

MINIX provides real userspace utilities and process/filesystem behavior. Aaronix should model the spirit of that separation while keeping the first command environment intentionally small.

## Rust Design Focus

Keep command parsing simple and explicit. Commands may begin as built-ins and move to separate userspace programs only when the loader and filesystem make that useful.

Avoid full shell grammar, pipes, redirection, permissions, and environment variables unless a Phase 1 milestone explicitly needs them.

## Decisions Required Now

- Init responsibility.
- Shell input/output path.
- Built-in command list.
- Whether `run` starts an embedded payload or a filesystem-loaded program first.

## Deferred

- POSIX shell compatibility.
- Pipes and redirection.
- Environment variables.
- Login/session management.
- Filesystem writes.
- Package management.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 10.1 | Init task starts the command loop. | Emulator log or screenshot. |
| 10.2 | `help` and `echo` work. | Interactive transcript. |
| 10.3 | `ls` and `cat` use the tiny filesystem. | Interactive transcript. |
| 10.4 | `run` launches a tiny program if program loading is ready. | Interactive transcript and exit status. |

## Test Strategy

Use command parser tests on the host and emulator transcripts for end-to-end behavior.

## Next Unlock

Epic 11 can package Phase 1 as a coherent boot-to-shell course checkpoint.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0003](../../arch/adrs/0003-kernel-userspace-and-tooling-boundaries.md)
- [ADR-0007](../../arch/adrs/0007-contract-tests-for-observable-boundaries.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
