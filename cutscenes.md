---
layout: documentation
title: "Cutscenes"
section: "Authoring"
order: 4
permalink: /cutscenes/
---

Tag the controlled camera `MainCamera`. Place a marker at the desired shot position and choose a live transform as the look target. **Position** moves to a marker and waits for the shot to settle. **Camera look at** turns toward a named target and waits for the turn. Both track a moving target once acquired.

**Dolly** adds a world-space offset at a speed in units per second. **Zoom** moves the camera to a distance from a target over milliseconds. Both start motion and advance immediately. Follow with **Sleep**, dialogue, or a movement command if the shot needs time on screen. **Release camera** or **Reset** restores the prior pose, with an optional duration, and waits for completion. Cleanup also occurs when a runner is stopped.

Only one runner can own a camera at a time. Pause camera inputs or controllers that write the same camera transform while the shot runs. Resume them after release. NpcScript drives transforms and uses no render-volume integration.

Open **03 Cutscene Direction** for a camera positioned at a marker, a dolly concurrent with the scout's movement, distance zoom, dialogue and handoff. The **Look left/right** buttons move the player camera pivot after completion.
