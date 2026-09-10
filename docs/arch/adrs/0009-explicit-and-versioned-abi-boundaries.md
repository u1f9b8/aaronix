---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0061
---

# ADR-0009 - Explicit and Versioned ABI Boundaries

## Context

Argaile treated binary ABI as a long-lived compatibility promise. Aaronix has even stronger reasons to be careful: an OS exposes binary boundaries by design.

Aaronix will eventually have:

- bootloader-to-kernel handoff structures,
- interrupt and trap frames,
- syscall numbers and argument conventions,
- IPC message layouts,
- executable loading rules,
- filesystem on-disk formats,
- userspace library interfaces,
- and later Linux and Windows compatibility layers.

In MINIX C, many of these are represented with headers and macros. In Rust, they should become explicit ABI modules, constants, newtypes, enums, and layout-tested structs.

## Decision

All ABI boundaries in Aaronix are explicit, documented, and versioned when they become externally observable.

Rules:

1. `#[repr(C)]`, packed layout, fixed-size integer fields, and raw pointers appear only in modules that own a hardware, disk, boot, syscall, IPC, FFI, or executable-format boundary.
2. Internal kernel APIs are not ABI. They remain idiomatic Rust and may evolve until exposed.
3. Syscall numbers, IPC message kinds, errno values, and executable-format assumptions must be documented before userspace depends on them.
4. ABI structs include explicit reserved fields or version fields when forward compatibility is needed.
5. Conversion between ABI types and kernel domain types is explicit and validated.
6. ABI compatibility tests are required once a boundary has a userspace consumer.
7. `unsafe` cannot cross an ABI boundary casually; the boundary module owns validation and exposes the narrow safe interface.

## Consequences

Good:

- Phase 1 userspace work will not accidentally freeze private kernel internals.
- Phase 2 Linux compatibility can be implemented as a compatibility layer, not a rewrite of kernel types.
- Phase 3 Windows runtime work has a place to attach PE/NT compatibility decisions.
- Course readers learn where Rust layout guarantees matter and where they do not.

Costs:

- ABI modules require more ceremony than private Rust structs.
- Once userspace depends on an ABI, changing it requires migration or an explicit break.

## Deferral

Only the ABI needed by each Phase 1 milestone should be frozen.

Linux ABI compatibility is deferred to Phase 2. Windows executable/runtime ABI work is deferred to Phase 3. GUI/application-facing APIs are deferred to Phase 4.

## Aaronix Review

The Argaile binary SDK ABI policy was useful as ABI discipline. Argaile SDK, C++ integration, SAP, callback threading, and product semver assumptions were removed. The decision now covers OS ABI surfaces.
