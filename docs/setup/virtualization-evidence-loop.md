# Virtualization Evidence Loop

## Purpose

Aaronix milestone output should not disappear into terminal scrollback. Emulator runs, boot logs, screenshots, and test reports are project data. They should feed back into PM status, design corrections, ADRs, and course material.

This document defines the loop for collecting data from development virtualization back into project analysis.

## Loop

1. Run the milestone command exactly as written in the PM file.
2. Capture automated output: exit status, serial log, test report, and emulator profile.
3. Capture human-visible output when useful: screenshot, short screen transcript, or command prompt behavior.
4. Write an evidence note using [milestone-evidence-template.md](../pm/resources/milestone-evidence-template.md).
5. Store evidence under `docs/pm/resources/` unless the artifact belongs more naturally under another `resources/` directory.
6. Update the PM milestone with pass/fail state and evidence links.
7. If behavior contradicts an epic or ADR, update the design doc or create a new ADR before continuing.
8. If the evidence is course-worthy, link it from the relevant epic or future course outline.

## Evidence Bundle Shape

Use this shape unless a PM file chooses a more specific one:

```text
docs/pm/resources/evidence/
  phase-1/
    <milestone-id>/
      <run-id>/
        evidence.md
        serial.log
        emulator-profile.txt
        test-output.txt
        screenshot.png
```

Recommended `run-id` format:

```text
yyyymmdd-hhmmss-short-description
```

Example:

```text
20260910-211500-boot-banner
```

## Minimum Evidence Note

Every evidence note should record:

- milestone,
- date and timezone,
- git revision or working-tree state,
- command run,
- host OS where relevant,
- toolchain version where relevant,
- emulator and version where relevant,
- artifact under test,
- expected result,
- observed result,
- pass/fail status,
- links to logs, screenshots, or reports,
- and analysis notes.

## What To Capture

Early Phase 1:

- emulator exit code,
- serial output,
- panic/halt reason,
- boot banner or milestone marker,
- binary size and artifact path when relevant.

Later Phase 1:

- process table snapshots,
- syscall traces,
- IPC message traces,
- filesystem image inspection output,
- shell command transcripts,
- and screenshots for display/input milestones.

Phase 1.5:

- emulator test ROM metadata,
- screenshot or frame hash for licensed test ROMs,
- input/timing behavior notes,
- and ROM license evidence for every bundled test asset.

## What Not To Capture

- Secrets or credentials.
- Host-private paths unless needed for debugging and intentionally sanitized later.
- Commercial ROMs or proprietary firmware without redistribution rights.
- Large binary artifacts unless they are required to reproduce an accepted milestone.

## Analysis Use

Evidence should drive project decisions:

- A passing milestone updates PM status.
- A repeated failure may create a research note.
- A behavior mismatch may update an epic.
- A boundary or compatibility surprise may create or amend an ADR.
- A clean visual milestone may become course material.

## Deferral

Automated evidence upload, dashboards, long-term artifact retention, and performance trend analysis are deferred until Aaronix has stable Phase 1 test commands.
