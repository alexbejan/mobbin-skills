# Playbook: build an app or a site from scratch

The user says "build me a habit tracker", "make the landing page for X",
"design the onboarding". This is the full loop. Do not stop early: the goal
is a finished product where every screen or page is verified against the
best references, not a scaffold.

Platform: `ios` for apps, `web` for sites and web apps. Set one
`task_intent` and keep it for the whole task.

## Phase 1: find the winners

1. Translate the brief into a category and 3–5 named leaders you know
   ship excellent UI in it (fitness coaching → e.g. WHOOP, Noom, Strava,
   MacroFactor; developer SaaS site → Linear, Vercel, Resend, Raycast).
   Mobbin has no revenue filters: your shortlist is your judgment, stated
   in `apps.md` with a one-line reason each.
2. One category search to catch what you did not think of:
   `search_flows(query="<category> onboarding", platform, limit=8)` for
   apps, or `search_sections(query="<category> hero section", limit=15)`
   for sites. Add any app or site that shows up strong.
3. Record the shortlist in `research/<topic>/apps.md`: name, platform,
   why it matters for *this* brief, verdict after study.

## Phase 2: study the journeys of the top 5

Apps: for each shortlisted app, run its decisive flows by name:
`search_flows(query="<App> onboarding")`, `"<App> paywall"`,
`"<App> home"` or the category's signature flow ("<App> workout
session", "<App> food logging"). Look at every per-screen preview, in
order. Fill gaps with `search_screens(query="<App> <screen type>")`.

Sites: for each shortlisted site, run its page by sections:
`search_sections(query="<Site> hero")`, `"<Site> pricing"`,
`"<Site> features"`, `"<Site> footer"`. Then category-wide section
searches (`"pricing page with three tiers and a highlighted plan"`) so you
see the grammar across sites, not one site's taste.

As you go, write `screens.md` (one line per reference: `mobbin_url`, app,
flow or section, what it does well, what to take) and download the ones
that matter into `img/`.

You are extracting the **category's design language**, so read across
products, not just within one:

- What does the first moment look like (app: welcome; site: hero above the
  fold)? What is the one thing it asks you to do?
- How long is onboarding or the sign-up path, and what does each step
  *earn* (permission, personalization data, commitment)?
- Where is the money ask (paywall / pricing), and what carries it (trial
  framing, price anchoring, feature grid, social proof)?
- What is the home or dashboard information hierarchy? What is one tap or
  one scroll away?
- What do empty states, progress, notifications, and errors look like?
- Sites: section order, section count, how much text per section, where
  the proof lives (logos, testimonials, numbers), what the footer carries.

Write the synthesis into `patterns.md`: what every winner does (table
stakes), what only the best do (edge), what all of them do badly (your
opening).

## Phase 3: frame by frame on the top 3

Pick the 3 strongest and re-walk their decisive flow or page. For each
screen or section answer: what is its ONE job, what makes it work, what
would you change. This is where you stop being a catalog and start being a
design director.

## Phase 4: design the spec

From `patterns.md`, write the product's spec: the best ideas across the
studied references, minus the bloat, plus the opening you found.

- **Apps:** screen list with flows in journey order, and the navigation
  map: for every screen, what it *is* (push, modal with its own stack, form
  sheet, full-screen modal, overlay, tab root) and what back does from it,
  including one-way doors (sign-in, onboarding done, purchase, finished
  session). Grammar from design-bar's navigation laws; evidence from the
  flows you walked (note whether each winner presents its composer as a
  modal, its filters as a sheet, its detail as a push, and copy that
  consistency).
- **Sites:** page list, then per page the section order with each
  section's one job, its content budget (headline length, number of
  bullets, one CTA), and what changes on phone width.

Get sign-off on the spec if the user is present; otherwise state your
choices and proceed.

## Phase 5: build screen by screen (or section by section)

For EVERY screen or section, in journey order:

1. Re-open your references for that type (local board first, then a
   fresh `search_screens` / `search_sections` for gaps; run one literal and
   one descriptive query, they surface different results).
2. Build it following **design-bar** end to end (hierarchy, one accent,
   shape lock, native controls or semantic HTML, motion laws, full state
   cycles).
3. Generate image assets with the best image model available (Higgsfield
   CLI or MCP if connected) at the highest quality, one style system for
   the whole product, per design-bar's image-assets reference.
4. **Verification loop until flawless:** run it (simulator for apps,
   browser at phone, tablet and desktop widths for sites), screenshot,
   actually look, exercise the motion, check both themes, fix, repeat. The
   checklist lives in design-bar's verification reference. A screen is not
   done because it compiles; it is done when it stands next to the top-3
   references without embarrassment.
5. State stays boring and strong (server, client, ephemeral separated per
   design-bar). Motion is verified, not assumed.

## Phase 6: the bar

Walk the whole product as a new user, three times: happy path, skeptic
path (skip everything skippable, scroll past everything), abuse path (bad
input, offline or slow network, interrupt mid-flow, every back path,
resize the browser mid-interaction). Compare each flow or page against the
best reference you studied. If any of yours is worse than the best
equivalent in your research, it goes back into the loop. **You cannot
declare the build finished until every screen holds that comparison.**
