# MINIX Source Listing Initial Map

## Purpose

This note records the first source-study checkpoint for organizing Aaronix Phase 1. It is not a request to port everything in order.

The MINIX implementation listing in [OperatingSystems-Design-and-Implementation-3ed.pdf](../resources/OperatingSystems-Design-and-Implementation-3ed.pdf) begins around PDF page 658. The first material is a set of public headers and constants rather than the smallest possible boot story.

## Initial Observation

The opening listing includes headers such as:

- `include/ansi.h`
- `include/limits.h`
- `include/errno.h`
- `include/unistd.h`
- `include/signal.h`
- `include/fcntl.h`
- `include/termios.h`
- `include/timers.h`
- `include/sys/types.h`

This is useful as a map of the eventual C system surface, but it should not become Aaronix's starting implementation order.

## Aaronix Interpretation

Aaronix should use the MINIX listing to ask focused questions at each epic:

- What problem was this MINIX source solving?
- Which part of that problem is needed for the current milestone?
- Which constants, layouts, or semantics must be stable now?
- Which items are only needed by future system calls, terminals, filesystems, signals, or compatibility work?

For example, `errno.h` shows that MINIX has an internal/external error-number distinction. Aaronix should study that when defining syscall or IPC error behavior, not during the first bootable kernel lesson.

Similarly, `termios.h` is relevant when Aaronix has a terminal driver or shell I/O path. It is not needed for a first serial banner.

## Simulator Note

The same appendix discusses using virtual machines and simulators such as QEMU, Bochs, and VMware for MINIX work. Aaronix should make emulator execution and evidence capture part of Phase 1 from the beginning, because it lets every milestone produce a visible result without relying on physical hardware.

## Working Rule

Treat the MINIX headers as source-study inventory. Implement Aaronix constants, types, and modules only when a current epic needs them and has a test or visible demonstration for them.

## Related Documents

- [Vision](../vision.md)
- [Phase 1 Overview](../epics/p1-overview.md)
- [ADR-0013: Depth-First Phase 1 Learning Sequence](../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
- [Test Environment](../setup/test-environment.md)
