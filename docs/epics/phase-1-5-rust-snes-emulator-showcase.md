# Phase 1.5 Epic - Rust SNES Emulator Showcase

## Purpose

Build a Rust SNES emulator, tentatively named `aaronix-snes`, as a post-Phase-1 userspace showcase. The emulator should prove that Aaronix can eventually run a substantial interactive program without turning that program into kernel code.

This epic is intentionally Phase 1.5, not Phase 1. Phase 1 remains focused on the basic MINIX-inspired OS foundation.

## Scope

The emulator is:

- written in Rust from scratch,
- self-contained enough to run as a Linux binary,
- structured as a portable emulation core plus platform adapters,
- ported to Aaronix userspace only after the OS exposes the necessary file, timer, input, video, and optionally audio surfaces,
- and used as a course capstone for portable systems programming.

The emulator is not:

- part of the Aaronix kernel,
- a fork, translation, or derivative of ZSNES,
- a reason to ship commercial ROMs,
- a prerequisite for Phase 1 completion,
- or a replacement for MINIX-inspired userspace tools.

## ROM Policy

Aaronix must not bundle commercial SNES ROMs unless explicit redistribution rights are obtained.

Bundled ROMs, if any, must be one of:

- original Aaronix demo ROMs,
- commissioned ROMs with written redistribution rights,
- homebrew ROMs with compatible licenses,
- public-domain ROMs with source/license evidence,
- or hardware/test ROMs whose license permits redistribution.

The emulator may let users load their own legally obtained ROM files.

## Design Direction

The emulator should be split into:

- `core`: CPU, bus, memory map, cartridge mapping, PPU, APU, timers, and deterministic stepping;
- `platform-linux`: window, audio, input, filesystem, and host configuration for Linux;
- `platform-aaronix`: Aaronix userspace ABI integration once available;
- `test-roms`: licensed or original ROMs used for automated and course-visible checks;
- `tools`: optional inspectors, trace diffing, and ROM metadata utilities.

The core must not depend on Linux, Aaronix, SDL, a windowing library, or a filesystem API directly. Platform adapters own those dependencies.

## Dependencies From Phase 1

The Aaronix-native adapter should wait for enough Phase 1 capability to exist:

- userspace binary loading,
- file read access or a ROM-loading equivalent,
- monotonic timing,
- keyboard/controller input,
- framebuffer or graphics output,
- process termination behavior,
- and optionally audio output.

The Linux adapter can begin earlier if treated as a separate host binary, but it must not distract from Phase 1 kernel sequencing.

## Candidate Milestones

| Milestone | Description | Visible evidence |
| --- | --- | --- |
| M1 | Linux-hosted emulator skeleton with cartridge header parsing and deterministic CPU stepping tests. | Unit tests and CLI ROM metadata output. |
| M2 | Basic graphics path with test ROM output on Linux. | Windowed or framebuffer screenshot from a licensed test ROM. |
| M3 | Input, timing, and save/load primitives. | Playable homebrew/demo ROM on Linux. |
| M4 | Aaronix platform adapter spike. | Same test ROM starts under Aaronix once OS surfaces exist. |
| M5 | Course capstone packaging. | Lesson-ready demo, license ledger, and reproducible run command. |

## ADRs

- [ADR-0011 - Rust SNES Emulator Showcase for Phase 1.5](../arch/adrs/0011-rust-snes-emulator-showcase-phase-1-5.md)
- [ADR-0003 - Kernel, Userspace, and Tooling Boundaries](../arch/adrs/0003-kernel-userspace-and-tooling-boundaries.md)
- [ADR-0007 - Contract Tests for Observable Boundaries](../arch/adrs/0007-contract-tests-for-observable-boundaries.md)
- [ADR-0009 - Explicit and Versioned ABI Boundaries](../arch/adrs/0009-explicit-and-versioned-abi-boundaries.md)

## Research Notes Needed

- SNES hardware documentation sources suitable for implementation reference.
- License ledger for every bundled ROM or test asset.
- Emulator accuracy test strategy.
- Linux platform adapter library decision.
- Aaronix userspace ABI requirements for video, input, timing, file loading, and audio.

## References

- [Nintendo Intellectual Property and Piracy FAQ](https://en-americas-support.nintendo.com/app/answers/detail/a_id/55888/~/intellectual-property-%26-piracy-faq)
- [U.S. Copyright Office - Copyright and Digital Files FAQ](https://www.copyright.gov/help/faq/faq-digital.html)
- [U.S. Copyright Office - 37 CFR 201.40](https://www.copyright.gov/title37/201/37cfr201-40.html)
- [ZSNES GPL-2 license listing](https://github.com/xyproto/zsnes/blob/main/COPYING)
