---
layout: documentation
title: "Localization and recordings"
section: "Authoring"
order: 5
permalink: /localization/
---

The authored strings are the English source. Set `NpcScripting.Localization.Localization.CurrentLanguageCode` from your settings UI, or use the **Localization** component's **Language Code**. Runtime text falls back to the authored string when a translation is missing or blank.

Build a translation template in your own editor utility with `JsonUtility.ToJson(NpcScriptLocalizationFileBuilder.Build(program, "fr"), true)`. Save it to `Assets/YourGame/Resources/localizations/fr/<program asset name>.json`. Preserve letter case in the path, exact source strings and interpolation tokens such as `{Local.Count}`.

```json
{
  "sourceLanguage": "en",
  "targetLanguage": "fr",
  "speakerName": { "english": "Harbor Guard", "translation": "Garde du port" },
  "entries": [
    { "text": "Do you have permission to enter the harbor?", "translation": "Avez-vous l'autorisation d'entrer dans le port ?" },
    { "text": "Yes, I have a permit.", "translation": "Oui, j'ai un permis." }
  ]
}
```

The Interactive Dialogue sample includes a complete French file. To try it, add a **Localization** component to a scene GameObject and set **Language Code** to `fr`, then run the dialogue. Use distinct program asset names to keep resource paths unique.

For English recordings, write a voice identifier in a **Context** row or in the runner's **Dialogue Voice** field. Use `Localizer.BuildVoiceLines(program)` to generate the names and `Localizer.VoiceFolderName(voice)` for the folder. Put AudioClips in `Resources/Voices/en/<voice folder>/`. `SayFor` can resolve a named speaker's voice. Voice lookup stays English when text is translated. The asset plays recordings you provide. It does not synthesize speech, download assets or supply a translation service.
