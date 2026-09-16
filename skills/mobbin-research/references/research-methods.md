# Research methods: flows, sections, and how to study like a director

## Flow research: study journeys, not screenshots

Flows are where conversion and retention live. Use them when the question
is "how do winners structure X?" rather than "what does X look like?".

1. `search_flows(query="<category or app> <flow>", platform, limit=5–8)`.
   Good queries: "fitness app onboarding with personalization quiz",
   "subscription paywall after onboarding", "checkout with saved payment
   methods", "Duolingo streak recovery". One journey per query.
2. Each result carries evenly spaced previews plus per-screen previews:
   read them in order. Study 3–5 products' versions of the same flow side
   by side and chart the common spine: step count, what each step asks vs.
   gives, where friction is deliberately placed and where it is removed,
   and what each step *is* (a pushed screen, a modal with its own steps, a
   sheet, an overlay; on web: a page, a stepper, a dialog, a drawer).
   Winners are consistent about presentation; that grammar is part of the
   spec.
3. Page (`page: 2`) while the results still change your spec.

High-value flow studies for almost any product: Onboarding (length,
personalization, permission timing), Paywall or Pricing (placement, trial
framing, price anchoring), Welcome or Hero (first 5 seconds), plus the
category's signature flows (Food Logging, Workout Session, Habit Check-in,
Invoice Creation, Team Invite…).

## Section research (web): pages are built from sections

Websites on Mobbin are cut into sections. This is the web equivalent of
element research, and the fastest way to build a landing page that reads
as designed rather than templated.

1. `search_sections(query="<section type> with <content>", limit=10–15)`
   per section: hero, social proof / logo bar, feature grid, how it works,
   pricing, testimonials, FAQ, final CTA, footer. Name a site to see its
   version: "Linear hero section".
2. Extract the numbers, not the vibe: headline length in words, number of
   CTAs (usually one), number of feature cards (3 or 4), how proof is
   shown, what the footer carries, how much whitespace between sections.
3. Note section order across 5+ sites; convergence is a convention you
   break knowingly or not at all.

## Element research: how winners build one component

Mobbin has no element taxonomy; describe the element inside the query and
let the images answer.

- Apps: `search_screens(query="bottom tab bar with five items and a raised
  center action", platform="ios")`, `"segmented control switching between
  week and month charts"`, `"empty state for a list with an illustration
  and a primary button"`.
- Web: `search_screens(query="data table with inline filters and bulk
  actions", platform="web")`, `search_sections(query="pricing toggle
  monthly annual with savings badge")`.

Extract sizes, placements, label conventions, active-state treatments, how
many items, what gets an icon vs. text.

## Study discipline (what separates research from tourism)

- **Question first.** Every search answers a named question from the
  current task. Knowing the question turns screens into a spec.
- **See everything, in order.** For a journey, read every per-screen
  preview; a sampled journey lies. Notes outlive links: `image_url` dies in
  about 30 days, `mobbin_url` and your notes do not.
- **Cross-product before in-product.** One app tells you its taste; five
  tell you the category's grammar. Divergence between winners is a real
  choice; convergence is a convention.
- **Reputation is context, not truth.** A famous app's paywall is evidence
  about paywalls; its settings screen might still be lazy. Weight evidence
  by whether that surface plausibly drives the product's success.
- **Same platform as the target.** Do not spec a phone screen from web
  references or a landing page from iOS screens, except deliberately, to
  borrow one idea, and say so.
- **Stop at saturation, not before.** A question is answered when new
  results stop changing your spec. Until then, keep paging.

## Working with the user's own references

When the user pastes a `mobbin_url` or a screenshot, that is a requirement,
not a suggestion: start from it. Re-find it (app name + screen description
in `search_screens`) to get its neighbours and the app's other screens, and
put it first in `screens.md`.
