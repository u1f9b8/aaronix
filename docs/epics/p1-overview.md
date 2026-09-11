# Phase 1 Overview: Depth-First Bootstrapping

## Purpose

Phase 1 builds the smallest useful Aaronix operating-system path: a bootable Rust kernel, visible diagnostics, enough memory and execution machinery to cross into userspace, and a tiny shell-like loop.

The sequence is intentionally depth-first. Each epic starts from the artifact produced by the previous epic and adds only the minimum code needed to make the next human-visible behavior possible.

## Phase 1 Rule

No constants avalanche.

Aaronix should not pre-port MINIX headers, POSIX constants, syscall tables, filesystem structures, or driver catalogs. A constant, type, module, crate, or helper belongs in the tree only when the current milestone uses it and the learner can explain why it is needed.

## File Layout

This overview stays at `docs/epics/p1-overview.md`.

Numbered Phase 1 epics live one level deeper:

- base epic: `docs/epics/epicNN/p1-eNN-*.md`
- first amendment: `docs/epics/epicNN/p1-eNNa-*.md`
- second amendment: `docs/epics/epicNN/p1-eNNb-*.md`

For example, Epic 10 starts with `docs/epics/epic10/p1-e10-init-shell-and-tiny-commands.md`. If that subject needs focused expansion later, new documents should be added beside it as `p1-e10a-*`, then `p1-e10b-*`, and so on.

## Learning Shape

Each epic should be usable as a short course module:

- one concept,
- one small coding surface,
- one MINIX comparison,
- one Rust design rationale,
- one visible result,
- one evidence record,
- and one clear unlock for the next epic.

## Epic Sequence

| Order | Epic | Human-visible result | Unlocks |
| --- | --- | --- | --- |
| 00 | [Workbench and Evidence Loop](epic00/p1-e00-workbench-and-evidence-loop.md) | A reproducible local build/test/emulator command path, even before the kernel exists. | Trustworthy feedback for every later lesson. |
| 01 | [Bootable Rust Kernel Skeleton](epic01/p1-e01-bootable-rust-kernel-skeleton.md) | The emulator reaches a Rust kernel entry point and exits or halts in a controlled way. | A real execution target for diagnostics. |
| 02 | [Visible Kernel Output](epic02/p1-e02-visible-kernel-output.md) | A banner and panic message are visible through the chosen early output channel. | Human-readable debugging for kernel work. |
| 03 | [Boot Data and Memory Map](epic03/p1-e03-boot-data-and-memory-map.md) | The kernel reports the bootloader-provided memory map or equivalent boot facts. | Ground truth for memory allocation. |
| 04 | [Frame Allocation Basics](epic04/p1-e04-frame-allocation-basics.md) | The kernel allocates and reports a few physical frames without a heap. | Controlled memory ownership for later paging and tasks. |
| 05 | [Interrupts, Clock, and Kernel Pulse](epic05/p1-e05-interrupts-clock-and-kernel-pulse.md) | A timer tick or heartbeat proves asynchronous kernel events are arriving. | A timing foundation for scheduling. |
| 06 | [First Task Model and Scheduler](epic06/p1-e06-first-task-model-and-scheduler.md) | Two tiny kernel tasks yield and run in a visible order. | Execution structure before user mode. |
| 07 | [User Mode and First Syscall](epic07/p1-e07-user-mode-and-first-syscall.md) | A minimal userspace program asks the kernel to write text and exit. | A real kernel/userspace boundary. |
| 08 | [MINIX-Style IPC Seed](epic08/p1-e08-minix-style-ipc-seed.md) | A request/reply message flows between a user task and a tiny service. | Service-shaped OS growth instead of kernel sprawl. |
| 09 | [Tiny Filesystem and Program Image](epic09/p1-e09-tiny-filesystem-and-program-image.md) | The system lists or reads files from a tiny read-only image. | A source for programs and shell-visible state. |
| 10 | [Init, Shell, and Tiny Commands](epic10/p1-e10-init-shell-and-tiny-commands.md) | Aaronix boots to a prompt with simple commands such as `help`, `echo`, `ls`, and `run`. | A usable Phase 1 operating loop. |
| 11 | [Phase 1 Capstone](epic11/p1-e11-phase-1-capstone.md) | A recorded boot-to-shell demo with tests, evidence, and updated PM state. | A stable base for Phase 1.5. |

## Source Study Discipline

The MINIX book source listing starts with broad C headers. Aaronix should study those headers when the current epic reaches their concern, not before.

Examples:

- early boot does not need `termios` constants,
- first serial output does not need a full TTY subsystem,
- first syscall does not need a complete POSIX syscall table,
- first file listing does not need the full MINIX filesystem surface,
- and a kernel task demo does not need Linux compatibility process semantics.

## PM Handoff

Each epic should receive a PM file only when it becomes the next implementation focus. PM files should split the epic into testable milestones and update status after implementation.

The first PM target should be Epic 00, because the workbench and evidence loop make every later step measurable.

## Related Documents

- [Vision](../vision.md)
- [Engineering Mandate](../Engineering.md)
- [MINIX Source Listing Initial Map](../research/minix-source-listing-initial-map.md)
- [ADR-0013: Depth-First Phase 1 Learning Sequence](../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
- [Project Management](../pm/index_pm.md)
