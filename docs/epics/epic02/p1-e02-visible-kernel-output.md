# Phase 1 Epic 02: Visible Kernel Output

## Purpose

Give the kernel a tiny, reliable way to communicate with the human running it.

## Human-Visible Result

Aaronix prints a short boot banner. A deliberate panic or fault path produces a clear message before the machine halts or exits.

## Learning Goal

Learn how early kernel diagnostics work and why visible output is an enabling feature, not polish.

## MINIX Comparison

MINIX eventually exposes terminal and console behavior through richer device and TTY layers. Aaronix should not start there. The first output path is a debugging surface for early kernel work.

## Rust Design Focus

Define the smallest output abstraction that supports:

- writing bytes or formatted text,
- panic reporting,
- and test capture from the emulator.

If a trait is introduced, it should be used immediately by the banner and panic path. Do not create a full logging framework yet.

## Decisions Required Now

- First output surface, such as serial, VGA text, framebuffer text, or bootloader console.
- Panic behavior for Phase 1.
- Evidence format for boot logs.

## Deferred

- TTY line discipline.
- `termios` constants.
- Keyboard input.
- Log levels and structured tracing.
- Graphical console.

## Candidate Milestones

| Milestone | Result | Evidence |
| --- | --- | --- |
| 02.1 | Output surface chosen and documented. | ADR or epic note. |
| 02.2 | Boot banner appears. | Serial log or screenshot. |
| 02.3 | Panic path appears and halts predictably. | Captured panic evidence. |
| 02.4 | PM records commands and pass/fail checks. | Updated PM file. |

## Test Strategy

Run the emulator and assert the expected banner text is visible in the captured output. Keep the asserted text small and stable.

## Next Unlock

Epic 03 can now report boot facts and memory data in a way the learner can inspect.

## Related Documents

- [Phase 1 Overview](../p1-overview.md)
- [ADR-0008](../../arch/adrs/0008-observability-and-human-visible-progress.md)
- [ADR-0013](../../arch/adrs/0013-depth-first-phase-1-learning-sequence.md)
