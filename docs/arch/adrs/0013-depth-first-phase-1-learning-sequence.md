# ADR-0013: Depth-First Phase 1 Learning Sequence

## Status

Accepted

## Context

Aaronix is both an operating-system implementation and the foundation for a future course on building an OS in Rust. Phase 1 must therefore progress in a way that is useful to a learner, not only in a way that is convenient for an experienced implementer.

The MINIX source listing begins with broad C header files and many constants, types, and prototypes. That is normal for a C system source distribution, but it is not the right teaching or implementation order for Aaronix. Translating all headers, constants, macros, structures, and helper APIs before the first bootable milestone would create a large amount of code that cannot yet be justified, tested, or explained through visible behavior.

Aaronix needs a sequence where each step asks:

- what is the smallest useful capability to build now,
- what human-visible result proves it works,
- what concept does the owner learn by typing it,
- what next capability does it unlock,
- which decision must be recorded now,
- and which decision becomes better if deferred.

## Decision

Phase 1 will be organized as a depth-first learning sequence.

Each epic must introduce one teachable capability, produce one human-visible result, and add only the code, constants, types, crates, files, and helper functions required by the current milestone.

Aaronix will not create broad translated constant files, bulk MINIX header ports, unused abstractions, placeholder subsystems, or future-facing crates merely because the project is expected to need them later. MINIX source material is a reference and comparison point, not a backlog to pre-port.

Future work may add constants, ABI values, structures, and modules when a current milestone needs them and can test them. At that point, the relevant epic or ADR must explain why the item is needed now and how MINIX handled the same concern.

## Consequences

This keeps Phase 1 understandable and demonstrable. Each milestone should be short enough for the owner to type, reason about, and explain as part of a course lesson.

This also means Aaronix will occasionally defer decisions that an experienced OS engineer could make earlier. Deferral is acceptable when the project does not yet have enough runnable evidence to make the decision useful.

The repository may temporarily look smaller than a traditional OS tree. That is intentional. Structure should appear when it supports the next testable result.

## Rules

- Start each Phase 1 epic from the previous runnable artifact.
- Add no constants or source files unless the current milestone uses them.
- Prefer a short, visible demonstration over invisible architectural completeness.
- Record architecture decisions before freezing a boundary.
- Keep the MINIX comparison in the epic, but implement the Aaronix subset that teaches the current concept.
- Treat unused code as design debt unless it directly supports test setup, diagnostics, or imminent execution.

## Deferred Decisions

The following should not be decided globally in early Phase 1:

- full POSIX constant inventory,
- full syscall table,
- full IPC message catalog,
- full filesystem layout,
- complete process table design,
- Linux binary compatibility surface,
- Windows runtime compatibility surface,
- graphics stack,
- and emulator application packaging.

Each of these can be decided when a later epic reaches the point where a real implementation step and test need it.

## Related Documents

- [Vision](../../vision.md)
- [Phase 1 Overview](../../epics/p1-overview.md)
- [Engineering Mandate](../../Engineering.md)
- [ADR-0008: Observability and Human-Visible Progress](0008-observability-and-human-visible-progress.md)
- [ADR-0012: Development, Test, and Virtualization Evidence Environment](0012-development-test-and-virtualization-evidence-environment.md)
