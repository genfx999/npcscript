---
layout: documentation
title: "Runtime debugging"
section: "Support"
order: 9
permalink: /runtime-debugging/
---

Select a scene runner or program asset and open the Program Editor. Validation reports structural errors, missing function selections, missing argument bindings and expression errors. Runtime-only objects are resolved during execution. An asset that validates can still fail if a named object or component is missing in the active scene.

In Play Mode, **Run** starts the assigned program, **Step** processes one instruction attempt, **Stop** cancels the current runner, and **Restart** starts from the first row again. A Step on an unfinished wait leaves the instruction pointer there. Set **Run On Start** off if you want to begin with Step.

The highlighted row follows the active instruction while the displayed program matches the runner's active program. When a call enters another program, the runner status shows the active program name and line. Folded parents temporarily open to reveal the current instruction. Check the Unity Console for logs, warnings and invocation stacks. Debug diagnostics are also available through `LastError`, `CurrentInstructionIndex` and `CurrentActivityDisplayName`.
