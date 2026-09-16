<!-- Simulator section adapted from Appllama Skills (MIT, appllama.io). -->

# Verification: the run-it-and-look checklists

A surface is finished when it survives this checklist running, not when
the code compiles. Budget as many iterations as it takes; the goal is
"cannot find a flaw", not "looks fine". Never batch more than a few fixes
between looks; regressions hide in batches.

Mechanics, both platforms:

1. Run it. Screenshot. **Open the screenshot and study it**, do not trust
   memory of what you wrote.
2. Interact: every control, overlong text, background/foreground or
   resize, both themes.
3. **Record the motion of the entire flow** and watch it twice: once at
   speed for feel, once frame by frame. Stills cannot catch a one-frame
   flash, a dropped spring, a keyboard jump-cut or a reveal that fires
   twice.
4. Fix → relaunch → re-verify.

If a UI test tool is available (Maestro, XCUITest, Playwright), script the
happy path once it stabilizes so later changes re-verify for free.

---

## A. Mobile (iOS Simulator / Android emulator)

Tools: `xcrun simctl io booted screenshot s.png`,
`xcrun simctl io booted recordVideo m.mov`, or the xcodebuildmcp /
Axiom simulator tools when present.

**Layout**
- [ ] Nothing clipped by the Dynamic Island / status bar; scrolled content
      passes *under* it with the intended fade/blur, not a hard edge
- [ ] Bottom CTA clears the home indicator
- [ ] Optical alignment: icons vs text baselines, centered things look
      centered (check at 2× zoom)
- [ ] Spacing rhythm consistent (no rogue gaps in the base-unit system)
- [ ] Long text: 2× length titles truncate or wrap by design
- [ ] Empty, loading and error states each verified by forcing them

**Theming and type**
- [ ] Dark AND light screenshots taken and inspected
- [ ] Dynamic Type at XL: no overlap, no clipped labels
- [ ] Contrast: secondary text readable in both themes

**Motion (on the full-flow recording, never on stills)**
- [ ] Entrance plays once, on first mount, and NOT again on back-navigation
- [ ] Gesture follows the finger 1:1; release springs with velocity;
      cancelling mid-gesture settles cleanly
- [ ] Frame by frame: no pop at start/end, no double-render flash, no
      one-frame white / wrong-theme / wrong-color frames during transitions
- [ ] Every modal and sheet cycle: present, drag, dismiss, cancel, smooth
      both ways
- [ ] Keyboard appear AND dismiss: layout glides, focused input stays
      visible, nothing jump-cuts after settling
- [ ] Sustained 60 fps through every transition (perf monitor / Instruments
      on a release build, slowest supported device)
- [ ] Reduce Motion on → spatial animations become fades

**Navigation**
- [ ] Every back path tried: chevron, edge swipe, Android hardware back,
      active-tab re-tap
- [ ] After each one-way door (sign-in, onboarding done, purchase, finished
      session) back does NOT re-enter the old state
- [ ] Rapid double-taps don't double-navigate or double-submit

**Interaction and state**
- [ ] Every tappable ≥ 44pt; press states visible; haptics where native
      controls would have them
- [ ] Keyboard type right, doesn't cover the input, dismisses sensibly
- [ ] Background mid-flow → return: state intact
- [ ] Kill and relaunch: persisted state restores, ephemeral resets
- [ ] Offline: actions queue or fail loudly, never silently

**Device matrix (minimum)**

| Profile | Why |
|---|---|
| Latest iPhone Pro (Dynamic Island) | Primary design target |
| iPhone SE-class (small, no island) | Layout compression, reachability |
| Latest Pixel (Android) | Material behaviors, back gesture, font metrics |
| iPad / tablet IF the app claims support | Otherwise explicitly letterbox |

Full checklist on the primary; layout, safe areas and the hero flow on the
others.

---

## B. Web (real browser)

Tools: agent-browser, Chrome DevTools MCP (`resize_page`,
`take_screenshot`, `emulate`, `lighthouse_audit`, `performance_*`), or
Playwright.

**Layout, at 360 × 800, 768 × 1024, 1280 × 800 (add 1920 if the design
has a wide layout)**
- [ ] No horizontal scroll at any width; 16 px gutters hold on phone
- [ ] Headline and primary CTA visible above the fold at 360 px
- [ ] Images keep their intended crop; no stretched or squashed media
- [ ] Sticky header never overlaps content or anchors (`scroll-margin-top`)
- [ ] Long text: 2× length headings wrap by design; tables scroll inside
      their container, not the page
- [ ] Empty, loading and error states each verified by forcing them

**Theming and type**
- [ ] Light AND dark screenshots at every width, inspected
- [ ] Browser zoom 200% (or root font 32 px): no overlap, no clipped text
- [ ] Contrast passes in both themes (Lighthouse accessibility, or the
      DevTools contrast checker)

**Motion (on a recording, never on stills)**
- [ ] Scroll reveals fire once, don't re-trigger on scroll back, never
      hide content from users who don't scroll-trigger them
- [ ] Menus, dialogs, drawers: open, close, Esc, click outside, smooth both
      ways, focus returns to the trigger
- [ ] Hover and press states under 150 ms; nothing animates layout
- [ ] `prefers-reduced-motion: reduce` → fades or nothing
- [ ] No jank on scroll with CPU throttled 4× (performance trace)

**Navigation and keyboard**
- [ ] Tab order logical; every control reachable; focus visible everywhere
- [ ] Browser back works after every in-page navigation; shareable states
      have URLs
- [ ] Links open where expected; external links marked; nothing traps focus

**Performance and SEO (on the built output)**
- [ ] Lighthouse: performance ≥ 95, accessibility 100, best practices ≥ 95,
      SEO 100
- [ ] LCP < 2.5 s, CLS < 0.05, INP < 200 ms on Fast 3G + 4× CPU
- [ ] Title, description, canonical, OG image render in a share preview
- [ ] `robots.txt`, `sitemap.xml`, JSON-LD validate

**Browser matrix (minimum)**

| Profile | Why |
|---|---|
| Chrome desktop | Primary, DevTools and Lighthouse |
| Safari iOS (simulator or device) | Real mobile Safari quirks: 100vh, safe areas, form controls |
| Firefox desktop | Font rendering, scrollbar and form differences |

Full checklist on Chrome; layout, forms and the hero flow on the others.
