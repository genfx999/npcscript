---
layout: documentation
title: "Troubleshooting"
section: "Support"
order: 10
permalink: /troubleshooting/
---

| Symptom | What to check |
| --- | --- |
| I cannot find the editor. | Open **Tools → NpcScript → Program Editor** after Unity finishes compiling. Resolve Console errors first. |
| Custom commands are missing. | Select the scene object with your subclass attached. Use a public instance method with the attribute, a unique name and supported parameter types. Refresh the Program Editor if necessary. |
| The prompt stays paused. | With the built-in panel, click a response or press its number key. With a custom panel, submit a one-based choice to the active runner. Check whether another runner already owns the prompt channel. |
| Proximity does not start. | Check the `Player` tag, the `MainCamera` tag, the range and the NPC's collider. Test a manual `Run()` to isolate the scene setup. |
| A movement call stops with an error. | Confirm the runner's NavMeshAgent is enabled and on a NavMesh that matches its agent type. Keep the destination near a reachable position or add an `ApproachPoint` child. |
| A prop cannot be found. | Check spelling and duplicate scene object names. A normal name lookup requires an active object. To reveal something later, leave the object active and disable its renderer, as shown in Actions and Sequencing. |
| The cinematic camera jitters. | Suspend other camera writers while the shot owns the transform. Release the camera before handing input back. |
| It works in the Editor but a command is missing in a build. | Preserve reflected custom commands and component members, then rebuild and test the target stripping level. |
| A localized string or voice is missing. | Check exact source text and the resource folder. Use the translation and recording helper APIs to avoid guessing filenames. |
| I cannot replay a sample from the beginning. | Restart Play Mode to restore its authored scene state. **Stop** cancels the runner and leaves completed scene actions in place. |
