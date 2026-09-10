# Milestone Evidence Template

Use this template for milestone evidence captured from host tests, emulator smoke tests, and manual demonstrations.

````markdown
# Evidence - <Milestone ID> - <Short Description>

## Summary

- Milestone:
- Date:
- Timezone:
- Result: Pass | Fail | Partial
- Git revision or working-tree state:

## Command

```text
<exact command>
```

## Environment

- Host OS:
- Rust toolchain:
- Emulator:
- Target architecture:
- Boot profile:

## Artifact Under Test

- Kernel image:
- Disk image:
- Userspace artifact:
- Other:

## Expected Result

Describe the behavior the milestone promised.

## Observed Result

Describe what actually happened.

## Captured Files

- Serial log:
- Test output:
- Emulator profile:
- Screenshot:
- Other:

## Analysis

Explain what this evidence means. If it changes the design, link the epic or ADR update.

## Follow-Up

- [ ] Follow-up task, if needed.
````

## Related Docs

- [Virtualization evidence loop](../../setup/virtualization-evidence-loop.md)
- [Test environment setup](../../setup/test-environment.md)
- [Project Management](../index_pm.md)
