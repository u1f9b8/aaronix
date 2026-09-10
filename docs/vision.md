# Aaronix Vision

## Definition

Aaronix is a Rust-first implementation of MINIX, built as close as practical to the original source and intent while using idiomatic, explicit, high-performance Rust wherever Rust improves correctness, safety, testability, and teachability.

The original MINIX implementation in C is the primary source study reference. In this repository, the book PDF is stored at [resources/OperatingSystems-Design-and-Implementation-3ed.pdf](resources/OperatingSystems-Design-and-Implementation-3ed.pdf), with the implementation listing beginning around page 658 of the document.

Aaronix is not a line-by-line translation exercise. It is an orderly OS implementation project: each design step studies the MINIX approach, decides how the same idea should be expressed in Rust, records the decision, then implements only the minimum useful capability needed to support the next step.

## Pairing Model

The owner writes the Rust implementation. Codex assists in a documentation and coaching capacity:

- clarify MINIX source intent,
- compare C structures, macros, and headers with Rust-native alternatives,
- help sequence Phase 1 into teachable, testable epics,
- draft and maintain ADRs, epics, PM files, diagrams, and research notes,
- review proposed implementation choices for architectural drift,
- and keep the documentation graph connected.

Codex should not independently type the Rust code unless the owner explicitly changes that working agreement.

## Ordering Principle

Every planning step should ask:

> If I were coding an OS from scratch, where would I start? What is the minimum useful thing I can build there to support the next step? How do I make this testable, progressing, and human-visible? Which decisions must be made now, and which decisions become better if deferred until a later phase?

This principle controls Phase 1 more than completeness does. The first version should be small, bootable, inspectable, and honest about what it does not yet decide.

## Phases

| Phase | Scope | Status |
| --- | --- | --- |
| Phase 1 | Basic MINIX-inspired OS foundation: boot path, kernel visibility, low-level output/input, memory foundations, process foundations, IPC direction, minimal userspace, and a shell-like human-visible loop. | Current focus. To be split into epics next. |
| Phase 1.5 | Rust SNES emulator showcase as a userspace/demo product. Linux-first standalone binary, later Aaronix-native adapter when Phase 1 exposes the required runtime surfaces. No bundled commercial ROMs. | Scoped by [ADR-0011](arch/adrs/0011-rust-snes-emulator-showcase-phase-1-5.md) and [Phase 1.5 epic scope](epics/phase-1-5-rust-snes-emulator-showcase.md). Deferred until Phase 1 can support it. |
| Phase 2 | Leverage the distributed base of Linux and enable Aaronix to run Linux binaries locally. | Deferred. Phase 1 should keep ABI boundaries explicit so this remains possible. |
| Phase 3 | Implement a native Windows executable runtime for the Aaronix environment, similar in ambition to WSL-style launch integration but in the opposite compatibility direction. | Deferred. Requires Phase 2-level ABI discipline and executable/runtime design. |
| Phase 4 | Minimal visual GUI to make Aaronix easier for newer users to work with. | Deferred. Should build on a stable userspace and graphics/input foundation. |

## Documentation Graph

| Area | Link | Purpose |
| --- | --- | --- |
| Project README | [../README.md](../README.md) | External project introduction and contributor starting path. |
| Documentation index | [readme.md](readme.md) | Human entry point for the docs folder. |
| Engineering mandate | [Engineering.md](Engineering.md) | Aaronix-specific engineering constraints for Rust-first OS design, documentation, testing, ABI boundaries, and phase discipline. |
| Setup | [setup/index_setup.md](setup/index_setup.md) | Development setup, test setup, and virtualization evidence collection. |
| Setup resources | [setup/resources/index_setup_resources.md](setup/resources/index_setup_resources.md) | Supporting setup assets. |
| Architecture | [arch/index_arch.md](arch/index_arch.md) | Architecture entry point, including ADRs and diagrams. |
| ADRs | [arch/adrs/index_adrs.md](arch/adrs/index_adrs.md) | Architecture decisions, adapted from reusable Argaile discipline and rewritten for Aaronix where applicable. |
| Diagrams | [arch/diagrams/index_diagrams.md](arch/diagrams/index_diagrams.md) | Architecture diagrams and visual explanations. |
| Architecture resources | [arch/resources/index_arch_resources.md](arch/resources/index_arch_resources.md) | Supporting architecture assets. |
| Epics | [epics/index_epics.md](epics/index_epics.md) | Concept/design breakdown by phase and capability. |
| Epic resources | [epics/resources/index_epic_resources.md](epics/resources/index_epic_resources.md) | Images, PDFs, binary examples, or other assets referenced by epic docs. |
| Project management | [pm/index_pm.md](pm/index_pm.md) | Execution coordination: milestones, tasks, subtasks, status, and acceptance evidence. |
| PM resources | [pm/resources/index_pm_resources.md](pm/resources/index_pm_resources.md) | Supporting execution artifacts, including milestone evidence. |
| Research | [research/index_research.md](research/index_research.md) | Source study, external references, tradeoff notes, and legal/licensing research. |
| Research resources | [research/resources/index_research_resources.md](research/resources/index_research_resources.md) | Research assets and copied references where redistribution is allowed. |
| General resources | [resources/index_resources.md](resources/index_resources.md) | Project-wide resources, including the MINIX book PDF. |

No major documentation artifact should be orphaned. New epics, PM plans, ADRs, diagrams, setup notes, evidence records, and research notes should be linked from their nearest index and, when important to the overall project shape, from this vision document or a phase overview.

## Setup and Evidence Loop

Development and test setup live under [setup/index_setup.md](setup/index_setup.md). The setup docs intentionally separate provisional tool categories from final commands because Phase 1 has not yet chosen the first boot target, bootloader, emulator profile, or build system shape.

Virtualized development output is part of project analysis. Emulator exit codes, serial logs, screenshots, test reports, and shell transcripts should be collected through [the virtualization evidence loop](setup/virtualization-evidence-loop.md), stored under [PM resources](pm/resources/index_pm_resources.md) when appropriate, and linked from the PM milestone they prove.

## Epic and PM Discipline

Epics live under [epics/](epics/) and cover concept/design. A good Aaronix epic explains:

- what minimal OS capability it introduces,
- what next step it unlocks,
- how MINIX handles the same concern,
- how Aaronix maps that design into Rust,
- what decisions are required now,
- what decisions are deferred and until when,
- and how progress becomes testable and human-visible.

PM files live under [pm/](pm/) and coordinate execution. Each PM file may contain milestones, tasks, and subtasks. A milestone is complete only when it is a testable bundle with visible evidence. After implementation, the relevant PM file must be updated so the plan reflects reality.

## ADR Discipline

ADRs live under [arch/adrs/](arch/adrs/). They are required whenever Aaronix chooses a meaningful architecture direction, translates a MINIX C idiom into a Rust design, freezes a boundary, introduces an ABI, changes phase ordering, or deliberately defers a decision.

The current seed ADR set is indexed at [arch/adrs/index_adrs.md](arch/adrs/index_adrs.md).

## Course Direction

Aaronix should be built so the implementation can become a comprehensive course on building an OS in Rust. That means each phase should produce:

- a concept narrative,
- a MINIX comparison,
- a Rust rationale,
- a small implementation target,
- a test or emulator demonstration,
- and a visible artifact a learner can understand.

The course should follow the actual project path, not a separate simplified fiction.

## Current Next Step

The next planning step is to split Phase 1 into epics. The split should start from the bootable minimum and progress only when each milestone unlocks the next capability.
