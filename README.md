# mobbin-skills

Two agent skills that turn the [Mobbin MCP](https://mobbin.com) into a
builder's method, for mobile apps and websites alike:

| Skill | What it does |
|---|---|
| [`mobbin-research`](skills/mobbin-research/SKILL.md) | The research engine: the Mobbin tool map (`search_screens`, `search_flows`, `search_sections`), the ios/web platform switch, query craft, and the playbooks for building from scratch, improving an existing screen or section, and flow/section research. |
| [`design-bar`](skills/design-bar/SKILL.md) | The build bar: platform-neutral hierarchy, anti-slop, navigation, motion and state laws, plus platform references for mobile (Expo/React Native, SwiftUI pointer) and web (Astro/React/HTML), an image-asset pipeline (Higgsfield), and run-it-and-look verification checklists for simulator and browser. |

They are a pair: **research** decides what to study, **design-bar** decides
how to build, and both end only when the surface has been run and cannot
be faulted.

The method and the mobile references are adapted from
[Appllama Skills](https://github.com/Appllama/appllama-skills) (MIT), rewritten
for Mobbin's three tools and extended to the web. See LICENSE.

## Install

User-wide for Claude Code (what this machine uses):

```bash
git clone https://github.com/alexbejan/mobbin-skills ~/Documents/mobbin-skills
ln -s ~/Documents/mobbin-skills/skills/mobbin-research ~/.claude/skills/mobbin-research
ln -s ~/Documents/mobbin-skills/skills/design-bar ~/.claude/skills/design-bar
```

Or with the skills CLI, once the repo is on GitHub:

```bash
npx skills@latest add alexbejan/mobbin-skills -g
```

## Connect the Mobbin MCP

Add `https://api.mobbin.com/mcp` as an HTTP MCP server (Claude Code:
`claude mcp add --transport http mobbin https://api.mobbin.com/mcp`) and
sign in with your Mobbin account. `mobbin-research` needs it;
`design-bar` works without it, sharper with it.

## Try it

> Study how the best fitness coaching apps do onboarding on iOS, then spec ours.

> Make the Vorti landing page. Research hero and pricing sections on Mobbin first.

> Make this screen better. *(paste a screenshot or a mobbin.com link)*

## Layout

```
skills/
  mobbin-research/
    SKILL.md
    references/build-from-scratch.md
    references/improve-a-screen.md
    references/research-methods.md
  design-bar/
    SKILL.md
    references/mobile.md          # Expo/RN laws + SwiftUI mapping
    references/web.md             # sites, landing pages, web apps
    references/verification.md    # simulator + browser checklists
    references/motion.md          # RN Reanimated patterns (Appllama)
    references/native-controls.md # RN native controls (Appllama)
    references/performance.md     # RN performance (Appllama)
    references/image-assets.md    # asset generation pipeline
```
