---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: New Aaronix decision
---

# ADR-0012 - Development, Test, and Virtualization Evidence Environment

## Context

Aaronix needs more than code and design notes. A contributor needs to know how to prepare a development machine, how tests are expected to run, and how output from virtualized OS runs feeds back into project analysis.

For an OS project, virtualization is not just a convenience. It is one of the main feedback channels. A boot banner, serial log, emulator exit status, panic message, screenshot, shell transcript, or filesystem image inspection can be the proof that a milestone exists.

If this evidence is collected ad hoc, PM files will drift from reality and course material will lose the concrete artifacts that make the project teachable.

## Decision

Aaronix will maintain explicit setup documentation under `docs/setup/`:

- [Development environment setup](../../setup/development-environment.md)
- [Test environment setup](../../setup/test-environment.md)
- [Virtualization evidence loop](../../setup/virtualization-evidence-loop.md)
- [Setup index](../../setup/index_setup.md)

The project root will have a public-facing [README](../../../README.md) that explains what Aaronix is, how to navigate the repository, and how to start contributing.

Testing and evidence rules:

1. Every active PM milestone must name the commands used to build, test, emulate, and collect evidence.
2. Emulator and virtual-machine runs should capture serial output, exit status, emulator profile, and screenshots when useful.
3. Milestone evidence belongs under `docs/pm/resources/` unless another `resources/` folder is a better fit.
4. Evidence notes should use [the milestone evidence template](../../pm/resources/milestone-evidence-template.md).
5. If captured evidence contradicts an epic or ADR, the documentation must be corrected before the project continues to build on the wrong assumption.
6. Setup docs may describe provisional tools, but exact build and emulator commands are not locked until the relevant Phase 1 ADRs and PM files choose them.
7. Evidence must not include secrets, commercial ROMs, proprietary firmware, or third-party binary assets without redistribution rights.

## Consequences

Good:

- New contributors have a clear path into the repository.
- Test setup is separated from implementation details that are not decided yet.
- Virtualized development output becomes traceable project evidence.
- PM status can cite real artifacts instead of informal recollection.
- Course material can reuse milestone evidence from the actual project path.

Costs:

- PM updates require a small evidence-writing step.
- Large binary artifacts need curation so `docs/pm/resources/` does not become an unbounded dump.
- Tool setup will need revision after the boot target and emulator profile are chosen.

## Deferral

This ADR does not choose the bootloader, CPU architecture, emulator, CI provider, or exact build commands.

The Phase 1 bootstrapping epic must turn the provisional setup categories into concrete commands. Continuous integration should wait until the first real build and emulator smoke tests exist.

## MINIX Comparison

MINIX's book-driven implementation is teachable because code and explanation move together. Aaronix extends that discipline by preserving emulator evidence and PM state alongside the design notes.

## Rust Rationale

Rust gives strong host-side testing options for parsers, allocators, state machines, and ABI conversions before booting the OS. The setup and test environment should exploit that strength while still requiring emulator proof for boot-visible milestones.
