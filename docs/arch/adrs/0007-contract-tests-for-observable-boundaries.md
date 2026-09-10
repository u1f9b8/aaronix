---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0050
---

# ADR-0007 - Contract Tests for Observable Boundaries

## Context

Argaile used shared adapter contract tests to prevent multiple implementations of a port from drifting. Aaronix will also have repeated boundaries:

- fake console versus serial/VGA console,
- synthetic boot memory map versus real bootloader memory map,
- in-memory filesystem image versus booted filesystem access,
- host-side syscall/IPC conformance tests versus real userspace programs,
- MINIX behavior descriptions versus Aaronix Rust behavior,
- and eventually Aaronix ABI versus Linux or Windows compatibility behavior.

An OS can appear to work while violating a boundary contract. That is especially dangerous when a later milestone depends on a behavior that was never made explicit.

## Decision

Aaronix creates shared contract tests when a boundary has more than one implementation or when a future implementation is already known.

Rules:

1. The contract test describes behavior observable through the boundary, not implementation details.
2. The simplest implementation, fake, or simulator can act as the first reference, but it must not define behavior by accident.
3. Emulator-level smoke tests are required for boot-visible milestones.
4. A Phase 1 milestone is not complete unless it has a test or demonstration that a human can understand.
5. When MINIX behavior is the reference, the epic should cite the source/book discussion and the contract test should encode the selected Aaronix behavior.
6. Performance contracts are separate from correctness contracts unless the milestone explicitly depends on timing.

Candidate contract-test surfaces:

- console output,
- boot information parsing,
- frame allocation,
- process-state transitions,
- interrupt dispatch,
- IPC message validation,
- syscall number and error behavior,
- filesystem metadata parsing,
- userspace program loading.

## Consequences

Good:

- Boundary behavior becomes discoverable.
- Fake and real implementations cannot drift silently.
- The project can move in small steps without relying only on end-to-end boot tests.
- Course lessons can pair each new abstraction with an executable check.

Costs:

- Shared contract tests introduce another artifact to maintain.
- Tightening a contract may require updates across several implementations.

## Deferral

No global contract-test crate is required before code exists. The first implementation epic that introduces a repeated boundary should create the smallest suitable contract-test location.

Linux ABI conformance tests are deferred to Phase 2. Windows executable/runtime conformance tests are deferred to Phase 3.

## Aaronix Review

The Argaile shared contract-test ADR was reusable. Database adapters, provider ports, async service adapters, and IPC plugin details were removed. The rewritten decision is about observable OS boundaries and MINIX comparison.
