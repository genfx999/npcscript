---
layout: documentation
title: Support
description: Find NpcScript setup help, troubleshooting, and answers about Unity compatibility, inputs, and C# commands.
section: Support
order: 10
permalink: /support/
---

Start with the [Getting Started guide]({{ '/getting-started/' | relative_url }}), [Troubleshooting]({{ '/troubleshooting/' | relative_url }}), or [API Reference]({{ '/api-reference/' | relative_url }}). For script errors, the [runtime debugging guide]({{ '/runtime-debugging/' | relative_url }}) explains how to find the active line and inspect the Console.

## Frequently asked questions

### Where do I open the Program Editor?

In Unity, choose **Tools → NpcScript → Program Editor**. Select an NpcScript program asset or a scene object with an NpcScript component.

### What Unity version has been verified?

NpcScript 1.0.0 was verified on Unity 6.3 (6000.3.27f2). The demos were run in the URP editor and in Windows x64 Built-in Mono and IL2CPP players. The core uses standard Unity modules.

### Why are my custom commands missing?

Select the scene object with your NpcScript subclass attached. Check that the method is public, has the `NpcScriptFunction` attribute, and uses a supported signature. See [Custom C# commands]({{ '/custom-commands/' | relative_url }}).

### Can I use my own dialogue UI?

Yes. Turn off the included dialogue panel, read the runner's prompt state, and submit a one-based response from your UI. See [Dialogue]({{ '/dialogue/' | relative_url }}).

## Contact

{% if site.support_email and site.support_email != '' %}
Email the publisher at [{{ site.support_email | escape }}](mailto:{{ site.support_email | escape }}).
{% else %}
**Publisher support email:** To be provided.
{% endif %}

{% if site.support_issues_url and site.support_issues_url != '' %}
Use the [public issue tracker]({{ site.support_issues_url | escape }}) for reproducible issues.
{% endif %}

When reporting an issue, include your Unity version, the exact Console message, and the steps to reproduce it.
