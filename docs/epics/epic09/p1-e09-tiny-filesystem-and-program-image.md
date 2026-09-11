# Phase 1 Epic 09: Tiny Filesystem and Program Image

## Purpose

Give Aaronix a tiny read-only source of named bytes that can later hold programs and shell-visible files.

## Human-Visible Result

The system lists file names from a small image and reads at least one file's contents.

## Learning Goal

Learn how an OS moves from hard-coded behavior to named system state without implementing a full filesystem too early.

## MINIX Comparison

MINIX includes filesystem headers, calls, permissions, and on-disk structures. Aaronix should study those when needed, but the first filesystem milestone should be deliberately tiny and testable.

## Rust Design Focus

Define a minimal image format or choose a small existing-compatible format only after the epic explains why. The first reader should expose only list/read behavior needed by the next shell epic.

Do not create write support, permissions, timestamps, devices, or path normalization until a milestone needs them.

## Decisions Required Now

- Whether the first image is custom educational, MINIX-compatible subset, or another tiny format.
- Host tool boundary for creating the image.
- Read-only file API needed by the first shell.

## Deferred

- Writable filesystem.
- Directories beyond the first listing need.
- Permissions and ownership.
- Device nodes.
- Mounting.
- Buffer cache.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 09.1 | Filesystem image choice recorded. | ADR link if boundary is fixed. |
| 09.2 | Host tool builds a tiny image. | Tool output and image manifest. |
| 09.3 | Kernel or service lists file names. | Emulator log. |
| 09.4 | Kernel or service reads a file. | Emulator log and host test. |

## Test Strategy

Use host tests for image construction and parsing. Use emulator evidence for the OS-visible list/read path.

## Next Unlock

Epic 10 can load commands, display files, and make the shell feel like an operating environment instead of a fixed demo.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0005](../../arch/adrs/0005-boundary-type-separation-policy.md)
- [ADR-0007](../../arch/adrs/0007-contract-tests-for-observable-boundaries.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
