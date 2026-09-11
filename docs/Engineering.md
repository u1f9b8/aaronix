# Aaronix Engineering Mandate

This document defines the engineering standards for Aaronix.

Aaronix is a Rust-first implementation of MINIX. The goal is to preserve the original MINIX source and intent as closely as practical while using Rust to improve safety, clarity, testability, and long-term maintainability. Aaronix is also being shaped as future course material for teaching how to build an OS in Rust.

## 1. Working Agreement

The project owner writes the Rust implementation. Assistants and collaborators operate primarily in documentation, design, coaching, review, and planning roles unless the owner explicitly asks for code changes.

Assistant responsibilities:

- study and explain MINIX source intent,
- compare MINIX C idioms with Rust alternatives,
- draft and maintain vision, epics, PM files, ADRs, diagrams, setup docs, and research notes,
- identify architectural decisions and deferrals,
- review proposed implementation direction,
- and keep the documentation graph connected.

## 2. Core Architecture Mandate

Aaronix uses OS boundary discipline. The important boundaries are:

- kernel core,
- architecture-specific hardware support,
- bootloader handoff,
- device-facing code,
- syscall and IPC ABI,
- userspace programs,
- host-side build/image/test tools,
- and documentation/course artifacts.

Dependency direction must preserve those boundaries:

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

Non-negotiable rules:

- Kernel core must not depend on host tools, emulator harnesses, userspace programs, or course scaffolding.
- Userspace must not link against private kernel internals.
- Hardware-specific code must be isolated behind narrow modules or traits.
- Host tools may inspect and build Aaronix artifacts, but they are not part of the booted OS.
- Any stable boundary exposed to userspace must be documented as ABI.
- Later Linux and Windows compatibility work must attach through compatibility layers, not by contaminating Phase 1 kernel types.

Relevant ADRs:

- [ADR-0003 - Kernel, Userspace, and Tooling Boundaries](arch/adrs/0003-kernel-userspace-and-tooling-boundaries.md)
- [ADR-0005 - Boundary Type Separation Policy](arch/adrs/0005-boundary-type-separation-policy.md)
- [ADR-0009 - Explicit and Versioned ABI Boundaries](arch/adrs/0009-explicit-and-versioned-abi-boundaries.md)
- [ADR-0013 - Depth-First Phase 1 Learning Sequence](arch/adrs/0013-depth-first-phase-1-learning-sequence.md)

## 3. Rust-First Systems Rules

Rust is the default implementation language. Assembly is acceptable only for cases where the hardware contract genuinely requires it, such as early boot transitions, interrupt/trap entry, context switching, privileged CPU instructions, or target-specific calling conventions.

Rules:

- Prefer safe Rust for all code that is not directly touching hardware, ABI layout, or CPU state.
- Keep `unsafe` small, local, documented, and wrapped by a safe API when possible.
- Kernel code should be `no_std` unless a specific crate or tool is intentionally host-side.
- Do not assume heap allocation before an allocator exists.
- Do not use hidden global mutable state when ownership or explicit initialization can model the state.
- Use fixed-width integer types at hardware, disk, and ABI boundaries.
- Use newtypes, enums, and bitflags instead of raw integers when the value has meaning.
- Use volatile, atomic, or memory-barrier operations only where the hardware or concurrency model requires them, and document why.
- Panic is acceptable in early bring-up and fatal kernel states, but recoverable subsystem errors should become typed results once the subsystem has a caller that can handle them.

## 4. MINIX Translation Rules

Aaronix is not a mechanical C-to-Rust translation. Before implementing a subsystem, the design notes should explain how MINIX handles the concern and how Aaronix will express the same intent in Rust.

Translation guidance:

- C `#define` constants become Rust `const`, enums, bitflags, or newtypes depending on meaning.
- Do not pre-port constants, structs, headers, or macros that the current milestone does not use.
- C structs used for binary layout become dedicated layout structs, not general kernel domain types.
- C structs used for internal state become Rust types with constructors and invariants where useful.
- C macros become functions, methods, const functions, or typed wrappers when possible.
- C global state becomes explicit subsystem state unless the hardware or bootstrap phase requires otherwise.
- Header-file boundaries become Rust modules/crates only when the boundary is meaningful in Aaronix.
- Preserve MINIX behavior intentionally; diverge only when Rust safety, clarity, portability, or phase ordering gives a strong reason.

Each epic should include a MINIX comparison and Rust rationale before execution PM is finalized.

## 5. Phase and Milestone Discipline

The project advances in ordered phases:

- **Phase 1:** basic MINIX-inspired OS foundation.
- **Phase 1.5:** Rust SNES emulator showcase as a userspace/demo product, scoped by [ADR-0011](arch/adrs/0011-rust-snes-emulator-showcase-phase-1-5.md).
- **Phase 2:** Linux binary compatibility.
- **Phase 3:** native Windows executable runtime.
- **Phase 4:** minimal visual GUI.

Phase 1 is the active focus. Later phases may influence boundary choices, but they must not pull premature implementation work into Phase 1.

Phase 1 follows a depth-first learning sequence. Each epic should be short-coded, practical, and course-friendly: one concept, one small implementation surface, one visible result, and one unlock for the next lesson.

Every milestone must answer:

- What is the smallest useful OS capability being added?
- What next capability does it unlock?
- How will it be tested?
- How will a human see that it works?
- Which decisions are required now?
- Which decisions are explicitly deferred, and until which phase or milestone?

## 6. Development and Test Environment

Development setup, test setup, and virtualization evidence collection are first-class project concerns.

Rules:

- The external starting path lives in [the root README](../README.md).
- Development setup lives in [docs/setup/development-environment.md](setup/development-environment.md).
- Test setup lives in [docs/setup/test-environment.md](setup/test-environment.md).
- Virtualized run output flows back through [docs/setup/virtualization-evidence-loop.md](setup/virtualization-evidence-loop.md).
- Exact build, emulator, and test commands belong in PM files once the relevant Phase 1 epic chooses them.
- Tool choices should be pinned in source-controlled files when implementation begins.

Relevant ADRs:

- [ADR-0012 - Development, Test, and Virtualization Evidence Environment](arch/adrs/0012-development-test-and-virtualization-evidence-environment.md)
- [ADR-0013 - Depth-First Phase 1 Learning Sequence](arch/adrs/0013-depth-first-phase-1-learning-sequence.md)

## 7. Testing Mandate

Aaronix must be testable from the first milestone.

Testing layers:

- **Unit tests:** pure Rust logic, type conversions, parsers, allocators, schedulers, and state machines where host-side testing is possible.
- **Simulation/fake tests:** deterministic devices, memory maps, clocks, consoles, filesystems, process tables, and syscall/IPC harnesses.
- **Contract tests:** observable behavior shared by fake and real implementations.
- **Emulator smoke tests:** bootable artifacts run under an emulator and produce an exit code, serial output, screen output, or another visible signal.
- **Manual demonstrations:** human-visible checkpoints for course and project progress.

Rules:

- A milestone is not complete without acceptance evidence.
- Hardware/emulator success alone is not enough when a smaller deterministic test can exist.
- A fake must be honest about unsupported behavior and must not silently report success for unimplemented behavior.
- Test artifacts referenced by PM files belong under `docs/pm/resources/` or another appropriate `resources/` folder.

Relevant ADRs:

- [ADR-0004 - Simulation and Test Double Discipline](arch/adrs/0004-simulation-and-test-double-discipline.md)
- [ADR-0007 - Contract Tests for Observable Boundaries](arch/adrs/0007-contract-tests-for-observable-boundaries.md)
- [ADR-0008 - Observability and Human-Visible Progress](arch/adrs/0008-observability-and-human-visible-progress.md)

## 8. Configuration and Boot Parameters

Configuration must be explicit, reproducible, and validated before use.

Rules:

- Compiled defaults, build/target configuration, and boot/runtime parameters have strict precedence.
- Required boot information must be validated before the kernel relies on it.
- The source of important configuration values should be visible in diagnostics or tests.
- Host-tool environment variables may follow `AARONIX__SECTION__FIELD`, but host-tool configuration is not automatically kernel configuration.
- Persistent runtime configuration is deferred until Aaronix has filesystem and init/userspace structure sufficient to own it cleanly.

Relevant ADR:

- [ADR-0006 - Configuration and Boot Parameter Precedence](arch/adrs/0006-configuration-and-boot-parameter-precedence.md)

## 9. Documentation and ADR Rules

Documentation is part of the engineering work.

Required documentation structure:

- [README.md](../README.md) for external orientation,
- [vision.md](vision.md) for project definition and phase map,
- [setup/](setup/index_setup.md) for development, test, and virtualization evidence setup,
- [epics/](epics/index_epics.md) for concept and design,
- [pm/](pm/index_pm.md) for execution planning and milestone state,
- [arch/adrs/](arch/adrs/index_adrs.md) for decisions,
- [arch/diagrams/](arch/diagrams/index_diagrams.md) for diagrams,
- [research/](research/index_research.md) for source study and tradeoff notes,
- and `resources/` folders for referenced assets.

Rules:

- Major documents should be reachable from an index.
- Every meaningful architectural decision needs an ADR.
- Every implemented task should update the relevant PM file.
- Every milestone should link its evidence.
- Every epic should cite the ADRs that govern it.
- Diagrams support decisions; they do not replace ADRs.
- Course-facing explanations should follow the real implementation sequence.

Relevant ADR:

- [ADR-0001 - Record Architecture Decisions](arch/adrs/0001-record-architecture-decisions.md)

## 10. Security, Integrity, and Licensing

Aaronix is an OS project, so mistakes in privilege, memory, and binary boundaries are engineering defects, not cosmetic issues.

Rules:

- Treat privilege transitions, memory access, interrupt state, syscalls, IPC, executable loading, and filesystem mutation as security-sensitive.
- Keep user/kernel boundaries explicit and narrow.
- Validate untrusted userspace input before it affects kernel state.
- Avoid leaking kernel pointers or private state through ABI surfaces.
- Do not bundle third-party binary assets unless their licenses permit redistribution.
- Phase 1.5 SNES ROMs must be original, commissioned, public-domain, or redistributable under compatible terms; commercial ROM bundles are forbidden without explicit rights.
- Do not copy, port, or translate GPL or otherwise restrictive code into Aaronix unless a dedicated ADR accepts the license consequences.

## 11. Review Behavior

When reviewing a design, epic, PM plan, or proposed implementation, check:

- Does it preserve kernel/userspace/tooling boundaries?
- Does it follow the Rust-first mandate without hiding necessary assembly?
- Does it preserve MINIX intent or explain a justified divergence?
- Does it freeze an ABI accidentally?
- Does it make the next step easier to build?
- Is the milestone testable and human-visible?
- Are unsafe blocks, global state, and layout assumptions locally justified?
- Has the relevant ADR or PM file been updated?
- Has milestone evidence been collected or linked when implementation has run?
- Is a decision being made too early when a later phase would provide better information?

Call out architectural drift directly and propose a narrower correction.

## 12. Output Behavior for Project Content

When generating project documentation, prefer this order:

1. State the architecture or subsystem purpose.
2. Explain the MINIX reference point.
3. Explain the Rust design rationale.
4. Identify required ADRs or new decisions.
5. Identify deferrals.
6. Define test and human-visible acceptance criteria.
7. Link the document into the appropriate index.

When discussing code, do not implement Rust on behalf of the owner unless explicitly asked. Provide focused guidance, review notes, pseudocode, test strategy, and boundary analysis as needed.
