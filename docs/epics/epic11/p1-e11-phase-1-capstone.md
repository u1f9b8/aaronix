# Phase 1 Epic 11: Phase 1 Capstone

## Purpose

Close Phase 1 as a coherent, testable, teachable operating-system foundation.

## Human-Visible Result

A recorded Aaronix run boots from the chosen emulator path, shows kernel diagnostics, enters userspace, starts a shell, lists files, and runs at least one tiny command or program.

## Learning Goal

Learn how to consolidate an OS milestone without expanding scope: finish the story, collect evidence, update PM, and identify deferred decisions honestly.

## MINIX Comparison

The capstone should revisit the MINIX intent at the system level: small cooperating components, clear kernel boundaries, and practical userspace behavior. It should not claim full MINIX compatibility.

## Rust Design Focus

Stabilize only the interfaces needed by the Phase 1 demo. Clean up code only when it improves clarity, removes demonstrated duplication, or protects a boundary already exercised by tests.

## Decisions Required Now

- What counts as Phase 1 complete.
- Which interfaces are stable enough for Phase 1.5.
- Which gaps become Phase 2 or later work.

## Deferred

- Linux binary compatibility.
- Windows runtime support.
- GUI.
- SNES emulator integration into Aaronix runtime.
- Full POSIX behavior.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 11.1 | End-to-end demo command is documented. | PM and setup docs. |
| 11.2 | Tests and emulator checks pass. | Test reports and logs. |
| 11.3 | Boot-to-shell evidence is captured. | Screenshot, transcript, or recording reference. |
| 11.4 | Deferred-decision register is updated. | ADR/PM/doc links. |

## Test Strategy

Run the full host and emulator evidence loop. Treat screenshots and transcripts as acceptance evidence, not decoration.

## Next Unlock

Phase 1.5 can start because Aaronix now has enough runtime shape to reason about a Rust SNES emulator as a userspace or hosted demo product.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [Phase 1.5 Rust SNES Emulator Showcase](../phase-1-5-rust-snes-emulator-showcase.md)
- [ADR-0011](../../arch/adrs/0011-rust-snes-emulator-showcase-phase-1-5.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
- [Project Management](../../pm/index_pm.md)
