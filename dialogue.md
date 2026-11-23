---
layout: documentation
title: "Dialogue"
section: "Authoring"
order: 3
permalink: /dialogue/
---

**Say** displays a line for the runner. **Say for** targets another object with a runner. **Say random** chooses a non-empty option. **Hide speech** dismisses the line and stops its recorded speech. **Wait for speech** waits for the recording, or for the fallback duration in milliseconds.

A **Prompt** reserves the shared prompt channel, displays its text and up to nine non-empty responses, and waits. Call **Response** in an If condition. The values are one-based and follow the row order. A proximity prompt canceled by departure yields zero. The default panel accepts clicks and digit keys and restores the prior cursor state on exit. Only one prompt owns the channel at a time.

For custom UI, disable **Show Dialogue UI**. Read `DialogueText`, `PromptText` and `PromptChoices` from the runner. Validate submissions with `SubmitResponse(oneBasedChoice)`; it returns false for an invalid choice, a duplicate click or a runner without the active prompt. Your UI owns input and cursor state when the supplied panel is off.

Optional **Prompt On Proximity** starts when the `Player` is in range and the `MainCamera` looks at an NPC collider. Manually started prompts have no scene camera or tag requirement. Programs that use **Say** can add explicit **Hide speech** steps after the message has been read.
