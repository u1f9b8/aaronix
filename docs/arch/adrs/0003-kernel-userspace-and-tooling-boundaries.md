---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0003
---

# ADR-0003 - Kernel, Userspace, and Tooling Boundaries

## Context

Argaile used strict hexagonal architecture to keep business logic independent from web, database, and vendor adapters. Aaronix is not that kind of system, but the underlying discipline is still useful: dependencies should point toward the stable core, and boundary crossings should be explicit.

For an OS, the important boundaries are different:

- kernel core versus architecture-specific hardware details,
- kernel interfaces versus userspace programs,
- host-side tools versus booted Aaronix artifacts,
- MINIX source-study notes versus Rust implementation choices,
- and temporary simulation/test machinery versus real boot/runtime behavior.

MINIX itself is structured around clear responsibilities: kernel mechanisms, process management, filesystem work, drivers, system tasks, and user programs. Aaronix should preserve that intent while using Rust modules, crates, types, traits, and tests to make the boundaries enforceable.

## Decision

Aaronix uses OS boundary discipline rather than application-style hexagonal language.

Dependency direction:

```text
host tools and test harnesses
        |
        v
architecture and device boundary code
        |
        v
kernel core mechanisms
        ^
        |
userspace crosses only through syscall/IPC ABI
```

Rules:

1. Kernel core code must not depend on host tools, emulator harnesses, test-only simulators, or userspace programs.
2. Architecture-specific code must be isolated behind explicit modules or traits. Unsafe code, inline assembly, packed layouts, descriptor tables, interrupt entry code, and hardware register access belong at this boundary.
3. Userspace programs must communicate with the kernel through documented syscalls, IPC messages, or bootstrapping contracts. They must not link against kernel internals.
4. Host tools may read or construct Aaronix artifacts, such as disk images or symbol maps, but those tools are not part of the running OS.
5. Rust safety boundaries are architectural boundaries. Every `unsafe` block needs a local reason, and every module that owns hardware unsafety should have a narrow public surface.
6. MINIX compatibility is an explicit design input, not an excuse to import C structure blindly.

## Consequences

Good:

- The kernel can be tested and reasoned about without host-tool leakage.
- Architecture-specific work can evolve without contaminating portable logic.
- Userspace work naturally prepares Phase 2 Linux compatibility because ABI boundaries are already explicit.
- The course can explain each boundary at the moment it becomes useful.

Costs:

- Small early milestones may require more scaffolding than a single-file kernel.
- Some MINIX C structures will require careful Rust translation rather than line-by-line copying.

## Deferral

The exact module/crate layout for architecture support is deferred until the boot epic chooses the first target architecture and boot path.

Linux binary compatibility is deferred to Phase 2. Windows executable runtime work is deferred to Phase 3. GUI boundaries are deferred to Phase 4.

## Aaronix Review

The Argaile ADR's dependency-direction discipline was useful. References to app/domain/adapters/databases/frameworks were removed. The decision now speaks in OS terms: kernel, architecture support, userspace, host tools, syscalls, IPC, and unsafe boundaries.
