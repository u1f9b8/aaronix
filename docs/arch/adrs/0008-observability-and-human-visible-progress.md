---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0023 and ADR-0051
---

# ADR-0008 - Observability and Human-Visible Progress

## Context

Argaile's observability ADRs were heavy because they served an enterprise product with audit, compliance, WORM storage, sidecars, and operators. Aaronix does not need that machinery in Phase 1.

Aaronix does need the underlying principle: the system must make progress visible, diagnosable, and testable from the first boot milestone onward.

For an OS course/project, "human-visible" matters as much as "machine-testable." A learner should be able to see that the kernel reached a point in the boot path, that the console works, that a process ran, that a syscall returned, or that a shell command executed.

## Decision

Every Phase 1 milestone must define its primary visibility surface.

Allowed early visibility surfaces include:

- emulator exit status,
- serial console output,
- VGA/text-console output,
- panic output,
- structured host-side test reports,
- filesystem image inspection,
- shell prompt behavior,
- and tiny userspace programs with observable output.

Rules:

1. A milestone should have one fast automated check and one human-visible demonstration whenever practical.
2. Boot diagnostics must work before higher-level logging exists.
3. Diagnostic output must avoid heap or filesystem assumptions until those subsystems exist.
4. Kernel panic reporting should include the milestone-relevant context without pretending to be a full crash reporter.
5. Once a filesystem exists, persistent diagnostic records may be introduced behind a new ADR.
6. Enterprise audit, signed logs, WORM retention, and compliance evidence are not Phase 1 requirements.

## Consequences

Good:

- Phase 1 progress will be visible before the OS can run meaningful programs.
- Debugging does not depend on features that have not been built yet.
- The course can show a working artifact at the end of each milestone.

Costs:

- Each milestone must spend time defining what "done and visible" means.
- Serial/VGA/emulator output can be brittle if not wrapped in clear test helpers.

## Deferral

Persistent logs are deferred until Aaronix has a filesystem and enough process structure to decide where logs belong.

Audit-grade history is not a Phase 1 concern. If later phases introduce package managers, compatibility runtimes, networking, or GUI administration, audit requirements should be reconsidered then.

## Aaronix Review

The Argaile source ADRs contributed the local-first diagnostic principle. Argaile-specific audit trails, enterprise roles, WORM retention, regulatory mappings, telemetry sidecars, and vendor integrations were removed.
