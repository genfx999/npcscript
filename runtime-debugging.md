---
layout: documentation
title: "Runtime debugging"
section: "Support"
order: 9
permalink: /runtime-debugging/
description: "Set NpcScript breakpoints, step through instructions, and inspect paused NPCs in Unity Play Mode."
---

Open **Tools → NpcScript → Program Editor** and select a scene runner or a saved program asset. The validation bar reports structural errors, missing function selections, missing argument bindings, and expression errors. Runtime references still need to resolve in the active scene.

## Breakpoints

Click the gutter to the left of a row to add or remove a breakpoint. Right-click the gutter or the row to toggle, enable, disable, or remove it. A red circle marks an enabled breakpoint and a gray circle marks a disabled one. Breakpoints are saved in local Unity Editor preferences using the program asset and instruction IDs. They follow an instruction when its line number changes, and pasted rows receive new IDs. Deleting a row makes its breakpoint inactive. Undo can restore the original row and breakpoint together.

Calls, prompts, conditions, loop headers, invocations, variable operations, jumps, and exits can have breakpoints. Comments, context and parameter declarations, `Else`, `End If`, and `End While` have no breakpoint toggle. The debugger can still show a step landing on one of those rows if the interpreter reaches it.

A small dot on a collapsed block shows that its hidden rows contain a breakpoint. A red dot indicates at least one enabled breakpoint. When the runner pauses, the editor temporarily reveals any folded parent rows and scrolls the instruction into view. Your saved foldout choices are restored when the editor stops following that instruction. A yellow arrow and outline show where the selected runner is paused, including when there is also a red breakpoint circle on the same row.

![Beacon Dialogue paused on Tween to Y. The IF breakpoint is red, the disabled Tween breakpoint is gray, the next instruction has a yellow arrow, and Debug Inspector shows Response 1.]({{ '/assets/breakpoint-debugger.png' | relative_url }})

## Control a runner

When a breakpoint is hit, NpcScript pauses immediately before executing that instruction. Unity continues updating the scene and other runners remain independent. If several runners pause, use **Paused executions** to choose one. The editor opens that runner's active program, including a nested program.

- **Continue** runs the paused instruction and then continues until another breakpoint, program completion, or a stop. A later iteration of a loop can hit the same breakpoint again.
- **Pause** requests a stop at the next safe instruction boundary. If a row is waiting, the toolbar shows that the pause is pending until the row completes.
- **Step Over** runs the current instruction to completion and pauses before the next instruction at the same call depth or in its caller.
- **Stop** cancels the selected runner using its normal cleanup. **Run** starts its assigned program and **Restart** starts it over. The `Step()` C# API still runs a single instruction attempt during ordinary manual stepping, including when a wait is unfinished.

### Step from the beginning

Disable **Run On Start** on the runner, enter Play Mode, and select the runner in the Program Editor. Click **Step Over** to execute its first instruction and pause before the next one. **Step Over** also starts from the beginning after a stop or completion. It passes an enabled breakpoint on the first instruction once, so the first click makes progress. To pause before that first instruction, set a breakpoint there and click **Run**.

### Waits and nested calls

A same-runner `Invoke` runs through the child before Step Over pauses in the caller. A breakpoint in the child can still stop execution. A waiting `Invoke` to another NPC resumes the caller after that NPC's child finishes. The other NPC can hit its own breakpoint and must be resumed independently. With waiting off, the caller continues to its next instruction once it starts the other NPC.

Pausing NpcScript does not freeze Unity's simulation. Any non-waiting movement, tweens, animations, sounds, or custom coroutines already in progress can continue. A prompt that has started needs a response before a pending Pause or Step Over can advance. An external program can temporarily interrupt a runner. NpcScript restores the original paused frame when the external program returns.

## Inspect variables

Expand **Debug Inspector** below the instruction list. It displays the paused NPC, active program, line, instruction ID, and invocation depth. The read-only rows show each value's scope, name, value, and type. **Local** and **Parameter** values come from the active call frame. **Member** values belong to the selected NPC. **Global** values are shared and can change while this NPC is paused. **Dialogue / Response** shows the last one-based prompt choice for this NPC.

Check the Unity Console for errors, warnings, and invocation stacks. Runner diagnostics are also available through `LastError`, `CurrentInstructionIndex`, and `CurrentActivityDisplayName`. Breakpoints and the Debug Inspector are available in Editor Play Mode. Player builds omit the debugger. Step Into, Step Out, and Run to Here are outside this version.

## Try it in the Beacon Demo

1. Open **Beacon Demo** and select the **Beacon Dialogue** program. Set a breakpoint on `if Response == 1`.
2. Enter Play Mode and choose **Raise it.** The runner pauses on the IF, the beacon stays at its starting height, and **Dialogue / Response** in Debug Inspector reads `1`.
3. Click **Step Over** to reach **Tween to Y**. Click **Step Over** again to start the tween and pause on the following **Say**. The beacon rises while the program remains paused. Click **Continue** to finish the branch and hear or read the shared reply.
4. Exit and re-enter Play Mode to reset the beacon. Choose **Leave it down.** The same breakpoint is hit with a response of `2`. **Step Over** goes to the other branch. **Continue** completes the shared reply with no beacon motion.
5. To try two NPCs with the same program, duplicate the Beacon Keeper for this session and disable **Run On Start** on the duplicate. Answer the first NPC's prompt, then select the duplicate and choose **Run**. After answering its prompt, switch between the two entries in **Paused executions**. Continue or stop each runner independently. The built-in dialogue UI displays one prompt at a time, so submit the first NPC's response before starting the second prompt.
