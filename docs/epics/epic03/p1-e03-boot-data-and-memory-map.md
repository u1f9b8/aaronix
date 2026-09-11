# Phase 1 Epic 03: Boot Data and Memory Map

## Purpose

Teach the kernel to read the minimum boot-provided facts it needs, starting with the physical memory map.

## Human-Visible Result

Aaronix prints a small memory summary, such as the number of usable regions and their total size.

## Learning Goal

Learn how a kernel receives facts from its boot environment and why it should not invent memory ownership.

## MINIX Comparison

MINIX relies on platform and boot-time knowledge to initialize kernel memory. Aaronix should study the same concern, but map it into explicit Rust boundary types for the selected boot protocol.

## Rust Design Focus

Represent boot data with narrow, validated types. Keep protocol-specific layouts at the boot boundary and convert into Aaronix-owned domain types before the rest of the kernel uses them.

Only the fields needed by this epic should be parsed.

## Decisions Required Now

- Boot-data boundary type.
- Memory-region classification used by the first allocator.
- How invalid or missing boot data is reported.

## Deferred

- Virtual memory.
- Heap allocation.
- Memory-mapped device catalog.
- ACPI or firmware table exploration unless needed by the chosen boot route.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 03.1 | Minimal boot-data type defined. | Source review. |
| 03.2 | Usable memory regions are parsed. | Emulator log. |
| 03.3 | Invalid boot-data path is testable with a fake. | Host test output. |
| 03.4 | PM records the accepted memory summary. | Updated PM file. |

## Test Strategy

Use host-side tests for boot-data conversion where possible, and emulator evidence for the real boot path.

## Next Unlock

Epic 04 can allocate physical frames from known usable memory instead of guessing.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0005](../../arch/adrs/0005-boundary-type-separation-policy.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
