---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0004
---

# ADR-0004 - Simulation and Test Double Discipline

## Context

Argaile required every port to ship with a deterministic dummy adapter so application logic could be tested without real infrastructure. Aaronix needs the same discipline, but the substitutes are OS-shaped:

- simulated devices,
- fake clocks,
- in-memory filesystems,
- synthetic boot information,
- fake process tables,
- syscall/IPC test harnesses,
- and emulator-level smoke tests.

OS projects often slip into "it works in QEMU" as the only test. That is not enough for a step-by-step project or a course. Hardware and emulator tests are necessary, but small deterministic tests must exist below them so each subsystem can be built in teachable increments.

## Decision

Every new boundary that is hard to exercise directly must ship with a deterministic test double or simulator in the same milestone that introduces the boundary.

Rules:

1. **Deterministic:** no wall-clock time, randomness, host filesystem dependency, or ambient environment unless explicitly injected.
2. **Inspectable:** tests can directly inspect state such as emitted console bytes, allocated frames, scheduled processes, or generated image structures.
3. **Honest:** a fake implements the same observable contract as the real boundary. If a capability is unsupported, it returns a documented error instead of panicking.
4. **Small:** fakes are not alternate OS implementations. They exist to exercise one boundary.
5. **Co-shipped:** a milestone that introduces a boundary also introduces the fake/simulated path or explains why the real emulator/hardware path is the only meaningful test.
6. **No hidden success:** placeholder code must not silently report success. Incomplete paths should fail clearly.

Examples:

- A console writer can be tested against a byte buffer before it writes to VGA or serial hardware.
- A frame allocator can be tested against a synthetic memory map before reading real bootloader memory information.
- A filesystem parser can be tested against a tiny in-memory image before booting from a disk image.
- A syscall or IPC boundary can have host-side conformance tests before real userspace programs depend on it.

## Consequences

Good:

- Phase 1 milestones stay testable without waiting for the whole OS to exist.
- Emulator failures become easier to localize.
- Course lessons can show a fast host-side test before the slower booted demonstration.

Costs:

- Each boundary has at least two paths to maintain: a real one and a simulated/test one.
- Poorly designed fakes can hide missing hardware behavior, so emulator acceptance tests still matter.

## Deferral

Full hardware-device emulation is not required in Phase 1. A fake should cover only the behavior needed by the current milestone.

Property testing and formal models are deferred until a subsystem has stable contracts worth generalizing.

## Aaronix Review

The Argaile source ADR was valuable, but "dummy adapter" was too application-specific. This ADR replaces that language with fakes, simulators, and emulator harnesses appropriate for kernel and userspace work.
