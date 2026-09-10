---
status: Accepted
date: 2026-09-10
phase: Phase 1
source: Adapted from Argaile ADR-0024
---

# ADR-0006 - Configuration and Boot Parameter Precedence

## Context

Argaile used strict configuration tiers to keep defaults, environment overrides, and runtime values understandable. Aaronix has a different configuration problem: early OS configuration is split across compile-time defaults, target/build settings, bootloader-provided information, kernel command-line parameters, and eventually persistent runtime configuration.

For Phase 1, configuration must help the project progress without hiding important decisions. A bootable kernel should fail loudly when a required parameter is missing or malformed. A course reader should be able to tell which value came from source defaults, build configuration, or the boot path.

## Decision

Aaronix uses strict precedence for configuration and boot parameters.

From lowest to highest precedence:

```text
Tier 1: compiled defaults
  - safe constants in source
  - minimal behavior for local development and early boot

Tier 2: target/build configuration
  - architecture target
  - emulator profile
  - image layout
  - feature flags selected at build time

Tier 3: boot/runtime parameters
  - bootloader-provided memory map and modules
  - kernel command line
  - emulator-supplied test parameters
  - later: init-provided runtime configuration
```

Rules:

1. Later tiers override earlier tiers.
2. The source of a final value should be inspectable in tests or diagnostics.
3. Required boot information must be validated before the kernel relies on it.
4. Invalid production-like boot configuration fails early and visibly.
5. Host-tool environment variables may use the `AARONIX__SECTION__FIELD` convention, but that convention is for host tools unless a later ADR adopts it inside Aaronix.
6. Secrets are not a Phase 1 kernel concern. If later phases introduce credentials for networked compatibility/runtime services, they need a separate ADR.

## Consequences

Good:

- Boot and emulator behavior become reproducible.
- Target-specific differences stay explicit.
- Test harnesses can inject configuration without hardcoding new behavior.
- The course can show configuration precedence before the OS has a filesystem.

Costs:

- Even early boot code needs a small validation story.
- Build scripts and host tools must avoid silently overriding kernel assumptions.

## Deferral

Persistent runtime configuration is deferred until Aaronix has enough filesystem and init/userspace structure to store and reload it intentionally.

Operator-facing configuration for Phase 2 Linux compatibility, Phase 3 Windows runtime support, and Phase 4 GUI behavior will require additional ADRs when those phases begin.

## Aaronix Review

The Argaile three-tier configuration ADR was useful for precedence discipline. YAML, database, service deployment, OIDC, TLS, and production-enterprise validators were removed. The decision now maps to bootable OS concerns.
