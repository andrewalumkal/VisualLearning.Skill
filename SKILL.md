---
name: visual-learning
description: Build animated visual explainers that teach how something works or unfolds, in any subject with parts that interact or steps that happen in order. Two formats that work together. Guided walkthroughs are narrated, chaptered animations with captions, a scrubber, speed control and a pause per step. Interactive explorers have switchable views, drill-downs, controls and plain-language tooltips. Fundamentals come first, then complexity. Every fact shown is checked against real sources. The agent proposes 2–3 approaches with references before building, and a separate subagent reviews each part before delivery. Use this skill whenever the user wants to learn or understand how something works or happens, asks to "show me", "visualize", "animate", "walk me through", "explain it like a video", "step by step" or "explain visually", says they are a visual learner, or asks for a diagram that moves, a simulation, a drill-down view, or a playground for a concept, even if they never say the word "visual".
license: MIT
---

# Visual learning explainers

The goal is not to fill in a format. The goal is for a human to **understand** a subject by
watching it work: first the fundamentals and the big picture, then the complexity, at their own
pace.

This skill works for any subject with parts that interact or steps that happen in order. In this
file, *system* means the subject's moving parts, and *model* means the state that changes. Kafka
appears in the examples because the explorer example is about it. Apply each principle to your own
subject.

Everything below is guidance with reasons, not a recipe. Pick what serves the topic. Different
subjects need different pictures, different views and different controls.

There are two formats, and one topic can use both, more than once: **guided walkthroughs** that
the learner watches, and **explorers** that the learner plays with. See "Choose the formats".

Three things are not optional:

- **Accuracy.** Every fact the visual teaches is backed by a source you actually checked.
- **Proposal first.** Propose approaches and present your sources before building anything.
- **Independent review.** After you build each part, a separate reviewer checks it. Fix what it
  finds before you deliver. See "Independent review after each part".

## Accuracy comes first

A learner can't tell a confident animation from a correct one. A wrong visual teaches
misinformation, and teaches it well, so it's worse than no visual. Treat every mechanism, number,
date, name, default and behavior the visual shows as a claim that needs evidence.

- **Research before you design.** Look things up with the tools you have. Don't rely on memory
  alone.
- **Prefer primary and authoritative sources for the field:** official documentation, standards,
  peer-reviewed work and primary records. For Kafka, that means the official documentation, the
  design proposals (KIPs) and the source code. After those, well-known write-ups from recognized
  experts. Avoid unattributed blog posts and forum answers unless nothing better
  exists, and say so when you use them.
- **Mind versions and context.** Facts can change with the version, the date or the place. For
  example, Kafka's ZooKeeper vs KRaft modes. Note which one the visual describes. Where experts
  disagree, say so instead of picking a side without telling the learner.
- **Only cite what you read.** List a source only if you actually opened it in this session and it
  supports the claim. Never invent or guess a URL.
- **No silent assumptions.** If you can't find a source for something but believe you know how it
  works, say so up front, before building. For example: "I couldn't find a primary source for X.
  From general knowledge I believe it works like Y." Then let the user decide: build it as-is,
  research more, or leave it out.
- **Label simplifications.** A simulation always simplifies (fewer parts, faster time, a
  sliding window). That's fine, but say so on the page so a simplification isn't mistaken for how
  the subject works.
- **Keep the evidence with the explainer.** Every explainer gets a "Sources" list on the page with
  titles and links. When an element shows something you couldn't verify, end its tooltip with "Not
  verified in a source." That way the evidence outlives the planning conversation.
- **Same rule when iterating.** A new view, control or detail added later brings new claims. Check
  them the same way, and add their sources to the list.
- **Captions are claims too.** Every walkthrough caption follows the same rules. Flag anything
  unverified in the caption or tooltip, and list it in the simplifications.

## Who this is for: standing preferences

These came from the user directly. Treat them as defaults unless the user says otherwise. The
review is the exception: it always happens.

- **A visual learner who learns by watching actions happen.** For example, a message drops into a
  queue and gets picked up. Motion should show the mechanism. Nothing moves just to decorate.
- **Simple, beautiful, easy to maintain beats lots of visual detail.** Use flat 2D shapes, a few
  colors and a calm layout. Don't use 3D. It was tried and rejected, so reach for it only if the
  user asks.
- **The page style is the one in the examples.** It follows OpenUI's default theme: Inter, neutral
  grays, white cards on a light gray page, pill-shaped switchers and a solid primary button. The
  user compared it side by side with the older warm, thin-bordered style and chose this one. See
  "Visual style".
- **Light and dark mode must both be right, legends included.** In past explainers the marks
  changed color in dark mode but the legend kept its light-mode colors, so the two didn't match.
  See "Light and dark mode".
- **Fundamentals first, complexity on demand.** The first thing on screen teaches the core idea.
  Detail is available, but the learner chooses when to go there.
- **Interactive.** Controls let the learner change the system and see the consequence: rates,
  adding or removing parts, bursts, single steps, failures. A static picture felt incomplete.
- **Connect to what the learner already knows.** Use analogies from their own background. Label
  each one as an analogy, so it isn't mistaken for a fact about the subject.
- **Every element explains itself with a tooltip** in plain, short sentences. See "Tooltips".
- **Drillable things are obvious.** Where a view supports zooming in, mark every zoom target with
  one standard marker: a blue plus badge with a slow pulsing ring.
- **Guided walkthroughs work well.** The user liked a narrated walkthrough that builds the picture
  step by step. It has a scrubber, speed control, chapter buttons, and a pause after each step
  until they press Play.
- **Don't open with a dense diagram.** A full explorer shown first, with boxes nobody had
  introduced, did not help: "I don't even really understand the diagram." Introduce each element
  before it moves.
- **Iterate.** The user reacts ("slower", "fewer colors", "add a view for X") and expects you to
  change that one thing, get it reviewed, then show it again.
- **Reviewed before delivery.** The user requires a separate subagent to review each part, and
  the fixes to happen before it's delivered. This is a requirement, not a preference you can drop.

## 1. Research and plan the learning before the UI

Before drawing anything, research the topic and work out *what the learner needs to understand*,
and in what order. Collect sources as you go (see "Accuracy comes first").

- **Fundamentals:** the 2–4 ideas everything else rests on. For Kafka: a topic is an append-only
  log; reading moves an offset and doesn't delete; keys pick partitions; one consumer per partition
  in a group. If the learner leaves with only these, the visual has done its job.
- **Misconceptions:** what a newcomer probably believes that isn't true ("Kafka is a queue").
  Showing the wrong model next to the right one is one of the strongest teaching moves.
- **Layers of complexity:** what comes after the fundamentals, roughly in order. For Kafka:
  partitions, then consumer groups and rebalancing, then replication and failover, then retention
  and compaction.
- **The model:** the entities, what flows between them, and the state that changes (in Kafka:
  offsets, lag, queue depth). One simulated model should drive every view so the views never
  disagree.

Get the semantics right, because visuals teach whether they're right or wrong. Label any
simplification ("the window shows the last 10 records; real retention is time-based").

## 2. Propose 2–3 approaches, then wait

Don't build yet. Come back to the user with a short proposal and let them choose. A wrong direction
is cheap to fix here and expensive after a build. Use this shape:

```
What it will teach
  The 2–4 fundamentals, in order.

Sources I checked
  - Claim → source title (link)
  - ...

Could not verify (from my own knowledge)
  - Claim, and how confident I am. Or "None."

Option A: <short name>
  Format (walkthrough, explorer, or both), views or chapters, drawing style and key
  interactions. What it teaches well, and what it doesn't.
Option B: <short name>
  ...
Option C: <short name>   (only if there's a genuinely different third idea)
  ...

Recommendation
  Which option and why, in 1–2 sentences.

Review
  The parts, in build order. A separate reviewer checks each one before delivery.
```

The options should differ in a real way, for example:

- a compare-first view vs a flow-first view
- a zoomable structure vs a single-item journey
- a step-through vs a live simulation
- a walkthrough first, then an explorer, vs an explorer alone

They shouldn't be the same idea in three colors. If the topic truly allows only one sensible
approach, say so and offer one. Build only after the user picks an option or says go. If the user
explicitly asks to skip the options, still report the sources and anything you couldn't verify
before building.

When an option includes a walkthrough, show its chapter outline with rough timings, one line per
chapter about what the learner will watch. The user can react to that outline before anything is
built.

## 3. Choose the formats: walkthroughs and explorers

A topic can have two kinds of artifact. They work best together, and you decide the mix.

| | Guided walkthrough | Explorer |
|---|---|---|
| What it is | A narrated, chaptered animation with a scrubber, like a short video | Switchable views with zoom, controls and tooltips |
| Best for | First contact and the big picture. Long chains of cause and effect that must be seen in order | Trying things: what-ifs, rates, failures, comparing states |
| The learner | Watches, pauses, rewinds | Experiments |

Guidance for the mix:

- **First contact: start with a walkthrough.** A learner who doesn't know the topic can't read a
  dense diagram yet. A walkthrough introduces each part before it moves.
- **Complex sequences get their own walkthrough**, even inside a topic that already has an
  explorer. For example, a Kafka rebalance.
- **A topic can have several of each:** an overview walkthrough, deep-dive walkthroughs for hard
  parts, and explorer views for experimentation.
- **Link them.** A walkthrough ends with "try it yourself", linking to the explorer. An explorer
  view can link to "watch how this works".
- **Keep the style shared.** Both formats use the same colors, shapes, tooltips and Sources list,
  so moving from one to the other feels familiar.

## 4. Guided walkthroughs

A walkthrough is a script: chapters made of steps. Each step has a caption and a short animation.
The picture builds up from an empty canvas to the finished scene.

- **Build up, don't present.** Introduce every element before it moves. Add one part per step,
  and keep parts where they are, so the picture grows into its final layout. Never show a shape the
  captions haven't explained yet.
- **One idea per step.** Write the caption in 1–4 short sentences, in the same plain style as the
  tooltips. Give each step 4–10 seconds of motion. Stagger the sub-steps inside a step so the eye
  can follow.
- **Shape:** 5–10 chapters and 3–6 minutes in total. The first chapter teaches the fundamentals
  with no comparison or edge cases. End with a recap screen that sums up the whole story.
- **Player:**
  - Pause automatically at the end of each step, with "Press Play to continue".
  - Play/pause, "Replay step", "Next step" and a speed control (0.75×–1.5×).
  - A draggable scrubber with chapter tick marks, and chapter buttons under it.
  - Captions under the picture, like subtitles, with the chapter and step number.
  - Keyboard: Space plays and pauses, ← replays the step, → goes to the next step.
- **Visual language:**
  - An amber outline marks what changed in this step. It shows only during that step.
  - A moving dot shows something that travels from one part to another. Prefer this to
    permanent link lines, which pile up and clutter the picture.
  - Show an arrow when you introduce a relationship, then remove it.
  - A banner names the event that happens now, for example "Producer sends order 17".
- **Exact scrubbing.** The picture at time t comes from replaying every finished step to its end,
  then the current step at its progress. So steps must be pure: no randomness and no clock reads.

## 5. Explorers: choose views, several ways into one topic

Learning rarely fits one picture with one path into it. Give the learner a main screen with a few
**views** they can switch between, each showing the same model from a different angle. Choose the
2–4 views that serve this topic best. Common kinds:

| View | Teaches | Kafka example |
|---|---|---|
| Basics or compare | The core idea, often next to the misconception | Traditional queue vs Kafka log, side by side |
| Flow | What moves where, live | Producers → partitions → consumers |
| Structure or zoom | What is inside what; drill into parts | Cluster → partitions → one partition's log |
| Lifecycle or journey | One item from birth to end | One record: send, append, replicate, read, expire |
| Failure or what-if | How the subject reacts to trouble or change | Stop a broker; a follower becomes leader |
| Timeline or sequence | Ordering and timing between parties | Producer ack, replication, commit, in time order |
| Layout | Where things sit, in space or in storage | Segment files, index, retention cleanup |

Guidance for choosing:

- **The default view is the fundamentals view.** It's what loads first, and it should make sense in
  under 10 seconds. A learner who never clicks anything should still learn the core idea.
- **Views are peers, not a ladder.** Use buttons or tabs on the main screen to switch between them.
  Don't force the learner through one overview to reach everything.
- **Drill-down is a tool inside a view**, used where containment is the point: a Kafka cluster
  contains brokers. Not every view needs it, and not every topic needs it.
- **Settings can change how a view shows the model**, when that teaches something. Examples: a
  detail toggle (simple vs full labels), slow motion, showing one item only, or failure on/off.
- **Start small.** One or two strong views beat five thin ones. Offer more views as next steps.

## 6. Pick the representation per view

Let the subject's shape decide the picture. Ask: *what drawing would make the mechanism obvious
with the labels removed?* For example, a Kafka log reads well as a horizontal lane.

## 7. Make explorers interactive where it teaches

For each view, and each zoom level within a view, ask: *what would a curious learner want to turn up
or down here to see what happens?* Give it 2–4 controls. Show the consequence of every control in
the picture itself. In Kafka: lag numbers climb, an idle consumer turns gray, reading pauses during
a rebalance. Cut any control whose effect you can't see.

Useful kinds of control:

- rates and speed
- amounts and counts
- bursts and sudden changes
- single steps, so cause and effect is visible one event at a time
- failures, or removing a part
- pause and play

## 8. Tooltips: explain every element in simple words

Give every meaningful element a tooltip: boxes, items, markers, numbers, legends, buttons and
sliders. Tips on moving things can include live values ("The consumer reads offset 27 next."),
which ties the words to the animation.

Write them in the spirit of ASD-STE100 Simplified Technical English. Follow it loosely, not as a
strict standard. The reader may have no background in the subject.

- One idea per sentence. Keep sentences short: aim for under 15 words and never go over 20.
  Use 1–3 sentences per tip.
- Active voice, present tense: "Kafka adds each record to the end", not "records are appended".
- Common words, with one meaning each. When a term from the subject matters (in Kafka: offset,
  partition, lag), name it and say what it is in the same tip: "Lag is the number of records that
  wait to be read."
- No filler or hedging ("basically", "simply", "just", "it's worth noting"), no idioms, no jokes.
- Say what the thing *is* or *does*, then what to notice: "Send 10 records quickly. Look at the lag
  increase."
- Zoom targets end with "Click to zoom in."

| Element | Tip |
|---|---|
| Producers box | Producers are apps that send records to Kafka. A record is one message, for example one order. |
| Read record | Record at offset 12, key user-1. The consumer read it. Kafka keeps it. |
| Idle consumer | Consumer 4 has no work. The topic has 3 partitions, so a 4th consumer waits. It can replace a consumer that stops. |
| Consume button | Read one record. The consumer offset moves forward by one. The record stays in the log. |

## 9. Make drill-down obvious (when a view has it)

Mark every zoom target with the same badge: a blue plus in a tinted circle, with a ring that
pulses slowly, about every 2.4s. On hover it fills solid. Explain the marker once on the page: "A
pulsing blue plus means you can click to zoom in." Keep the pulse slow and subtle so it reads as a
signpost, not an alarm. Turn it off under `prefers-reduced-motion`. Reserve blue for this marker,
so never use it for data or for other controls. Links are neutral and underlined. Zooming in should
feel like flying into the box you clicked, and zooming out should reverse it. Breadcrumbs, a
zoom-out button and Escape all go back up one level.

## Visual style

The style has two layers: the **page** around the picture (title, controls, captions, sources) and
the **canvas** (the SVG that teaches). Copy the CSS tokens from the examples instead of inventing
new ones, so every explainer looks like part of one set.

The page follows OpenUI's default theme (the `@openuidev/react-ui` theme tokens):

- **Type:** Inter from Google Fonts, falling back to `system-ui`. Body 16px, line height 1.5,
  letter spacing −0.01em. Headings weight 600, labels and emphasis weight 560, body weight 400.
- **Color:** neutral grays with no warm or cool tint. A light gray page, white cards, near-black
  text, secondary text at 60% opacity, and hairline borders at 6–12% opacity. The primary accent is
  neutral too: near-black in light mode and near-white in dark mode.
- **Cards:** the canvas, the caption panel, the controls row and the Sources section each sit on a
  white card with a 1px hairline border and 14–16px corners.
- **Controls:** buttons have 10px corners, a 1px border and a very soft shadow that goes away on
  hover. The main action (Play, Pause) is a solid accent button. View switchers and chapter buttons
  are a segmented pill: a tinted track, with the active item as a raised white chip.
- **Sliders and scrubber:** a thin, fully rounded track with a white round thumb.
- **Tooltips:** a white card with a hairline border, 10px corners and a larger soft shadow.

The canvas stays flat:

- Flat shapes, thin borders, rounded corners, no gradients, shadows or glow. Shadows belong to the
  page controls only.
- Wide, short SVG canvases, about 680×320, so a scene fits on screen without scrolling. Text at
  14px for labels and 12px for secondary text. Sentence case. At most two lines of text in a box.
- Color carries meaning, not decoration. Use 2–3 color ramps per scene: for example, one hue per
  category, one hue for the things that act on them, and gray for structure and "done" states.
  Add a legend when color encodes something. The gray ramp uses the same neutral grays as the page.
- Motion is short (0.3–0.6s in explorers), eased and purposeful. In a walkthrough, each motion
  inside a step is about 0.5–2s, and the steps follow the pacing in "Guided walkthroughs". Things
  that persist in the real system should persist in the visual. For example, Kafka records that have
  been read fade but stay.
- A walkthrough canvas can be taller (about 680×400), because it builds up a whole picture.

### Light and dark mode

Every explainer follows the system color scheme, and every color on it, legends included, must
change with it. A legend that keeps its light-mode colors while the marks switch to dark-mode
colors teaches the wrong mapping, so treat it as a broken page, not as polish.

- **Every color is a token.** Define each color as a CSS variable or class, and set it three times:
  light as the default on `:root`, dark under `@media (prefers-color-scheme: dark)` with
  `:root:not([data-theme="light"])`, and dark again under `:root[data-theme="dark"]`. The
  examples do this, for page colors and for data colors.
- **A legend uses the same token as the marks it explains.** Give the swatch the mark's class or
  variable, for example `<span class="sw c-purple">` next to records drawn with
  `<g class="c-purple">`. Then the two can't drift apart. If the mark is a pale fill with a strong
  outline, the swatch is too.
- **No color literals in markup.** A hex color in a `fill="#…"` attribute, a
  `style="background:#…"` or a JavaScript constant can't follow the scheme. Use a class, or
  `style="fill:var(--name)"` (`var()` doesn't work inside SVG presentation attributes). The only
  exception is a mid-tone that is readable on both backgrounds and used identically in both modes,
  such as a moving dot. If a legend explains that dot, the legend uses the same constant.
- **The same goes for everything that shows a color:** legend chips, key buttons, color words in
  tooltips ("purple records"), and HTML next to the canvas.
- **Check it in both modes.** In dark mode, put each legend swatch next to the mark it explains and
  confirm they match. Then do the same in light mode.

## Worked examples and reusable parts

There are two examples, one per format. **Use them as references and parts bins, not molds.**
Both are about computing topics, but their parts work for any subject.

### Walkthrough template

`assets/example-walkthrough.html` is a short template, about 200 lines: 2 chapters and 5 steps on
"what a database page is". It has the complete player. Copy it and replace the script.

- **Script:** `chapter(name)` and `step(duration, caption, fn)`. `fn(S, u)` draws the step at
  progress `u` (0 to 1) into the scene object `S`.
- **Actions:**
  - `add`, `del` and `mv` create, fade out and animate items. In `mv`, numbers animate, and other
    properties (labels, colors, tooltips) switch halfway.
  - `hl` adds the focus outline for the current step only.
  - `fly` sends a dot from one point to another.
  - `seg(u, a, b)` staggers sub-steps inside one step.
- **Items:** `box`, `text`, `arrow` and dots, each with an optional tooltip.
- **Replay:** `stateAt(t)` replays the finished steps with `u = 1`, then the current step. This
  makes the scrubber exact.
- **Player:**
  - auto-pause at each step end, with `skipPause` to resume past it
  - scrubber with chapter ticks, chapter buttons, Replay step, Next step, speed, keyboard

### Explorer example

`assets/example-kafka-explorer.html` is one finished explainer, built for this user. It has two
views, "Basics: queue vs log" and "System: flow and zoom", and the System view drills into
partitions and the consumer group. It's a single HTML file with no build step. Its structure fits
Kafka; your topic may need different
views, layouts and controls.

Parts worth reusing as they are:

- **The state → render pattern.** One state object `S` is the source of truth. `setTimeout` loops
  advance it, re-reading the slider values each tick. Animations are short-lived events
  (`emit(...)`) that the scenes draw. Every view stays consistent, and a view can be auto-playing
  and interactive at the same time.
- **Scenes:** `{name, parent, rect, st(), dyn(now), controls()}`.
  - A root scene (no `parent`) is a view, listed in `VIEWS`.
  - A child scene is a zoom level. `rect` is its box on the parent level, which the zoom animation
    flies into.
  - Scene keys can carry an argument (`part:2`), so one definition can serve every partition.
- **Engine:**
  - zoom camera, crossfade between views, breadcrumb, zoom-out button, Escape, trackpad pinch
  - controls wiring (`C.stepper`, `C.slider` and `C.button`, with `ACT` handlers)
  - tooltips (`tip()` and `data-tip`)
  - the pulsing drill badge (`plus(x,y)`)
  - color ramps that handle light and dark mode, and legend swatches (`swatch(cls)`) that use the
    same ramp class as the marks
  - the page theme tokens, set for light, dark from the system, and forced dark
  - a "Sources" list rendered from a `SOURCES` array, plus a simplifications note

## Delivering

- **Inline in chat**, if you have an inline visual or widget tool (for example `show_widget`). This
  is best for quick iterations. Convert to a fragment: drop `<html>`, `<head>` and `<body>`, use
  the host's CSS variables and color classes, and avoid `position: fixed`.
- **As a file**, when the user wants to keep it or no inline tool exists. Save it as a standalone
  `<topic>-walkthrough.html` or `<topic>-explorer.html` in the user's project folder. When a topic
  has both, link them to each other with relative links. Browser previews may not run `file://`
  pages live, so serve the folder (for example `python3 -m http.server`) and open it over
  `http://localhost`.
- Deliver only after the independent review and its fixes are done.
- After it appears, write a short guide in prose. For a walkthrough, list the chapters and how to
  use the player. For an explorer, give 3–5 things to try, and point to the fundamentals view
  first. Add the review summary (see "Tell the user").

## Verify before saying it's done

Open it and go through every view, every zoom level and every control. Check for:

- console errors
- labels overlapping each other or shapes
- clicks that don't register
- missing tooltips
- numbers with float junk such as `0.30000000000000004`
- dark mode readability
- every legend swatch matches the mark it explains, in light mode and in dark mode
- every claim matches a source in the page's Sources list, or is marked "Not verified in a source"
- simplifications are labeled

For a walkthrough, also check:

- Play stops at the end of every step, and Play again continues into the next step.
- A sweep through the whole timeline in small increments (for example every 0.3s) of `stateAt`
  throws no errors and gives no `NaN` positions.
- A screenshot at the end of each step shows no overlapping text and no labels that overflow
  their boxes.
- Chapter buttons, the scrubber, Replay step, Next step, and the final recap screen all work.
- The total time matches what the page says.

For an explorer with a model behind it, run many random actions in a script and assert what must
always hold in the model. For example, in Kafka, lag is never negative, and no partition has two
consumers in the same group.

If you can't run it, say so plainly.

## Independent review after each part

This is a requirement, not a default. Every part gets an independent review after you build it,
and you fix what the review finds before you deliver it. A part is one walkthrough, one explorer,
or one other page such as an index. Your own check in "Verify before saying it's done" doesn't
count, because the builder misses the same things twice.

- **Order.** Build the part, run your own checks, get it reviewed, fix, check the fixes, then
  deliver it. Then start the next part. Parts often share code (one player, one color set). If you
  fix a part before building on it, the parts built later get those fixes too.
- **Don't wait for the user between parts if they said not to.** The reviews still happen.
- **Who reviews.** Use a separate subagent that starts with a fresh context and didn't take part
  in the build. It only reads and reports. It doesn't edit files, so you make every fix. If your
  tool can't start a subagent, do a separate review pass of your own from a fresh read of the
  file. Tell the user that this review wasn't independent.
- **Every change is reviewed.** This includes changes the user asks for later. The review covers
  what changed: a color tweak gets a short visual check, and a new chapter gets the full brief.

### The reviewer's brief

Give the reviewer:

- the file paths, and how to serve them over `http://localhost`
- who the learner is and what they already know
- the outline the user agreed to
- the page's Sources list and the "could not verify" list from the proposal

Ask it to check:

1. **Claims.** Check every caption, tooltip, label, number, date, default and version against the
   sources. The reviewer opens each source itself instead of trusting the list. It looks for
   newer versions that change the answer. Flag any claim with no source, and any claim the
   source doesn't support.
2. **Teaching.** Each element is introduced before it moves. Each step has one idea. Fundamentals
   come before complexity. Analogies and simplifications are labeled.
3. **Behavior.** Run the checklist in "Verify before saying it's done" in a real browser,
   in light mode and in dark mode. Compare each legend swatch to the marks it explains in both.
   For an explorer, also try edge cases: reset during an animation, repeated clicks, long runs,
   timers that outlive a reset, and caps or limits in the model.
4. **Consistency.** Nothing contradicts an earlier part. Terms, colors and the legend match
   across parts.

Ask for findings in this shape:

```
Severity   Wrong | Misleading | Broken | Polish
Where      chapter and step, or view and control
Problem    one or two sentences
Evidence   a source quote with its link, or the steps to reproduce
Fix        a suggested change
```

Also ask for a short list of what it checked and found correct, so the review's coverage is
visible.

### Fix, then check the fixes

- **Always fix** confirmed findings that are Wrong, Misleading or Broken, plus missing sources
  and unreadable text.
- **Fix Polish findings** when the fix is cheap and makes the page clearer.
- **Skip a finding** only with a reason: it goes against the user's stated preferences, it's out
  of scope, or it's a matter of taste. Never skip a Wrong, Misleading or Broken finding this way.
- **The reviewer can be wrong.** Check the source yourself before you change a claim. Don't
  change correct content just because the reviewer said so. Note the disagreement in the summary.
- **Check each fix.** Rerun the reviewer's steps to reproduce, reopen the source, and repeat the
  checks the fix could affect.
- **Re-review large fixes.** If the fixes changed a lot of claims or model logic, ask a new
  reviewer to check only what changed. Stop after two rounds, and report anything still open.

### Tell the user

When you deliver the part, include a short review summary, for example: "Review: 9 findings, 7
fixed, 2 skipped." Name each skipped finding and give the reason. Mention any finding that is
still open.

## Pitfalls already solved in the examples

Walkthrough:

- **Resuming from a pause point.** Playback stops just before the step ends. Pressing Play there
  paused again at once, until `skipPause` let the player run past that one point.
- **Space on a focused button.** The button's own click and the page's Space handler both toggled
  play. Ignore keyboard shortcuts when the target is a button or a select.
- **Impure steps.** Randomness or clock reads inside a step make scrubbing show a different
  picture each time. Keep all variation in the script itself.
- **Prefix deletes.** A helper that removes items by ID prefix (for example every ID that starts
  with `pc`) can remove items you meant to keep. Give each group a distinct prefix.
- **SVG text collapses spaces.** Use visible separators such as " · " instead of runs of spaces.
- **Fixed-width boxes.** Long labels overflow. Check the longest label in each box at the end of
  each step.

Both formats:

- **Legends stuck in light mode.** A legend swatch with its own hex color
  (`style="background:#7F77DD"`) stayed the same in dark mode while the marks it explained switched
  to their dark ramp colors. Give the swatch the marks' class (`sw c-purple`) so both come from one
  token. See "Light and dark mode".
- **Dark rules that ignore a forced theme.** A bare `@media (prefers-color-scheme: dark)` rule
  wins even when the page sets `data-theme="light"`. Guard it with `:root:not([data-theme="light"])`
  and repeat it under `:root[data-theme="dark"]`.

Explorer:

- **Lost clicks.** Anything redrawn every frame swallows clicks, because the element is replaced
  between mousedown and mouseup. Keep clickable shapes in a static layer and make the per-frame
  layer `pointer-events: none`. Only dynamic shapes that have a tooltip opt back in. Resolve clicks
  with `document.elementsFromPoint`, so a hoverable shape on top can't block the box under it.
- **Zoom vs. switching views.** Zoom with a CSS `transform` on one wrapping `<g>`
  (`transform-origin: 0 0`). Switch views with a short crossfade instead, because flying the camera
  between unrelated views would imply containment that doesn't exist.
- **Pinch to zoom.** A trackpad pinch arrives as a `wheel` event with `ctrlKey`. Handle only those,
  and never hijack plain scrolling.
- **Tooltip placement.** Position the tooltip `absolute` inside a `position: relative` wrapper,
  never `fixed`, so it also works inside chat widgets (iframes). Flip it near the edges.
- **Sliding windows.** When only the last N items fit, slide the window, clamp pointers to it, and
  keep the true numbers (lag, offsets) in the readouts.
- **Controls follow context.** Rebuild the controls row whenever the view or zoom level changes,
  and avoid disabled buttons. Round every displayed number.

## Iterating with the learner

End each delivery by offering 2–3 concrete next steps, such as another view, a failure control, a
simpler basics view, a deep-dive walkthrough of a hard part, or an explorer to go with a
walkthrough. When the learner reacts, change that one thing, get it reviewed and show it again. Keep
their preferred style stable across topics so each new topic feels familiar.

A change that adds new facts goes through the same accuracy check. Research it, say what you
couldn't verify, and update the Sources list. Every change also gets an independent review that
covers what changed.
