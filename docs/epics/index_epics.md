# Aaronix Epics

Epics contain concept and design work. They explain what Aaronix will build, why the work is ordered that way, how MINIX handled the concern, how Aaronix should express it in Rust, which ADRs govern it, and what testable milestone the work unlocks.

## Phase Map

| Phase | Epic status | Notes |
| --- | --- | --- |
| Phase 1 | [Depth-first sequence drafted](p1-overview.md) | Current focus: basic bootable MINIX-inspired OS foundation. Each epic is a small learning step with a visible result and a direct unlock for the next step. |
| Phase 1.5 | [Rust SNES Emulator Showcase](phase-1-5-rust-snes-emulator-showcase.md) | Deferred userspace showcase after Phase 1 exposes enough runtime surface. Governed by [ADR-0011](../arch/adrs/0011-rust-snes-emulator-showcase-phase-1-5.md). |
| Phase 2 | Deferred | Linux binary compatibility. Requires explicit ABI discipline from Phase 1. |
| Phase 3 | Deferred | Native Windows executable runtime. Requires compatibility-runtime architecture decisions. |
| Phase 4 | Deferred | Minimal visual GUI. Requires stable userspace and graphics/input foundations. |

## Folder Convention

Phase overview files may stay directly under `docs/epics/`, such as [p1-overview.md](p1-overview.md).

Numbered Phase 1 epics live in folders named `epicNN/`. The base epic file uses `p1-eNN-*`, and future amendments use the same folder with letter suffixes:

- base: `docs/epics/epic10/p1-e10-init-shell-and-tiny-commands.md`
- first amendment: `docs/epics/epic10/p1-e10a-*.md`
- second amendment: `docs/epics/epic10/p1-e10b-*.md`

This keeps top-level epic navigation small while allowing each subject to grow.

## Phase 1 Sequence

Phase 1 follows [ADR-0013](../arch/adrs/0013-depth-first-phase-1-learning-sequence.md): no broad pre-porting of MINIX headers, constants, syscall tables, or unused future scaffolding. Each epic adds only the code and concepts needed for the next human-visible result.

| Order | Epic | Primary lesson |
| --- | --- | --- |
| 00 | [Workbench and Evidence Loop](epic00/p1-e00-workbench-and-evidence-loop.md) | Make the build, run, and evidence loop trustworthy before kernel work starts. |
| 01 | [Bootable Rust Kernel Skeleton](epic01/p1-e01-bootable-rust-kernel-skeleton.md) | Reach Rust code from the boot environment. |
| 02 | [Visible Kernel Output](epic02/p1-e02-visible-kernel-output.md) | Give the kernel a human-readable voice. |
| 03 | [Boot Data and Memory Map](epic03/p1-e03-boot-data-and-memory-map.md) | Read facts from the boot environment instead of guessing. |
| 04 | [Frame Allocation Basics](epic04/p1-e04-frame-allocation-basics.md) | Claim physical memory safely and minimally. |
| 05 | [Interrupts, Clock, and Kernel Pulse](epic05/p1-e05-interrupts-clock-and-kernel-pulse.md) | Handle asynchronous machine events. |
| 06 | [First Task Model and Scheduler](epic06/p1-e06-first-task-model-and-scheduler.md) | Run more than one unit of work. |
| 07 | [User Mode and First Syscall](epic07/p1-e07-user-mode-and-first-syscall.md) | Cross the first kernel/userspace boundary. |
| 08 | [MINIX-Style IPC Seed](epic08/p1-e08-minix-style-ipc-seed.md) | Introduce request/reply service structure. |
| 09 | [Tiny Filesystem and Program Image](epic09/p1-e09-tiny-filesystem-and-program-image.md) | Move from hard-coded behavior to named bytes. |
| 10 | [Init, Shell, and Tiny Commands](epic10/p1-e10-init-shell-and-tiny-commands.md) | Combine the primitives into a usable command loop. |
| 11 | [Phase 1 Capstone](epic11/p1-e11-phase-1-capstone.md) | Package the boot-to-shell path as a course checkpoint. |

## Epic Template Expectations

Each epic should include:

- scope and non-goals,
- MINIX comparison,
- Rust rationale,
- required ADRs,
- deferred decisions,
- milestone breakdown,
- test strategy,
- and human-visible demonstration criteria.

## Related Indexes

- [Vision](../vision.md)
- [Architecture](../arch/index_arch.md)
- [ADRs](../arch/adrs/index_adrs.md)
- [Project Management](../pm/index_pm.md)
- [Research](../research/index_research.md)
