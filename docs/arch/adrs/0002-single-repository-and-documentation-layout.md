---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0002
---

# ADR-0002 - Single Repository and Documentation Layout

## Context

Aaronix needs kernel code, architecture-specific support, userspace programs, filesystem/image tools, emulator harnesses, diagrams, epics, PM plans, ADRs, and source-study notes to move together. Splitting these too early would make a small OS harder to evolve. Collapsing everything into one crate or one undocumented folder would hide boundaries that matter for correctness and teaching.

Phase 1 should feel like a sequence of testable, human-visible increments:

1. a minimal bootable artifact,
2. a kernel that can report where it is,
3. basic output/input,
4. memory and process foundations,
5. IPC and small userspace programs,
6. a shell or shell-like command loop,
7. and eventually enough MINIX-like structure to justify deeper subsystem work.

The repository layout should support that progression without deciding every future crate name before the first implementation epic is written.

## Decision

Aaronix uses one repository for all first-party source, host tools, tests, documentation, and course material.

The intended top-level documentation layout is:

```text
docs/
  vision.md
  Engineering.md
  resources/
  research/
  epics/
    resources/
  pm/
    resources/
  arch/
    index_arch.md
    adrs/
    diagrams/
    resources/
```

The intended implementation layout, to be created by Phase 1 epics as needed, is:

```text
.
  Cargo.toml              # Rust workspace when code begins
  crates/                 # reusable Rust crates
  kernel/                 # bootable kernel composition root, if kept separate from crates
  userspace/              # first Aaronix user programs and shell experiments
  tools/                  # host-side image builders, inspectors, and test utilities
  tests/                  # integration and emulator-level tests
  docs/                   # design, planning, source-study, and course material
```

Rules:

1. The repository is the unit of architectural review.
2. Documentation is not an afterthought; epics, PM files, ADRs, diagrams, and source-study notes are first-class project artifacts.
3. Code layout decisions that affect dependency direction, crate naming, target architecture, bootloader choice, or userspace ABI require an ADR.
4. Host tools must stay visibly separate from kernel and userspace code.
5. Generated build artifacts, disk images, emulator output, and binary blobs belong outside source directories or under an explicit `resources/` directory when documentation references them.
6. The course narrative should follow the same milestones as the implementation rather than becoming a separate invented path.

## Consequences

Good:

- Design, execution, and teaching material stay synchronized.
- Cross-cutting changes can be made atomically.
- Readers can trace a milestone from vision to epic, PM tasks, ADRs, tests, and implementation.

Costs:

- The repository will hold several kinds of artifacts, so index files must be maintained.
- The workspace layout must resist growing accidental folders without an explicit purpose.

## Deferral

This ADR does not choose:

- CPU architecture,
- bootloader,
- disk-image format,
- Rust workspace crate names,
- shell design,
- or user/kernel ABI.

Those decisions belong to the Phase 1 boot and kernel-foundation epics.

## Aaronix Review

The Argaile source ADR was useful for monorepo discipline. Argaile-specific Rust backend, React, WASM, database, and provider crate examples were removed. The rewritten decision is about OS artifacts, host tooling, documentation, and course progression.
