---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0005
---

# ADR-0005 - Boundary Type Separation Policy

## Context

Argaile separated domain entities, transport DTOs, persistence DTOs, and ABI structs to prevent one concern from shaping every layer. Aaronix needs the same protection, but with OS-specific types.

The original MINIX source uses C structs, headers, macros, integer constants, and layout assumptions across many files. Rust gives Aaronix better tools: newtypes, enums, modules, trait boundaries, explicit conversions, and tightly scoped `repr(C)` or packed structures when layout matters.

If Aaronix reuses one struct for kernel logic, hardware layout, on-disk records, and userspace ABI, the design will become brittle. A disk layout change could affect kernel invariants. A syscall compatibility decision could leak into internal scheduling logic. A hardware descriptor could be accidentally treated as a safe Rust value.

## Decision

Aaronix keeps distinct type families for distinct boundaries.

| Type family | Owns | Examples | Forbidden |
| --- | --- | --- | --- |
| Kernel domain types | Internal invariants and state transitions | `Pid`, process state, permissions, virtual addresses, frame counts | External binary layout promises unless explicitly wrapped |
| Architecture/hardware layouts | CPU, bootloader, interrupt, descriptor, register, and device layouts | GDT/IDT entries, boot memory map entries, port I/O wrappers | Business logic, userspace policy, host-tool assumptions |
| On-disk layouts | Filesystem and executable bytes as stored | superblocks, inodes, directory entries, executable headers | Direct mutation as kernel domain objects |
| Syscall/IPC ABI types | Stable user/kernel message layout | syscall numbers, message structs, errno values, IPC envelopes | Private kernel-only fields |
| Host-tool DTOs | Configuration and artifact formats for build/test tools | image-builder manifests, emulator test reports | Running-kernel invariants |

Rules:

1. `#[repr(C)]`, `#[repr(transparent)]`, packed structs, and fixed-size integer layout are used only where layout is part of the contract.
2. Kernel domain types should use Rust invariants instead of raw integers where the invariant matters.
3. Boundary conversion is explicit and fallible when validation can fail.
4. MINIX constants and `#define` values should become Rust constants, enums, bitflags, or newtypes according to their meaning.
5. On-disk and ABI structs are parsed into internal types before domain logic operates on them.
6. Unsafe layout assumptions are documented locally and tested with size/alignment checks where possible.

## Consequences

Good:

- Rust types can encode invariants that MINIX C relied on discipline to preserve.
- Phase 2 Linux ABI work can be isolated instead of mixed with Aaronix internal kernel types.
- Disk-format and syscall compatibility can evolve independently.
- The course can teach why a C header is not always a Rust struct.

Costs:

- More mapping code.
- Early examples may look more verbose than the equivalent C.

## Deferral

Exact type names and module locations are deferred to the epics that introduce each boundary.

Linux-compatible ABI structs are deferred to Phase 2. Windows/PE/NT compatibility structs are deferred to Phase 3.

## Aaronix Review

The Argaile DTO separation policy was reusable as a boundary-type policy. Web transport, database, `serde`, and `sqlx` assumptions were removed. The result is focused on kernel domain types, hardware layouts, on-disk layouts, syscalls, IPC, and host-tool data.
