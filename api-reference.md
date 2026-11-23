---
layout: documentation
title: "API reference"
section: "Reference"
order: 8
permalink: /api-reference/
description: "Saved member and object references use scene hierarchy paths. Rename or reparent referenced objects with care across save versions. Local variables and..."
---

| API | Use |
| --- | --- |
| `NpcScripting.NpcScript.Program` | Assign the program to a scene runner. |
| `Run()`, `Restart()`, `Stop()` | Start, restart or stop execution. `Run` returns whether startup succeeded. |
| `Step()`, `Tick(instructionBudget)` | Advance manually for tools and tests. Enabled runners already tick themselves. |
| `RunProgramAsInterruption(program)` | Run a parameterless interruption program on this runner. Its invocation frame allows the prior program to resume. |
| `ActiveProgram`, `CurrentInstructionIndex`, `InvocationDepth`, `CurrentActivityDisplayName` | Inspect live state. Instruction indexes are zero-based. |
| `IsRunning`, `IsPaused`, `HasCompleted`, `LastError` | Inspect execution and failures. |
| `LocalVars`, `MemberVars`, `globalVars` | Read scoped variables. The global dictionary is also the host integration point for shared values. |
| `DialogueText`, `PromptText`, `PromptChoices`, `SubmitResponse(int)` | Integrate a custom dialogue UI. |
| `NpcScriptProgram.Instructions`, `EnsureInstructionIds()` | Author assets programmatically and ensure stable row IDs before execution. Preserve IDs when editing existing rows. |
| `NpcScriptFunctionAttribute` | Expose a public instance method. Configure command, condition, expression and waiting eligibility. |
| `NpcScriptVariablePersistence.CaptureGlobals()`, `CaptureMembers()`, `Apply(globals, members)` | Serialize save DTOs with your own system. Use `ClearForNewSession()` to clear the shared session state explicitly. |
| `NpcScriptLocalizationFileBuilder.Build(program, language)`, `Localization.CurrentLanguageCode`, `Localizer.BuildVoiceLines(program)` | Export text, select a language and get voice filenames. |

Saved member and object references use scene hierarchy paths. Rename or reparent referenced objects with care across save versions. Local variables and the instruction pointer are transient. Programs are project-authored executable data. The expression language can access public instance component members, so keep untrusted input out of executable expressions.
