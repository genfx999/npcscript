---
layout: documentation
title: "Custom C# commands"
section: "Reference"
order: 7
permalink: /custom-commands/
description: "Create a component subclass of NpcScript, attach it in place of the base runner, and select it in the Program Editor. Public instance methods marked..."
---

Create a component subclass of `NpcScript`, attach it in place of the base runner, and select it in the Program Editor. Public instance methods marked `NpcScriptFunction` appear when their signatures are supported. Use unique method names. The display label is independent of the serialized method name, so rename labels freely and migrate saved calls before renaming methods.

```csharp
using NpcScripting;
using UnityEngine;
using UnityEngine.Scripting;

public sealed class CheckpointNpc : NpcScript
{
    [SerializeField] private Transform door;
    private bool permitted;

    [Preserve, NpcScriptFunction("Chime", allowAsCondition: false)]
    public void Chime() => Debug.Log("Checkpoint accepted.", this);

    [Preserve, NpcScriptFunction("Give permission", allowAsCondition: false)]
    public void GivePermission(bool allowed) => permitted = allowed;

    [Preserve, NpcScriptFunction("Has permission", allowAsCommand: false)]
    public bool HasPermission() => permitted;

    [Preserve, NpcScriptFunction("Raise door", waitUntilTrue: true, allowAsCondition: false)]
    public bool RaiseDoor(float height)
    {
        if (door == null) throw new System.InvalidOperationException("Assign a door in the Inspector.");
        Vector3 p = door.localPosition;
        p.y = Mathf.MoveTowards(p.y, height, 2f * Time.deltaTime);
        door.localPosition = p;
        return p.y == height;
    }
}
```

Call **Give permission** with `true`, test **Has permission** with **is true**, then call **Raise door** and **Chime**. Sample **04 Custom CSharp Integration** runs the equivalent workflow with `GateDemoNpc`.

A waiting command returns `false` while work continues and `true` to advance. The interpreter polls it again on later ticks. For a host coroutine, task or external callback, own its lifecycle in the host component and return the result from your polling method. A call can throw a descriptive exception. NpcScript reports it with the instruction line and invocation stack. For background work, keep Unity API calls on the main thread and cancel work explicitly when your integration stops.

Use Unity's `Preserve` attribute or your own `link.xml` for reflected custom commands and component members in stripped builds. The product includes a linker descriptor for the base runner. Test your own commands in the target player configuration.
