# NpcScript product website verification

The product website is live at [genfx999.github.io/npcscript/](https://genfx999.github.io/npcscript/). GitHub Pages built the product update from `main` at `5029029` using the repository's existing Jekyll publishing source. The site uses static HTML, Markdown, CSS and local Unity captures. The public repository contains the website and its guides.

## Verified against the deployed site

| Check | Result |
| --- | --- |
| Homepage and documentation | The product homepage, Support and all 13 existing guide URLs returned HTTP 200. All 13 guide bodies match the previous source. |
| Navigation and assets | All same-site links and anchors resolved under `/npcscript/`. The stylesheet and five genuine Unity images loaded. |
| Responsive layout | Edge rendered the homepage and Instruction Reference at 1440, 768 and 360 CSS pixels with no page-level horizontal overflow. Tables and code blocks keep their own scrolling areas. |
| Keyboard and images | Keyboard testing opened the mobile menus, moved focus to a navigation item and opened the API Reference. Images have descriptive alt text and decoded successfully. The checked text, link and button colors had contrast ratios above 7:1. |
| Runtime | No page script exceptions or failed same-site network requests during the browser checks. Navigation uses native HTML disclosure controls and the site needs no site JavaScript. |
| SEO | Canonical URLs, descriptions, Open Graph metadata, `robots.txt` and the 15-entry sitemap were checked over HTTPS. |
| Publication scope | The Unity source repository was independently verified as private. No Unity code or scene assets were changed for the site. |

The browser checks used Edge with emulated viewport sizes. Separate physical-device and screen-reader reviews are outside this verification.

## Publisher settings

The Asset Store state is **Coming Soon**. Add the real listing, video and support details to `_config.yml` when available. Add a `video_url` to an example in `_data/examples.yml` if a genuine clip is published.

The hero and showcase use a genuine Unity Program Editor capture. Four gallery cards have genuine Game view captures. Custom C# Integration and Beacon Demo show a neutral pending state. To complete the gallery, capture the gate raised in the Custom C# Integration sample and a visible Beacon prompt or raised beacon in Beacon Demo. Keep the capture scoped to Unity's Game view.
