# Aaronix

Aaronix is a Rust-first implementation of MINIX, built as close as practical to the original source and intent while using idiomatic Rust for safety, clarity, testability, and teaching value.

The project is currently in documentation and Phase 1 planning. The immediate goal is to turn the Phase 1 epic sequence into execution PM, starting with the development workbench and evidence loop.

## Project Direction

Aaronix is not a line-by-line C-to-Rust translation. Each subsystem should start with a MINIX source study, then an Aaronix design decision, then a small implementation step that unlocks the next step.

Current phase map:

| Phase | Scope |
| --- | --- |
| Phase 1 | Basic MINIX-inspired OS foundation, organized as a depth-first boot-to-shell learning sequence. |
| Phase 1.5 | Rust SNES emulator showcase as a userspace/demo product, Linux-first, no bundled commercial ROMs. |
| Phase 2 | Linux binary compatibility. |
| Phase 3 | Native Windows executable runtime. |
| Phase 4 | Minimal visual GUI. |

## Start Here

Read these in order before coding:

1. [Project vision](docs/vision.md)
2. [Engineering mandate](docs/Engineering.md)
3. [Phase 1 overview](docs/epics/p1-overview.md)
4. [Development environment setup](docs/setup/development-environment.md)
5. [Test environment setup](docs/setup/test-environment.md)
6. [Architecture Decision Records](docs/arch/adrs/index_adrs.md)
7. [Epics](docs/epics/index_epics.md)
8. [Project management](docs/pm/index_pm.md)

## How To Start Coding

Aaronix implementation should follow the docs, not race ahead of them.

1. Pick a Phase 1 task from a PM file once Phase 1 PM exists.
2. Read the linked epic and ADRs.
3. If the task requires a new architecture decision, add or update an ADR before coding.
4. Keep the implementation to the smallest testable capability that unlocks the next step.
5. Add or update tests for the new boundary.
6. Run the relevant host and emulator checks.
7. Save milestone evidence using the [virtualization evidence loop](docs/setup/virtualization-evidence-loop.md).
8. Update the PM file with results, gaps, and follow-up tasks.

The project owner writes the Rust implementation. Assistants collaborate through documentation, design, coaching, review, and planning unless explicitly asked to edit code.

## Repository Map

| Path | Purpose |
| --- | --- |
| [docs/vision.md](docs/vision.md) | Project definition, phase map, documentation graph, and course direction. |
| [docs/Engineering.md](docs/Engineering.md) | Engineering rules for Rust-first OS work. |
| [docs/setup/](docs/setup/index_setup.md) | Development setup, test setup, and emulator evidence collection. |
| [docs/arch/](docs/arch/index_arch.md) | Architecture index, ADRs, diagrams, and resources. |
| [docs/epics/](docs/epics/index_epics.md) | Concept and design breakdown by phase. |
| [docs/epics/p1-overview.md](docs/epics/p1-overview.md) | Phase 1 depth-first learning sequence. |
| [docs/pm/](docs/pm/index_pm.md) | Milestones, tasks, subtasks, and acceptance state. |
| [docs/research/](docs/research/index_research.md) | MINIX source study, tradeoffs, licensing, and experiments. |
| [docs/resources/](docs/resources/index_resources.md) | Project-wide resources, including the MINIX book PDF. |

## Current Status

The repository currently contains the documentation spine, initial ADRs, setup docs, and Phase 1 epic sequence. Phase 1 PM is the next planned step, beginning with [Epic 00: Workbench and Evidence Loop](docs/epics/epic00/p1-e00-workbench-and-evidence-loop.md).

There is not yet a committed Rust workspace, boot target, bootloader decision, emulator command, or CI contract. Those decisions should be made in Phase 1 bootstrapping ADRs and PM milestones.

## License And Asset Note

Do not add third-party code, ROMs, firmware, images, binaries, PDFs, or other assets unless redistribution rights are clear and documented. Phase 1.5 SNES work must not bundle commercial ROMs without explicit rights.
