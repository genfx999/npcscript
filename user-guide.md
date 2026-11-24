---
layout: documentation
title: "User guide and examples"
section: "Start here"
order: 2
permalink: /user-guide/
description: "Programs are ScriptableObjects. Each scene runner points at a program asset. Multiple runners can share a program while keeping separate member and..."
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

## Save and load NpcScript state

Use `NpcScriptVariablePersistence` to include NpcScript in your game's save file. `CaptureGlobals()` collects shared variables such as `Global.QuestStage`. `CaptureMembers()` collects per-runner variables such as `Member.HasGreeted`, along with cached member records for unloaded NPCs that NpcScript has already seen. `Apply(globals, members)` restores these values so programs can read them when they start again. `Player` is looked up from the scene when needed.

The example below serializes the NpcScript portion of a save as JSON. Your save manager can store that string or put the two lists directly into its own save model.

```csharp
using System;
using System.Collections.Generic;
using NpcScripting;
using UnityEngine;

public static class NpcScriptSaveSection
{
    [Serializable]
    private sealed class Data
    {
        public List<NpcScriptVariablePersistence.SavedVariable> globals;
        public List<NpcScriptVariablePersistence.SavedMember> members;
    }

    public static string CaptureJson()
    {
        var data = new Data
        {
            globals = NpcScriptVariablePersistence.CaptureGlobals(),
            members = NpcScriptVariablePersistence.CaptureMembers()
        };
        return JsonUtility.ToJson(data);
    }

    public static void ApplyJson(string json)
    {
        if (string.IsNullOrWhiteSpace(json))
            return;

        var data = JsonUtility.FromJson<Data>(json);
        if (data != null)
            NpcScriptVariablePersistence.Apply(data.globals, data.members);
    }
}
```

**Save at a checkpoint.** Call `CaptureJson()` after your game has set the variables that mark the checkpoint. Store the result with the rest of your save data, such as the scene and player position.

**Restore before starting programs.** For a fresh game or when switching save slots, call `NpcScriptVariablePersistence.ClearForNewSession()` before the destination scene's runners initialize. Turn off **Run On Start** for runners your save manager starts. Load the scene, recreate any referenced objects and load any referenced assets, then call `ApplyJson()` on Unity's main thread. Start the relevant runners with `Run()` once the restore is complete. Member records for other scenes are applied when a matching runner initializes later.

**Save progress in globals or members.** Local variables, invocation parameters, the instruction pointer, the call stack, pending prompts, and in-flight actions are transient. Store durable progress such as `Global.QuestStage = 3` or `Member.HasGreeted = true` in a global or member. After loading, start the program from the beginning and branch on those values. Save the actual world state, such as whether a door is open or where an NPC is standing, in your game's save system.

The persistence helper supports `null`, booleans, `int`, `float`, `double`, strings, enums, and Unity object references. Unsupported values such as vectors and custom objects are skipped with a warning. Store those values in your own save format or break them into supported fields. Member IDs and scene references depend on scene paths, hierarchy names, and the order of same-named siblings. Asset references resolve by type and name among loaded objects. Use stable game IDs in your own save model when objects are dynamically spawned or renamed. An unresolved Unity reference is restored as `null` and logged.
