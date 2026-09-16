<!-- Adapted from Appllama Skills (MIT, appllama.io). -->

# Mobile: native fidelity on iOS and Android

This is how the design-bar laws are obeyed on a phone. Default stack is
Expo + React Native; for native SwiftUI / UIKit projects keep the laws and
take implementation details from the Axiom skills when present.

## Platform baseline (Expo / React Native)

Override only if the project already differs:

- **Expo + Expo Router**, React Native, TypeScript.
- `react-native-reanimated` for motion, `react-native-gesture-handler` for
  gestures, `@shopify/flash-list` for any list that can grow.
- `expo-image` for images (and SF Symbols via `source="sf:name"` on iOS),
  `expo-video` / `expo-audio` (never the deprecated `expo-av`).
- `react-native-safe-area-context` for insets. Never hard-code notch
  numbers.
- `process.env.EXPO_OS` over `Platform.OS` for compile-time platform checks.

## Native fidelity laws

These separate "web page in a wrapper" from "native app". Violating any of
them is a finding, not a style preference.

1. **Semantic colors, both themes, day one.** System/semantic tokens
   (`Color.ios.label`, `Color.ios.secondarySystemBackground` from
   `expo-router` on iOS; Material dynamic colors on Android). Every screen
   renders correctly in light AND dark before it is "done". Never pass
   semantic color objects into Reanimated animated styles; resolve to
   strings first.
2. **Native controls over rebuilt ones.** Switch, Slider, SegmentedControl,
   context menus, date pickers: the native control or a faithful wrapper. A
   rebuilt toggle that animates 50 ms differently than iOS's reads as fake
   instantly. Selection table: [native-controls.md](native-controls.md).
3. **SF Symbols / Material Symbols for iconography.** They inherit weight,
   optical size and Dynamic Type. Never three icon families on one screen.
4. **Typography is hierarchy.** Platform type ramp (Large Title / Title /
   Headline / Body / Footnote). One display size per screen. Tabular
   numerals for anything that counts. `selectable` on data users may copy.
5. **Continuous corners.** `borderCurve: 'continuous'` on every rounded
   rectangle. Squircles are the cheapest "feels iOS" win that exists.
6. **Shadows via CSS `boxShadow`**, not legacy `shadow*`/`elevation` props.
   One elevation system per app; shadows are for elevation logic, not
   decoration.
7. **Spacing rhythm.** Base unit 4 or 8, flexbox `gap`, ScrollView padding in
   `contentContainerStyle` never on the ScrollView itself.
8. **Safe areas and the Dynamic Island are part of the design.** Verify with
   content scrolled under the island (does the blur/fade hold?), with the
   home indicator (does the bottom CTA clear it?), in landscape if supported.
9. **Navigation titles belong to the navigator.** Use the stack's native
   title and large-title collapse rather than a hand-rolled header.
10. **Haptics are punctuation.** Selection tick when a value passes a step,
    light impact when something snaps home, success/error for outcomes, on
    the same frame as the visual, one per user action, never the only
    feedback. Never on scroll, never in loops.
11. **Root scroll behavior**: screens that can overflow wrap content in a
    ScrollView (first component in the route) with
    `contentInsetAdjustmentBehavior="automatic"`. `useWindowDimensions`,
    never `Dimensions.get()`.

## Navigation, Expo Router specifics

- `router.push` to go deeper; `router.replace` / `<Redirect>` to move on;
  `router.dismissTo(href)` for "finish this flow and land on X".
- Presentation options: `presentation: 'modal'` (own stack, Cancel/Done),
  `formSheet` with detents for short interruptions, `fullScreenModal` with
  explicit Close for immersive content, `transparentModal` for overlays,
  action sheet for destructive confirms, native context menu for item
  actions, the system controller for share / web / photo picking.
- One-way doors: guard with `Stack.Protected` and land with `replace`, so
  Android back from home exits the app rather than showing Login, and a
  paid paywall never re-opens.
- Block back only via `usePreventRemove` on the modal's root screen, for
  an irreversible request in flight or unsaved work.
- Tabs: each keeps its own stack, re-tap pops to root, full-attention
  screens (composer, player, checkout) live in the root stack above the
  tabs. Deep links land with a real stack (`initialRouteName` /
  `withAnchor`); splash held until session state resolves.
- Do not put a back-navigable flow deeper than 2 steps inside a modal
  without a stack and header of its own. Never block the interactive pop
  gesture. Tab bars: 3–5 items, filled SF Symbol for the active tab, labels
  always on.

## Motion, mobile specifics

Full patterns in [motion.md](motion.md). The short version:

- One spring vocabulary per app: `{ duration: 400, dampingRatio: 1 }` to
  settle, `{ 300, 0.8 }` for sheets; bounce only when the gesture carried
  momentum. Always pass release velocity into the spring.
- Timing under 300 ms with `Easing.bezier(0.23, 1, 0.32, 1)`. Press
  feedback on press-*in*, 100–150 ms: scale 0.97 on buttons and cards,
  background highlight (never scale) on rows, opacity on bar buttons.
- Gesture → animation never hops the JS thread: worklets, shared values
  with `.get()`/`.set()`, `scheduleOnRN` only at gesture end,
  `transform`/`opacity` only, no `entering` on recycled list rows, never
  animate a header's height, keyboard-tracking UI via
  `react-native-keyboard-controller`.
- `useReducedMotion()`: every spatial animation gets a cross-fade fallback.
- Measure 60 fps on a **release build on the slowest supported device**;
  Expo Go and dev builds hide the jank you're hunting
  ([performance.md](performance.md)).

## State and perceived performance, mobile specifics

- TanStack Query for server state, Zustand/Jotai for client state, MMKV
  (not AsyncStorage) for tiny persisted state when latency shows.
- Skeletons only for content whose shape you know; never a full-screen
  spinner for a partial update.
- FlashList for every list, stable keys, `recyclingKey` on `expo-image`
  items, thumbhash/blurhash placeholders.
- Preload the next screen's data on press-in, not on navigation-complete.
- Cold-start TTI and bundle discipline: [performance.md](performance.md),
  measure → optimize → re-measure, never blind memoization.

## Native SwiftUI / UIKit projects

All the laws above hold. Implementation mapping: semantic colors are
`Color(.label)` and friends; native controls are the SwiftUI controls;
navigation semantics map to `NavigationStack` push, `.sheet` with
`presentationDetents`, `.fullScreenCover`, and replacing the root view for
one-way doors; motion is `withAnimation(.spring(duration:bounce:))` and
`.interactiveSpring`; verification is the same simulator loop with
`xcrun simctl io booted screenshot` / `recordVideo`. When the Axiom skills
are installed, load `axiom-swiftui` and `axiom-design` for the details.
