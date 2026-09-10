# Aaronix Architecture Decision Records

This directory contains Architecture Decision Records for Aaronix.

Aaronix is an OS project, not a web application or enterprise AI service. The ADRs adapted from Argaile were reviewed and rewritten to preserve useful engineering discipline while removing Argaile-specific business decisions, compliance framing, identity-provider assumptions, provider integrations, database choices, and UI-product concerns.

## Accepted ADRs

| ADR | Title | Source | Aaronix review |
| --- | --- | --- | --- |
| [ADR-0001](0001-record-architecture-decisions.md) | Record Architecture Decisions | Argaile ADR-0001 | Reused almost directly, with Aaronix-specific triggers for MINIX comparisons, Rust tradeoffs, phase impact, and deferral boundaries. |
| [ADR-0002](0002-single-repository-and-documentation-layout.md) | Single Repository and Documentation Layout | Argaile ADR-0002 | Kept the monorepo discipline, removed web/backend/database specifics, and scoped code layout decisions to OS artifacts, userspace, host tooling, docs, and course material. |
| [ADR-0003](0003-kernel-userspace-and-tooling-boundaries.md) | Kernel, Userspace, and Tooling Boundaries | Argaile ADR-0003 | Reframed hexagonal dependency direction as OS boundary discipline across kernel core, architecture support, userspace, and host tooling. |
| [ADR-0004](0004-simulation-and-test-double-discipline.md) | Simulation and Test Double Discipline | Argaile ADR-0004 | Recast dummy adapters as deterministic fakes, simulators, and emulator harnesses for hardware, filesystems, process tables, and ABI contracts. |
| [ADR-0005](0005-boundary-type-separation-policy.md) | Boundary Type Separation Policy | Argaile ADR-0005 | Replaced DTO language with OS boundary types: kernel domain types, hardware/boot layouts, on-disk layouts, syscall/IPC ABI structs, and host-tool DTOs. |
| [ADR-0006](0006-configuration-and-boot-parameter-precedence.md) | Configuration and Boot Parameter Precedence | Argaile ADR-0024 | Retained strict precedence, adapted it to compiled defaults, target/build configuration, and boot/runtime parameters. |
| [ADR-0007](0007-contract-tests-for-observable-boundaries.md) | Contract Tests for Observable Boundaries | Argaile ADR-0050 | Kept conformance-suite discipline, adapted it to kernel subsystems, filesystem images, drivers, syscalls, IPC, and MINIX behavior comparison. |
| [ADR-0008](0008-observability-and-human-visible-progress.md) | Observability and Human-Visible Progress | Argaile ADR-0023 and ADR-0051 | Kept the local-first diagnostic principle, removed enterprise audit/WORM machinery, and tied observability to boot milestones and course checkpoints. |
| [ADR-0009](0009-explicit-and-versioned-abi-boundaries.md) | Explicit and Versioned ABI Boundaries | Argaile ADR-0061 | Reused ABI discipline for syscalls, IPC messages, boot handoff, executable formats, and future Linux/Windows compatibility layers. |
| [ADR-0010](0010-supervision-and-restart-semantics.md) | Supervision and Restart Semantics | Argaile ADR-0068 and ADR-0115 | Preserved restart/supervision thinking but scoped it to MINIX-style servers, process lifecycle, and later phase compatibility runtimes. |
| [ADR-0011](0011-rust-snes-emulator-showcase-phase-1-5.md) | Rust SNES Emulator Showcase for Phase 1.5 | New Aaronix decision | Adds Phase 1.5 scope for a Rust-from-scratch userspace SNES emulator, Linux-first, with no bundled commercial ROMs. |
| [ADR-0012](0012-development-test-and-virtualization-evidence-environment.md) | Development, Test, and Virtualization Evidence Environment | New Aaronix decision | Adds root README, setup docs, test environment expectations, and a virtualization evidence loop for PM and analysis. |

## Argaile ADRs Left Behind

The remaining Argaile ADRs were not copied because their decisions are specific to Argaile's product domain. The main excluded categories are:

- AI provider ports, model acquisition, fine-tuning, prompts, recipes, suggestions, RAG, and content policy.
- Enterprise identity federation, OIDC, Entra ID, linked IdPs, role catalogs, and customer-defined permissions.
- Database product choices such as PostgreSQL/SQLite tiers, migrations, repository DTOs, and web service persistence.
- Compliance/audit-office workflows, evidence packs, WORM archives, electronic signatures, and regulated-market lifecycle gates.
- React/Web/WASM UI decisions, chat state, drawer UX, and product-tier behavior.
- External vendor adapter/plugin machinery that solves Argaile integration delivery rather than Aaronix kernel/userspace design.

Some excluded records may become useful later as source material, but they should be reconsidered only when a concrete Aaronix phase raises the same force. For example, Argaile's out-of-process adapter IPC records should not drive Phase 1 kernel IPC design, but they may be worth rereading during Phase 2 or Phase 3 compatibility-runtime planning.

## Related Indexes

- [Vision](../../vision.md)
- [Architecture](../index_arch.md)
- [Setup](../../setup/index_setup.md)
- [Epics](../../epics/index_epics.md)
- [Project Management](../../pm/index_pm.md)
- [Research](../../research/index_research.md)
