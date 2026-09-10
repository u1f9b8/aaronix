# Test Environment Setup

## Purpose

Aaronix should be testable from the first milestone. The test environment must support both fast host-side feedback and slower booted/emulated evidence.

This document defines the expected test layers. Exact commands will be added by the Phase 1 epics and PM files after the first target and boot path are chosen.

## Test Layers

| Layer | Purpose | Typical evidence |
| --- | --- | --- |
| Unit tests | Check pure Rust logic such as parsers, state machines, allocators, and conversions. | Test output and coverage notes where available. |
| Simulation/fake tests | Exercise deterministic substitutes for consoles, memory maps, clocks, filesystems, process tables, and devices. | Test output plus inspected fake state. |
| Contract tests | Verify that fake and real implementations share observable behavior. | Contract-suite output and linked boundary docs. |
| Emulator smoke tests | Boot Aaronix artifacts in a virtual machine. | Serial log, exit status, screenshot when useful, emulator profile. |
| Manual demonstrations | Show a human-visible milestone result for course/project review. | PM evidence note with command, expected result, observed result, and links to captured files. |

## Emulator Requirements

The default emulator is not yet locked. QEMU is the likely first candidate, but the boot ADR must make the final decision.

The selected emulator profile should support:

- headless execution for automated checks,
- serial output capture,
- deterministic exit status for pass/fail smoke tests,
- optional screenshots for display milestones,
- optional debug attachment for low-level bring-up,
- and stable command lines checked into the repository.

## Acceptance Evidence

Each PM milestone should specify:

- exact command run,
- expected result,
- observed result,
- relevant logs or screenshots,
- failure mode if the milestone is incomplete,
- and follow-up tasks if behavior differs from the design.

Use [virtualization-evidence-loop.md](virtualization-evidence-loop.md) and [the milestone evidence template](../pm/resources/milestone-evidence-template.md) for this process.

## Test Data Rules

- Prefer tiny deterministic fixtures.
- Store reusable test fixtures under the nearest `resources/` folder.
- Do not commit machine-local absolute paths in expected output.
- Do not commit secrets.
- Do not commit copyrighted ROMs, firmware, BIOS files, or commercial assets without documented redistribution rights.

## CI Deferral

Continuous integration is deferred until the first implementation milestone defines real build and test commands.

The first CI contract should be small: formatting, host tests, and a headless emulator smoke test. Broader matrix testing should wait until Phase 1 has multiple meaningful targets or artifacts.
