# Development Environment Setup

## Purpose

This document describes the development environment expected for Aaronix contributors. It is intentionally conservative because Phase 1 has not yet chosen the first CPU target, bootloader, image format, or emulator command.

When implementation begins, the repository should pin tool versions in source-controlled files such as `rust-toolchain.toml`, workspace configuration, and runner scripts. Until then, this document defines the required categories of tools and the setup discipline.

## Baseline Tools

Install or make available:

- Git.
- Rust through `rustup`.
- Rust components expected for systems work: `rustfmt`, `clippy`, `rust-src`, and LLVM tools when needed.
- A system emulator, likely QEMU, once the boot ADR chooses the first target.
- Binary inspection tools such as `readelf`, `objdump` or `llvm-objdump`, `objcopy`, `nm`, and `file`.
- A debugger such as GDB or LLDB.
- Common shell tools used by the repo checks, such as `rg`, `find`, `sed`, and `awk`.
- A Markdown editor suitable for working with linked docs. Obsidian is useful but not required.

Do not treat this list as the final Phase 1 tool contract. The first boot epic should turn it into exact commands after the target and boot path are selected.

## First-Time Contributor Flow

1. Read [the project vision](../vision.md).
2. Read [the engineering mandate](../Engineering.md).
3. Read [the ADR index](../arch/adrs/index_adrs.md).
4. Read the relevant epic under [epics](../epics/index_epics.md).
5. Read the corresponding PM file under [pm](../pm/index_pm.md) once one exists.
6. Confirm that any needed design decision already has an ADR.
7. Set up the test environment described in [test-environment.md](test-environment.md).

## Tool Pinning Policy

Once code begins, tool versions should be pinned where practical:

- Rust toolchain: `rust-toolchain.toml`.
- Cargo configuration: `.cargo/config.toml` when target-specific settings are needed.
- Build and test commands: checked-in scripts or task recipes.
- Emulator profiles: source-controlled configuration or scripts, not one-off shell history.

Pinned tools make course instructions reproducible and make emulator evidence comparable across machines.

## Local Build Discipline

Before Phase 1 code exists, there is no canonical build command.

Once code begins, every active PM milestone should name the exact commands needed to:

- format,
- lint,
- type-check,
- run host tests,
- build boot artifacts,
- run emulator smoke tests,
- and collect evidence.

## Environment Variables

Host-side tools may use the `AARONIX__SECTION__FIELD` naming convention described in [ADR-0006](../arch/adrs/0006-configuration-and-boot-parameter-precedence.md). These variables configure host tools unless a later ADR explicitly exposes them to Aaronix itself.

Suggested future host variables:

- `AARONIX__DEV__PROFILE` for local profile selection.
- `AARONIX__EVIDENCE__DIR` for overriding local evidence output before curation into `docs/pm/resources/`.
- `AARONIX__EMULATOR__PATH` when the emulator binary is not on `PATH`.

These names are suggestions, not active contract, until code or PM files adopt them.

## Non-Goals

- This document does not choose the bootloader.
- This document does not choose the first CPU architecture.
- This document does not require Linux, Windows, or macOS host parity yet.
- This document does not install dependencies automatically.
