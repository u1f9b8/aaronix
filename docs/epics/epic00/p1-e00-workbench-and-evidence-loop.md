# Phase 1 Epic 00: Workbench and Evidence Loop

## Purpose

Create the smallest reliable development loop for Aaronix before writing kernel behavior. The learner should know how to build, run, observe, and record evidence from the project.

## Human-Visible Result

A local command path can prove the workbench is ready. It should identify the Rust toolchain, target strategy, emulator strategy, and evidence output location. If a kernel image does not exist yet, the command may fail with an intentional "no boot artifact yet" status instead of pretending success.

## Learning Goal

Before coding an OS, learn how OS work will be observed. The first lesson is not "write a constant file"; it is "make feedback trustworthy."

## MINIX Comparison

The MINIX appendix describes installation and simulator-based development. Aaronix should borrow the discipline of running in a simulator, but document a Rust-first workflow that fits this repository.

## Rust Design Focus

This epic should choose only the minimum host-side shape needed to support later Rust work:

- toolchain expectations,
- target strategy,
- emulator choice,
- evidence directories,
- and command names.

It should not create kernel crates, drivers, ABI files, or constants unless the first boot milestone needs them.

## Decisions Required Now

- Host development baseline.
- Emulator or virtual machine baseline.
- Evidence capture convention.
- Whether the first boot path uses an existing Rust-friendly bootloader or a smaller custom handoff.

## Deferred

- CI matrix.
- Cross-architecture support.
- Full repository crate layout.
- Kernel module structure.
- Userspace toolchain.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 00.1 | Document required local tools and setup checks. | Setup transcript or checklist. |
| 00.2 | Define the first emulator run command shape. | Command output, even if no kernel image exists yet. |
| 00.3 | Define where serial logs, screenshots, and emulator exits are stored. | First evidence folder following the template. |
| 00.4 | Record bootloader/emulator decision in an ADR if it becomes fixed. | Linked ADR. |

## Test Strategy

Use host checks first. Do not require a bootable OS artifact until Epic 01.

## PM Handoff

Create a PM file for this epic first. Its milestones should be small enough to complete before any kernel code is typed.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [Development Environment](../../setup/development-environment.md)
- [Test Environment](../../setup/test-environment.md)
- [Virtualization Evidence Loop](../../setup/virtualization-evidence-loop.md)
- [ADR-0012](../../arch/adrs/0012-development-test-and-virtualization-evidence-environment.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
