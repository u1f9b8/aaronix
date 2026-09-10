# Aaronix Research

Research files collect source-study notes, external references, tradeoff analysis, licensing evidence, and technical experiments that inform epics and ADRs.

## Primary Source Material

- [MINIX implementation book PDF](../resources/OperatingSystems-Design-and-Implementation-3ed.pdf) - original OS design and implementation reference, with source listings beginning around page 658 of the document.
- [General resource index](../resources/index_resources.md)

## Research Backlog

| Topic | Why it matters | Target link |
| --- | --- | --- |
| MINIX source map | Phase 1 epics need source-study anchors for boot, kernel, process, memory, IPC, filesystem, and userspace. | Future research notes under this folder. |
| Rust OS implementation patterns | We need idiomatic Rust alternatives to C headers, macros, global state, and unsafe hardware access. | Future research notes under this folder. |
| SNES emulator references and ROM licensing | Phase 1.5 may bundle only original or redistributable ROM/test assets. | [Phase 1.5 epic](../epics/phase-1-5-rust-snes-emulator-showcase.md) |
| Linux ABI compatibility | Phase 2 depends on syscall, executable, filesystem, signal, and process semantics. | Deferred until Phase 2 planning. |
| Windows executable/runtime compatibility | Phase 3 depends on PE/COFF, loader, NT runtime assumptions, and process semantics. | Deferred until Phase 3 planning. |

## Related Resources

- [Research resources](resources/index_research_resources.md)

## Related Indexes

- [Vision](../vision.md)
- [Epics](../epics/index_epics.md)
- [ADRs](../arch/adrs/index_adrs.md)
- [Project Management](../pm/index_pm.md)
