---
layout: documentation
title: "Function reference"
section: "Reference"
order: 6.5
permalink: /function-reference/
description: "Look up every built-in NpcScript function, its parameter types and defaults, what it returns, and when it waits."
---

Look up the built-in functions available to NpcScript programs. The names match the Program Editor. Parameters are listed in order with their types and any defaults. A waiting function holds the current row until its completion condition is met.

[Dialogue](#dialogue) · [NPC state and targeting](#npc-state-and-targeting) · [Timing and math](#timing-and-math) · [Navigation](#navigation) · [Animation](#animation) · [Audio](#audio) · [Props and transforms](#props-and-transforms) · [Cameras](#cameras) · [Diagnostics](#diagnostics)

Most functions are available in **Call** rows. Entries below identify functions used in conditions or expressions. For row syntax and project-specific commands, see the [Instruction reference]({{ '/instruction-reference/' | relative_url }}) and [Custom commands]({{ '/custom-commands/' | relative_url }}).

## Dialogue

### Say

**Parameters** — `message` (text).

Shows dialogue for the current NPC and starts its recorded voice when available. Continues immediately.

### Say for

**Parameters** — `speaker` (Transform), `message` (text).

Shows dialogue and starts available voice for the NPC on the speaker Transform or one of its parents. Continues immediately.

### Say random

**Parameters** — `messages` (one or more text choices).

Picks one nonblank choice at random and passes it to **Say**. At least one nonblank choice is required.

### Hide speech

**Parameters** — None.

Clears the current NPC's dialogue and stops its voice playback.

### Wait for speech

**Parameters** — `unvoicedMilliseconds` (integer, default `0`), `speaker` (Transform, defaults to the current NPC).

Waits for the speaker's recorded voice to finish. If the line has no recording, waits for the specified fallback duration instead.

### Response

**Parameters** — None.

Returns the most recent prompt choice as a one-based integer. Use it in a condition such as `Response == 1`.

## NPC state and targeting

### NPC name

**Parameters** — None.

Returns the current NPC's GameObject name as text.

### NPC is active

**Parameters** — None.

Returns whether the NpcScript component is enabled and its GameObject is active in the scene hierarchy.

### Select nearest target

**Parameters** — `maximumRange` (number in world units, default `100`).

Stores and returns the nearest other active GameObject with NpcScript within range. Clears the selection and returns `null` when no candidate is in range.

### Target Transform

**Parameters** — None.

Returns the stored selected target Transform, or `null` when no target is selected. Use **Target Transform** wherever the editor offers it as a variable, or `GetTargetTransform()` in a text expression.

### Select target by name

**Parameters** — `gameObjectName` (text).

Stores the Transform of the active GameObject found by name. Clears the selection if the name is blank or no match is found.

### Select target by tag

**Parameters** — `tag` (text, a defined Unity tag).

Stores the Transform of an active GameObject with the tag. Clears the selection if the tag is blank or no match is found.

### Select target in collider

**Parameters** — `colliderTransform` (Transform with Colliders on it or its children).

Returns the nearest object touching or overlapping a source Collider, excluding the current NPC's hierarchy. Returns the attached Rigidbody's Transform when present, or the Collider's Transform otherwise. A miss returns `null`. This query leaves the stored target unchanged.

### Distance to player

**Parameters** — None.

Returns the distance in world units from the object tagged `Player` to the nearest enabled solid NPC Collider. It falls back to trigger Colliders and then to the NPC's position. If no active player is found, returns `float.MaxValue`. Available in conditions and expressions.

### Player is focusing on NPC

**Parameters** — `maximumDistance` (number in world units, default `5`).

Returns true when the player is in range and the ray through the center of the Main Camera hits this NPC without a solid obstruction. Available in conditions and expressions.

### Find nearest

**Parameters** — `baseLocation` (Transform), `maxDistance` (nonnegative number in world units), `componentName` (text).

Returns the nearest Component of the specified type to the origin, including components on inactive GameObjects. Returns `null` when no match is in range. Use the fully qualified type name to resolve ambiguous names. Available in a Call row or as `FindNearest(...)` in a text expression.

### GameObject.Find

**Parameters** — `objectName` (text).

Finds an active GameObject by name or hierarchy path and returns it, or returns `null` if there is no match. This Unity helper is available in text expressions, including **Exec**. For example, `GameObject.Find("Lamp").SetActive(false)` deactivates an active GameObject named `Lamp`.

## Timing and math

### Sleep

**Parameters** — `milliseconds` (integer).

Waits for the specified real-time duration, independent of Unity's time scale. A duration of zero or less advances immediately.

### Sleep while player is focusing

**Parameters** — `milliseconds` (integer), `maximumDistance` (number in world units, default `5`).

Waits while the player is focusing on the NPC. Advances when the duration elapses or the player stops focusing.

### Random

**Parameters** — `min` (number, inclusive), `max` (number, exclusive).

Returns a random integer in the interval. The bounds must be finite and the interval must contain at least one supported integer. Use in a condition or expression.

### Min

**Parameters** — `v1` (number), `v2` (number).

Returns the smaller value. Use in a condition or expression.

### Max

**Parameters** — `v1` (number), `v2` (number).

Returns the larger value. Use in a condition or expression.

## Navigation

These functions use the current NPC's enabled NavMeshAgent on a NavMesh. The destination must be reachable.

### Move to

**Parameters** — `gameObjectName` (text).

Finds an active GameObject by name, moves the NPC toward it, and waits for arrival.

### Move to Transform

**Parameters** — `target` (Transform).

Moves the NPC toward the target Transform and waits for arrival.

### Wander

**Parameters** — `worldPosition` (world-space Vector3), `distance` (nonnegative radius in world units), `wait` (checkbox, default `false`).

Chooses one random reachable NavMesh destination within the horizontal radius. With **Wait** checked, the next row runs after arrival. With it unchecked, the next row runs while movement continues.

## Animation

These functions require an Animator on the NPC or one of its children.

### Set animator trigger

**Parameters** — `triggerName` (text).

Resets and then fires the named Trigger parameter in the Animator Controller.

### Set animator enabled

**Parameters** — `enabled` (boolean).

Enables or disables the Animator component.

### SetAnimationParameter

**Parameters** — `parameterName` (text, case-sensitive), `value` (text).

Sets the named Animator Controller parameter. Enter values such as `1.5` for a Float, `2` for an Int, or `true` or `false` for a Bool. For a Trigger, `true` fires it and `false` resets it.

### PlayAnimation

**Parameters** — `animationClip` (AnimationClip).

Plays the clip once and holds the row until the clip finishes.

### CrossFade

**Parameters** — `animationClip` (AnimationClip), `timeInMilliseconds` (nonnegative integer, default `500`).

Blends from the current animation into the clip over the specified duration. A duration of `0` switches immediately. The row waits for both the transition and the clip's playback interval.

### Play animation loop

**Parameters** — `animationClip` (AnimationClip), `milliseconds` (integer).

Repeats the clip and waits for the requested real-time duration, then stops the loop. A duration of zero or less advances immediately.

## Audio

### PlaySound

**Parameters** — `audioClip` (AudioClip).

Plays a one-shot sound through the AudioSource on the NPC's GameObject and continues immediately.

### Play sound at volume

**Parameters** — `audioClip` (AudioClip), `volume` (number clamped to `0`–`1`, default `1`), `wait` (checkbox, default `false`).

Plays the clip using the AudioSource on the NPC or a child, scaling its volume by the supplied value. With **Wait** checked, the row holds until this sound finishes. With it unchecked, the next row runs immediately.

### Play sound loop

**Parameters** — `clip` (AudioClip), `timeInMilliseconds` (nonnegative number), `Wait` (checkbox).

Plays the clip on a loop for the specified real-time duration. The checkbox controls whether the next row waits for the loop to stop. A duration of `0` stops any existing loop.

## Props and transforms

Tween functions start the motion and continue to the next row immediately. A duration of zero or less applies the change immediately.

### Set parent

**Parameters** — `parent` (Transform or `null`), `child` (Transform).

Reparents the child while preserving its world pose. Pass `null` to detach it. Parenting the current NPC suspends its NavMeshAgent, and detaching lets it rebind to a nearby NavMesh.

### Set visible

**Parameters** — `target` (Transform), `visible` (boolean).

Enables or disables MeshRenderers, SkinnedMeshRenderers, and Colliders on the target and its children.

### Tween position

**Parameters** — `tr` (Transform), `deltaX`, `deltaY`, `deltaZ` (numbers in world units), `transitionTime` (milliseconds).

Moves the Transform from its current world position by the supplied world-space offset.

### Tween position by name

**Parameters** — `gameObjectName` (text), `deltaX`, `deltaY`, `deltaZ` (numbers in world units), `transitionTime` (milliseconds).

Finds an active GameObject by name and moves it by the supplied world-space offset.

### Tween transform

**Parameters** — `target` (Transform), `timeInMilliseconds` (number).

Moves and rotates the current NPC to the target's world position and rotation as captured when the function starts.

### Tween to Y

**Parameters** — `tr` (Transform), `targetY` (number in local units), `timeInMilliseconds` (number).

Moves the Transform to the specified local Y position, keeping its local X and Z coordinates.

### Tween rotate X

**Parameters** — `tr` (Transform), `degrees` (number), `time` (milliseconds).

Rotates the local X angle to the target angle along the shortest angular path.

### Tween rotate Y

**Parameters** — `tr` (Transform), `degrees` (number), `time` (milliseconds).

Rotates the local Y angle to the target angle along the shortest angular path.

### Tween rotate Z

**Parameters** — `tr` (Transform), `degrees` (number), `time` (milliseconds).

Rotates the local Z angle to the target angle along the shortest angular path.

## Cameras

Scripted camera moves use the active Main Camera. See the [Cutscenes guide]({{ '/cutscenes/' | relative_url }}) for setup and examples.

### Camera look at

**Parameters** — `gameObjectName` (text), `milliseconds` (nonnegative integer).

Finds the named active GameObject and turns the camera to look at it. Waits for the initial turn, then keeps tracking the target until another camera command or release.

### Release camera

**Parameters** — `milliseconds` (nonnegative integer, default `500`).

Returns control to the original camera behavior and waits for the transition to finish. Advances immediately if this NPC has no scripted camera control.

### Zoom

**Parameters** — `target` (Transform), `range` (positive distance in world units), `milliseconds` (nonnegative integer, default `1000`).

Moves the camera toward or away from the target until it reaches the requested distance, keeping it aimed at the target. The script continues during the move.

### Dolly

**Parameters** — `x`, `y`, `z` (world-space offset numbers), `speed` (positive world units per second), `target` (Transform).

Moves the camera by the offset at the specified speed while looking at the target. The script continues during the move.

### Reset

**Parameters** — `milliseconds` (nonnegative integer, default `500`).

Returns camera control using the same transition and waiting behavior as **Release camera**.

### Position

**Parameters** — `position` (destination Transform), `target` (look-at Transform), `milliseconds` (nonnegative integer, default `0`).

Places the camera at the destination while aiming it at the target. Waits for the move to finish for a positive duration. Zero applies the pose immediately.

## Diagnostics

### Log

**Parameters** — `message` (text).

Writes an informational message to the Unity Console with the NPC name and any program call stack.

### LogWarning

**Parameters** — `message` (text).

Writes a warning to the Unity Console with the NPC name and any program call stack.
