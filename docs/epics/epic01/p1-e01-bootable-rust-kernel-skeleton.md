# Phase 1 Epic 01: Bootable Rust Kernel Skeleton

## Purpose

Produce the first bootable Aaronix artifact: an emulator reaches a Rust kernel entry point and stops in a controlled way.

## Human-Visible Result

The emulator run proves that control reached Aaronix Rust code. The proof may be an emulator exit code, debug-port signal, serial marker, or other minimal observable agreed in the boot ADR.

## Learning Goal

Learn the minimum path from firmware or bootloader into Rust. The lesson should stay focused on "how does the machine start our code?"

## MINIX Comparison

MINIX begins from a mature source tree with established boot and kernel assumptions. Aaronix should instead isolate the first boot handoff and explain how much startup machinery is truly needed before any kernel service exists.

## Rust Design Focus

Use a tiny `no_std` Rust entry path and the smallest architecture-specific boundary required by the selected boot route.

This epic should avoid:

- broad architecture modules,
- process structures,
- memory managers,
- filesystem code,
- syscall tables,
- and translated MINIX headers.

## Decisions Required Now

- Bootloader or boot protocol for Phase 1.
- Initial CPU architecture target.
- Kernel artifact format.
- Minimum assembly exception, if any, and why Rust cannot cover it.

## Deferred

- Multiprocessor startup.
- Full interrupt setup.
- Heap allocation.
- Userspace.
- MINIX process table.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 01.1 | Boot approach recorded in an ADR. | ADR link. |
| 01.2 | Minimal Rust kernel entry exists. | Source review and build output. |
| 01.3 | Emulator reaches the entry point. | Exit code or log marker. |
| 01.4 | PM records the exact command and artifact path. | Updated PM file. |

## Test Strategy

Use the workbench from Epic 00. The acceptance test is not that the OS is useful yet; it is that the machine reliably reaches Aaronix-owned Rust code.

## Next Unlock

Epic 02 can add early output because there is now a running kernel context to speak from.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
- [Virtualization Evidence Loop](../../setup/virtualization-evidence-loop.md)
