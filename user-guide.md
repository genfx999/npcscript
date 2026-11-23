---
layout: documentation
title: "User guide and examples"
section: "Start here"
order: 2
permalink: /user-guide/
---

Programs are ScriptableObjects. Each scene runner points at a program asset. Multiple runners can share a program while keeping separate member and local variables.

Use the + button to insert instructions, **If block** or **While block** for matching end markers, and the row buttons to copy, paste, move or remove an instruction. **If** copy uses the whole If block and pastes fresh IDs. Jump targets use stable instruction IDs. The foldout triangle hides fields or a branch body. **Else** folds independently. **Expand All** and **Collapse All** affect presentation only.

Locals belong to an invocation. Members belong to a runner. Globals are shared within the play session, including an implicit read-only `Player` lookup. Ordinary declarations start with numeric zero. Enter expressions such as `Local.Count + 1`, `Min(5, Global.Score)`, or `Parameter.Speaker.position.y`. Text interpolation uses braces, as in `Visit {Member.Visits}`. Names with spaces can use backticks in expressions.

**Invoke** runs another program asset or a program stored in a variable. Put parameter declarations first. Input arguments can be expressions. An in/out argument must be a writable variable reference, such as `Local.Counter`. A call can target this runner or another runner found through a scene transform. Waiting external calls resume when the target invocation completes. Stop the target itself if a separate runner must end an in-flight action.

| Scene | Explore |
| --- | --- |
| 01 Interactive Dialogue | A permission prompt, two branches, local state and a gate action. |
| 02 Actions and Sequencing | Baked navigation, animation completion, a one-shot sound, visibility and prop tweening. |
| 03 Cutscene Direction | Camera position, dolly/tracking, character movement, zoom, dialogue and handoff. |
| 04 Custom CSharp Integration | Attributed commands, a boolean getter and a frame-by-frame waiting command. |
| 05 Advanced Scripting | A while loop, local and shared state, expression arguments, and an in/out child call. |
| Beacon Demo | A shorter dialogue and a shared reply program. |

Transform fields accept a scene object name, `this`, `TargetTransform`, a scoped variable, or `null` where the command allows that value. Use unique scene object names for name-based references. When a name is computed, use a Set expression such as `GameObject.Find("Indicator " + Local.Step)` to store the object in a local and pass that local to a transform field.
