---
layout: documentation
title: "Getting started"
section: "Start here"
order: 1
permalink: /getting-started/
description: "5. In Play Mode, click Speak to guard, or use Run in the Program Editor. The positive branch waits for the gate to reach its target height. The..."
---

1. Import the Asset Store download from **Window → Package Manager → My Assets**. For a local export, choose **Assets → Import Package → Custom Package** and select the `.unitypackage`.
2. Open **Tools → NpcScript → Getting Started**, then open **01 Interactive Dialogue**. Enter Play Mode, click **Speak to guard**, and try either answer. Restart Play Mode to replay from the original gate position.
3. Exit Play Mode. Select **Harbor Guard** in the Hierarchy, then open **Tools → NpcScript → Program Editor**. The selection loads the assigned **Harbor Conversation** program and the custom commands from `GateDemoNpc`.
4. To make a new program for the same scene, use **New** in the Program Editor. Save the asset and assign it to **Harbor Guard → Program** in the Inspector. Keep the created `Exit` at the bottom. Insert these rows above it.

| Row | Setup |
| --- | --- |
| Prompt | “Do you have a permit?” with choices “Yes” and “No” in that order. |
| If | Choose **Response**, **==**, and enter `1`. |
| Call | Choose **Open gate**, set the height to `3.35`. |
| Else | Use the Else row inside the If block. |
| Call | Choose **Say** and enter “Visit the harbor office for a permit.” |
| End If | Use the closing row created by **If block**. |
| Exit | Finish the program. |

5. In Play Mode, click **Speak to guard**, or use **Run** in the Program Editor. The positive branch waits for the gate to reach its target height. The negative branch leaves it down. Watch the active instruction as you take each branch. The sample scene button works with the assigned program and no movement controller.

For a blank scene, add `NpcScript` to any GameObject, assign a program, and leave **Run On Start** enabled. A basic Prompt and Say need no camera or Player tag. Use `Run()` from your own interaction trigger when starting on demand.
