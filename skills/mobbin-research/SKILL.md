---
name: mobbin-research
description: Use the Mobbin MCP well — research real shipped iOS apps and websites (screens, flows, website sections), then build from what you learn. Load when the Mobbin MCP is connected (mcp__mobbin__search_screens / search_flows / search_sections) and the task involves designing or building any mobile screen, app flow, website page or section, improving an existing screen, or studying how the best products structure onboarding, paywalls, pricing, checkout, dashboards, landing pages. Covers the tool map, the platform switch (ios vs web), query craft, expiring media, and the research-then-build playbooks. Pairs with the design-bar skill for implementation.
license: MIT
metadata:
  author: Alex Bejan (method adapted from Appllama Skills, MIT)
  version: 1.0.0
---

# Mobbin Research Skill

Mobbin is a library of real, shipped product UI: iOS app screens and flows,
and website pages broken into sections. The MCP puts that library in your
hands. **It is a builder's tool, not a mood board:** you study what already
ships and works, extract the pattern, then build something that stands next
to it.

Pair this skill with **design-bar** for every design or implementation
step. This skill decides *what to study*; design-bar decides *how to build*
and how to verify it.

## Ground rules (read first)

1. **Pick the platform first.** `platform: "ios"` for anything that runs on
   a phone (native, Expo, Flutter, PWA-as-app). `platform: "web"` for
   websites, web apps, dashboards. Building a responsive web app? Search
   both: `web` for layout, `ios` for the mobile behaviors it must match.
2. **Same `task_intent` on every call of one task.** One short English
   sentence ("Design the onboarding flow for a fitness coaching app"). It
   steers ranking and must not change mid-task. Never put user messages,
   file contents or personal data in it.
3. **Read the images, not the metadata.** Every result carries inline
   preview images. Look at them and describe what you actually see. Never
   describe a screen from its title or app name alone.
4. **One thing per query.** One screen type, one flow, one section per
   call. Split combined asks into separate searches. No negations ("without
   ads"), no style adjectives ("modern", "clean"), no keyword soups.
   Concrete UI nouns work: "checkout with promo code field and Apple Pay
   button".
5. **Name an app to filter to it.** "Duolingo onboarding", "Linear pricing
   page". This is Mobbin's substitute for app profiles and per-app screen
   walks: to study one app deeply, run its flows and screens by name.
6. **Links are durable, media is not.** `mobbin_url` is the permanent
   citation and the link to show the user. `image_url` is the high-res file
   and expires in about 30 days: download it into the local research board
   when you need the pixels later. Inline previews are for you to read, not
   for the user.
7. **Cite every screen you mention** as a markdown link to its
   `mobbin_url`. When the user wants to save, export, embed or paste a
   result (Figma, Notion, docs, slides), download from `image_url`.
8. **Mind the context.** Default limits are generous; use `limit: 5–10` for
   flows and `10–20` for screens, and page (`page: 2, 3…`) instead of
   asking for 30 at once. Use `mode: "deep"` for nuanced or semantic asks,
   `mode: "standard"` for quick, literal lookups.
9. **Research, don't harvest.** Every search answers a named question from
   the current task. Sweeping the catalog to build a dataset is not
   research and is against Mobbin's terms.

## Tool map

| Tool | Platforms | What it gives you | Typical use |
|---|---|---|---|
| `search_screens` | ios, web | Single screens matching a description, with previews, `mobbin_url`, `image_url`, app metadata. `mode` deep/standard, `limit` ≤ 30, `exclude_screen_ids` to page past what you've seen | Gather design references for one screen type or page |
| `search_flows` | ios, web | Multi-step user journeys with evenly spaced previews and per-screen previews. `limit` ≤ 10, `page` ≤ 20 | Study how winners structure onboarding, checkout, paywall, sign-up, settings |
| `search_sections` | web | Website sections (hero, pricing, features, testimonials, footer, FAQ, CTA…) with previews. `limit` ≤ 30, `page` | Build a landing page or marketing site section by section |

**What Mobbin does not have, and what to do instead**

| Appllama-style capability | Mobbin substitute |
|---|---|
| Revenue / downloads / rating filters | Shortlist by reputation: name 3–5 category leaders you already know are strong, then search them by name |
| Walk one app's screens in journey order | `search_flows(query="<App> <flow>")` gives the ordered journey; `search_screens(query="<App> <screen>")` fills gaps |
| UI element taxonomy | Describe the element in the query ("bottom tab bar with 5 items and center action button") |
| Boards / saved curation | The local research board below |
| Screen-ref pasting | The user pastes a `mobbin_url`; re-find it by app name + screen description |

## The playbooks

| Scenario | Reference |
|---|---|
| Build an app or a site from scratch ("build me a habit tracker", "make the Vorti landing page") | [references/build-from-scratch.md](references/build-from-scratch.md) |
| Make an existing screen, page or section better | [references/improve-a-screen.md](references/improve-a-screen.md) |
| Flow research, section research, general study discipline | [references/research-methods.md](references/research-methods.md) |

Both build playbooks end the same way: **the verification loop from
design-bar, repeated until you cannot find a flaw.** Research without that
loop is decoration.

## Local research board

Links expire; notes and downloads don't. Keep research next to the project,
in a folder git ignores:

```
research/
  <topic>/                 # e.g. onboarding, landing-page, paywall
    apps.md                # shortlist: app/site, platform, why it matters, verdict
    screens.md             # per reference: mobbin_url, app, what it does well, what to take
    patterns.md            # cross-app synthesis: the category's design language, the spec
    img/                   # downloaded image_url files, named <app>-<screen>.webp
```

Download the screens you pick as you go (`curl -sL "<image_url>" -o
research/<topic>/img/<app>-<screen>.webp`). Synthesis happens with the
images side by side, not from titles.

## When the Mobbin MCP is not connected

Say so, then fall back to what is available (Higgsfield `website` /
Appllama if connected, the user's own screenshots, or your knowledge of the
category), and mark every claim that would have needed real references as
unverified. Do not invent "Mobbin says".
