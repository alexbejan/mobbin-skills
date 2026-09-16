---
name: design-bar
description: Build benchmark-quality product UI on any platform — native-feeling mobile screens (iOS / Expo / React Native) and websites or web apps (Astro, Next, React, plain HTML). Use when designing or implementing any screen, page, flow, onboarding, paywall, pricing page, landing page, dashboard, tab bar, sheet, settings, empty state — or when polishing hierarchy, motion, navigation, typography, dark mode, responsiveness or perceived performance. Enforces study-real-products-first (pairs with mobbin-research), one-accent / shape-lock anti-slop discipline, navigation semantics, a motion frequency gate, full state cycles, generated image assets, and a run-it-and-look verification loop (simulator for apps, browser at three widths for web). Trigger on "build a screen", "build a page", "make this better", "design the onboarding", "make the landing page", "polish the UI", "make it feel native", or any UI design/implementation task.
license: MIT
metadata:
  author: Alex Bejan (adapted from Appllama Skills, MIT)
  version: 1.0.0
---

# Design Bar

You are building UI that will sit next to the best-designed products in the
world, and the user will compare it to them within seconds. This skill
defines the bar and the method for clearing it, on mobile and on the web.

## The prime directive: study before you draw

Never design from imagination when you can study how the best products
solved the same screen. Shipping UI encodes thousands of hours of
iteration. Your first move on any screen or page is research:

1. If the **Mobbin MCP** is connected, pull real references for the platform
   and screen type you are building (the `mobbin-research` skill has the
   playbooks). Study 10–30 references before writing a line of UI code.
2. Extract the **pattern, not the pixels**: layout skeleton, information
   hierarchy, control choices, spacing rhythm, where the primary action
   sits, what gets an illustration vs. plain text, how progress is shown.
3. Then design **your** screen: the proven skeleton, your product's voice.
   Copying one product 1:1 is lazy and legally risky; ignoring every
   convention users already know is worse.

## Pick the platform reference

| Target | Load |
|---|---|
| iOS / Android app (Expo, React Native) | [references/mobile.md](references/mobile.md) plus motion, native-controls, performance |
| Native SwiftUI / UIKit app | [references/mobile.md](references/mobile.md) for the laws; implementation details come from the Axiom skills (`axiom-swiftui`, `axiom-design`) if present |
| Website, landing page, web app, dashboard | [references/web.md](references/web.md) |
| Image / illustration assets, any platform | [references/image-assets.md](references/image-assets.md) |
| Final verification, any platform | [references/verification.md](references/verification.md) |

The laws below apply everywhere. The platform reference says how to obey
them with that platform's tools.

## Hierarchy laws

1. **One job per screen or section.** Name it in one sentence before you
   build. Everything on the surface either serves that job or leaves.
2. **One display size per screen.** The type ramp is the hierarchy: one
   headline, then titles, body, captions, from the platform ramp (iOS
   Large Title → Footnote; web: a 5–6 step modular scale). Tabular numerals
   for anything that counts, times or prices.
3. **The primary action is obvious and singular.** One primary button per
   screen or section, where the eye lands last; secondary actions are
   quieter, never a second primary.
4. **Spacing rhythm.** Pick a base unit (4 or 8) and never leave it. Use
   `gap` over margin stacking. A rogue 13px gap in an 8pt system is a bug.
5. **Format numbers like a product, not a database**: 1.4M, 38k, $4.99,
   trailing zeros trimmed, dates localized.

## Anti-slop laws

AI-built UI shares a look, and users file it under "template" within
seconds. Each of these is a default ban; the only override is a brand that
explicitly asks for it AND a reason you can articulate.

1. **No AI-default styling.** Purple/indigo gradient CTAs with a glow,
   glassmorphism on every card, mesh-gradient heroes, confetti for minor
   events, sparkles in headings, three-card feature grids with a centered
   icon in a tinted circle. Your palette, materials and layout come from the
   references you studied, never from the priors you'd reach for unprompted.
2. **One accent, locked.** One accent color, THE accent on every screen.
   Neutrals carry the product; the accent is spent where the money is
   (primary action, active state, progress).
3. **One grey family.** Warm or cool, never both.
4. **Shape lock.** One corner-radius scale stated as a rule ("actions are
   pills, cards 16, inputs 8") and never violated.
5. **No emoji as iconography.** Icons are SF Symbols / Material Symbols /
   one icon set (Lucide, Phosphor…). Emoji appear only in content, when the
   product's voice is genuinely chat-native, never in chrome.
6. **One label per intent.** "Get started", "Start now" and "Begin" are the
   same intent: pick one phrasing and use it everywhere.
7. **Emphasis stays in the family.** Weight or italic of the same typeface;
   never a serif word dropped into a sans headline for interest.
8. **Ship full state cycles, not the happy path.** Skeletons match the
   final layout's shape, empty states are composed and say how to fill
   them, errors are inline and specific, loading never blocks the whole
   surface for a partial update.
9. **The slop pre-flight is mechanical.** Before any surface reaches
   verification, count: distinct accent hues (1), distinct corner radii
   (all from the stated scale), emoji in chrome (0), gradients without a
   brand reason (0), duplicate labels for one intent (0). A failed count is
   a fix, not a judgment call.

## Navigation laws

Navigation is the part a screenshot can't show, and users feel it in ten
seconds. Every transition answers three questions: what is the destination
to here, must the user be able to come back, and what does back do
afterwards (iOS chevron and edge swipe, Android hardware back, browser
back button).

1. **Push goes deeper, replace moves on.** Push when the user will want to
   return here; replace when coming back would land in a state the world
   has moved past. Back undoes *navigation*, never *events*.
2. **Presentation is meaning.** A self-contained multi-step task is a modal
   with its own stack and its own Cancel/Done (web: a route or a full
   dialog with steps). A short interruption (picker, filters, item options)
   is a sheet or popover with drag or click-outside to dismiss. Immersive
   content is full-screen with an explicit Close. Something floating over a
   still-visible screen is an overlay. Destructive confirms use the
   platform's confirm pattern. If a link could open it, it is a route, not
   local state.
3. **One-way doors leave the stack.** Sign-in, finished onboarding (Skip
   included), a purchase, a completed session: land with replace so back
   can never re-enter the old state. But keep the user's *place*: sign-in
   demanded by one action is a modal over the screen that completes the
   action where it was tapped; a paywall opened from a feature dismisses
   back onto the feature, unlocked.
4. **Back is blocked in exactly two cases**: an irreversible request in
   flight (seconds, with visible progress) and unsaved work in a modal
   (ask first). Anything else that traps back is a defect.
5. **Tabs are peers** (mobile) and **top nav items are peers** (web): no
   slide between them, each keeps its own stack or scroll position,
   re-selecting the active one returns to its root. Deep links and cold
   starts land with a real stack underneath, never a login flash before
   home.
6. **Study the grammar, not just the pixels.** When walking a winning flow
   on Mobbin, note what each step *is* (push, modal, sheet, page, dialog)
   and copy that consistency.

## Motion laws

Motion is the highest-leverage polish surface and the easiest to overdo.
Decide in this order:

- **The frequency gate comes first.** Met 100+ times a day (tab switch,
  keyboard, scroll, back, hover) → the platform default and nothing else;
  tens a day (press, row select) → near-imperceptible, under 150 ms;
  occasional (sheets, modals, toasts, section reveals) → standard motion;
  delight only on rare, first-time moments. Passing this gate with zero
  lines of code is a success; when unsure, delete the animation.
- **Name the purpose in one word** (feedback, spatial continuity, state
  change, preventing a jarring cut, explanation, delight) or don't build
  it. Data the user is reading never moves for style.
- **If a finger or a pointer drag was involved, it's a spring** seeded with
  the release velocity; everything else is timing under 300 ms with a
  strong ease-out (`cubic-bezier(0.23, 1, 0.32, 1)`), never ease-in on an
  entrance. Exits are faster than entrances and leave the way they came in.
  Enter from `scale(0.95)` + fade, never `scale(0)`.
- **Never on the main thread.** Mobile: worklets, `transform`/`opacity`
  only. Web: `transform`/`opacity` only, CSS transitions or the Web
  Animations API, no layout-triggering properties, no scroll-linked JS
  where CSS `animation-timeline` or IntersectionObserver does.
- **Respect Reduce Motion** on both platforms: spatial motion collapses to
  cross-fades.
- The bar: 60 fps through the hero flow, measured on the slowest device or
  a throttled browser, not vibed.

## State architecture

UI that feels great has boring state:

- **Server state** in a query cache (TanStack Query or the project's
  equivalent) with retries and optimistic updates. Never `useEffect` +
  `fetch`.
- **Client state** in a small atomic store. Broad app-state contexts cause
  the re-render cascades that make UI feel heavy.
- **Ephemeral UI state** (open/closed, focus, scroll) stays local.
- **Optimistic by default**: taps reflect instantly, reconcile in the
  background, roll back loudly on failure.
- Uncontrolled inputs for high-frequency typing surfaces.

## Image and illustration assets

When a surface calls for illustration, empty-state art, hero imagery or
icons beyond the symbol set: generate with the best image model available
(Higgsfield CLI / MCP when connected) at the highest quality, one style
system for the whole product, transparent or exact-surface-color
backgrounds, then downscale. Never upscale. Pipeline and prompt patterns:
[references/image-assets.md](references/image-assets.md).

## The verification loop (non-negotiable)

A surface does not exist until you have seen it running.

1. Implement → run it (iOS Simulator / Android emulator for apps; a real
   browser at 360, 768 and 1280 px for web, via agent-browser, Chrome
   DevTools MCP or a screenshot tool).
2. Screenshot and **actually look**: alignment, optical centering, spacing
   rhythm, truncation with long content, dark mode, large text sizes.
3. Run the **full-motion pass**: record the whole flow, watch it at speed
   for feel, then scrub frame by frame for pops, flashes, jumps.
4. Fix, relaunch, re-verify. Repeat until you cannot find a defect, then
   run [references/verification.md](references/verification.md) once more.

Do not declare a surface finished from code review alone. Stop at "cannot
find a flaw at 100% zoom", not at "looks fine".

## Definition of done, per screen or page

- [ ] Studied 10+ real references for this surface type (Mobbin when
      available) and can name the pattern adopted
- [ ] One job named; one primary action; type ramp holds hierarchy
- [ ] Navigation answered: what this surface *is*, what back does, and
      behind a one-way door that back cannot re-enter the old state
- [ ] Slop pre-flight counts pass (accent 1, radii from scale, emoji 0,
      unreasoned gradients 0, duplicate labels 0)
- [ ] Light + dark verified running
- [ ] Mobile: safe areas / Dynamic Island / home indicator verified.
      Web: 360, 768, 1280 px verified, no horizontal scroll, 16px gutters
- [ ] Long-content, empty, loading and error states designed, not defaulted
- [ ] Motion recorded and scrubbed: native feel, zero glitch frames,
      Reduce Motion respected, 60 fps measured
- [ ] Large text (Dynamic Type XL / browser zoom 200%) doesn't break layout
- [ ] Tap targets ≥ 44pt; contrast passes both themes; keyboard-navigable
      on web with visible focus
- [ ] Assets: single style family, crisp at the largest rendered size, no
      compositing halos, under budget
- [ ] Lists virtualized; no controlled-input jank; no re-render storms
      (profiled, not guessed)
