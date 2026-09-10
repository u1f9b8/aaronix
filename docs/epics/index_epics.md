# Aaronix Epics

Epics contain concept and design work. They explain what Aaronix will build, why the work is ordered that way, how MINIX handled the concern, how Aaronix should express it in Rust, which ADRs govern it, and what testable milestone the work unlocks.

## Phase Map

| Phase | Epic status | Notes |
| --- | --- | --- |
| Phase 1 | To be decomposed next | Current focus: basic bootable MINIX-inspired OS foundation. Start from the smallest bootable artifact and add only what unlocks the next visible capability. |
| Phase 1.5 | [Rust SNES Emulator Showcase](phase-1-5-rust-snes-emulator-showcase.md) | Deferred userspace showcase after Phase 1 exposes enough runtime surface. Governed by [ADR-0011](../arch/adrs/0011-rust-snes-emulator-showcase-phase-1-5.md). |
| Phase 2 | Deferred | Linux binary compatibility. Requires explicit ABI discipline from Phase 1. |
| Phase 3 | Deferred | Native Windows executable runtime. Requires compatibility-runtime architecture decisions. |
| Phase 4 | Deferred | Minimal visual GUI. Requires stable userspace and graphics/input foundations. |

## Epic Template Expectations

Each epic should include:

- scope and non-goals,
- MINIX comparison,
- Rust rationale,
- required ADRs,
- deferred decisions,
- milestone breakdown,
- test strategy,
- and human-visible demonstration criteria.

## Related Indexes

- [Vision](../vision.md)
- [Architecture](../arch/index_arch.md)
- [ADRs](../arch/adrs/index_adrs.md)
- [Project Management](../pm/index_pm.md)
- [Research](../research/index_research.md)
