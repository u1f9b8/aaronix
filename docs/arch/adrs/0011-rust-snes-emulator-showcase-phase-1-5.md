---
status: Accepted
date: 2026-09-10
phase: Phase 1.5
source: New Aaronix decision
---

# ADR-0011 - Rust SNES Emulator Showcase for Phase 1.5

## Context

Aaronix Phase 1 is about building the smallest credible MINIX-inspired OS in Rust. A serious interactive userspace program would be valuable immediately after that foundation exists because it exercises many OS surfaces at once: executable loading, memory pressure, input, timers, framebuffer or graphics output, audio, file access, and process lifecycle.

A Super Nintendo Entertainment System emulator is a strong Phase 1.5 showcase because it is technically rich, understandable to users, and attractive as course material. It should not, however, distort Phase 1 kernel sequencing or introduce copyright risk.

## Decision

Aaronix will include a Phase 1.5 scope for a Rust SNES emulator showcase, tentatively named `aaronix-snes`.

The emulator is:

- a userspace/demo product, not part of the kernel;
- written in Rust from scratch;
- self-contained enough to run as a normal Linux binary;
- portable across a Linux host adapter and a future Aaronix userspace adapter;
- structured with an emulator core separated from platform I/O;
- and used as a capstone demonstration that Aaronix can run a meaningful interactive program.

The emulator is not:

- a fork, port, translation, or derivative of ZSNES unless a later ADR deliberately accepts that license and maintenance burden;
- a reason to ship commercial ROMs;
- a Phase 1 blocker;
- or a kernel feature.

ROM policy:

1. Aaronix must not ship commercial SNES ROMs without explicit distribution rights.
2. Bundled ROMs, if any, must be homebrew, public-domain, open-license, commissioned, or original test/demo ROMs with license evidence stored in `resources/` next to the asset.
3. The emulator may provide a documented path for users to load their own legally obtained ROMs.
4. Course examples should prefer original or clearly redistributable ROMs.

## Consequences

Good:

- Phase 1.5 gives Aaronix a highly visible proof that the OS can support non-trivial userspace.
- A Linux-first binary lets emulator development progress before Aaronix has every runtime surface.
- The adapter split naturally teaches portable systems programming in Rust.
- The project avoids making copyright infringement part of the default distribution.

Costs:

- A from-scratch emulator is substantial work and must not compete with Phase 1 kernel goals.
- Accurate SNES emulation has deep CPU, PPU, APU, timing, and cartridge-mapper complexity.
- Audio/video/input support in Aaronix may lag behind the Linux version until the OS catches up.

## Deferral

Phase 1.5 starts only after Phase 1 has enough userspace, file access, timing, and display/input capability to make the Aaronix port meaningful.

The first emulator milestone can run on Linux only. The Aaronix-native adapter begins when the Phase 1 OS exposes the necessary ABI.

Networked game libraries, save-state UX, shaders, debugger UI, and large ROM catalogs are deferred beyond Phase 1.5.

## MINIX Comparison

MINIX is not concerned with game-console emulation. The relevant MINIX lesson is architectural: a clear boundary between kernel services and user programs lets complex software run without becoming kernel code.

## Rust Rationale

Rust is a good fit for an emulator because CPU, memory bus, cartridge, PPU, and APU components can be modeled with explicit ownership and narrow mutable state. Platform adapters keep unsafe OS/video/audio calls out of the emulation core.

## References

- [Nintendo Intellectual Property and Piracy FAQ](https://en-americas-support.nintendo.com/app/answers/detail/a_id/55888/~/intellectual-property-%26-piracy-faq)
- [U.S. Copyright Office - Copyright and Digital Files FAQ](https://www.copyright.gov/help/faq/faq-digital.html)
- [U.S. Copyright Office - 37 CFR 201.40](https://www.copyright.gov/title37/201/37cfr201-40.html)
- [ZSNES GPL-2 license listing](https://github.com/xyproto/zsnes/blob/main/COPYING)

## Aaronix Review

This is a new Aaronix ADR prompted by Phase 1.5 planning. ZSNES is treated as historical inspiration only, not source material. Commercial ROM bundling is rejected unless distribution rights are obtained.
