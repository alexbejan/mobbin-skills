# Playbook: make an existing screen, page or section better

The user has a screen, a page or a section (code, screenshot, live URL, or a
running app) and wants it better. "Better" means measurably closer to the
best equivalents that ship today, in hierarchy, motion and feel, verified
by running it, not by eyeballing code.

## 1. Diagnose before you search

Run it (simulator or browser) and study it against design-bar's definition
of done. Name the top 3 deficits precisely: "no visual hierarchy, three
same-weight text rows", "dead motion, modal pops with no transition",
"non-native segmented control", "hero has two competing CTAs", "pricing
table unreadable at 390px". The research pass is aimed at these deficits,
not at inspiration.

## 2. The 20 + 20 reference board

Two searches, two pages each, same `task_intent`, right `platform`. Keep
paging if results stay strong.

- **Literal:** describe the screen by its content and elements.
  Apps: `search_screens(query="workout summary with total load, sets table
  and share button", platform="ios", mode="standard", limit=10)`.
  Sites: `search_sections(query="pricing section with monthly/annual
  toggle and three plan cards", limit=10)`.
- **Descriptive:** describe what it should feel like and do.
  `search_screens(query="calm dark stats dashboard with one hero number and
  weekly bars", mode="deep")`. Different results surface; that is the
  point of running both.
- If the screen belongs to a journey, one `search_flows` for the
  surrounding flow so you see what comes before and after.

Save the results into the local board (see SKILL.md), download the ones
that matter. Pick the **best 5–8** and say why each earns its place. If one
is nearly perfect, search that app by name for its neighbours: a sibling
screen from the same app often carries the same system and answers more of
your questions.

## 3. Extract the pattern

From the picks, write the target: layout skeleton, hierarchy order,
control choices, spacing rhythm, palette role-mapping, copy budget, motion
moments (entrance, press feedback, data reveal), and for web the
breakpoints where the layout changes. This is a spec, not a mood board:
every line should be checkable in a screenshot.

## 4. Rebuild and iterate (the loop)

1. Implement against the spec using **design-bar** (type ramp, one
   accent, both themes, native controls or semantic HTML, motion with the
   platform's curves, state kept boring).
2. Asset gaps (illustration, empty-state art, icons beyond the symbol set):
   generate with the best available image model at max quality, one style
   system, per design-bar's image-assets reference.
3. **Verification loop until perfect:** screenshot, compare side by side
   with your top references, fix, repeat. Record the motion and scrub it.
   Apps: Dynamic Island and safe areas, dark and light, Dynamic Type XL,
   Reduce Motion, 60 fps. Web: 360, 768, 1280 widths, dark and light,
   keyboard navigation, prefers-reduced-motion, Lighthouse.
4. Do not stop at "better than before". Stop when the honest side by side
   with the best reference reads **at least as good**. If it doesn't, name
   the gap and go around again.

## 5. If the screen is part of a flow

Screens live in journeys. After the screen passes, walk one step before
and one step after it: entrance transition, exit transition, state carried
across, what back does (iOS chevron, edge swipe, Android hardware back,
browser back). If it is a sheet, a modal or a step behind a one-way door,
verify its presentation matches what it *is*, not just how it looks. Use
`search_flows` on the winners if you need to see how they chain the
surrounding steps (references/research-methods.md).
