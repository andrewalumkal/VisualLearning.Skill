---
name: visual-learning
description: Build animated visual explainers that teach how a technical system works under the covers (Kafka, databases, networking, Kubernetes, compilers, anything with moving parts). Two formats that work together. Guided walkthroughs are narrated, chaptered animations with captions, a scrubber, speed control and a pause per step. Interactive explorers have switchable views, drill-downs, controls and plain-language tooltips. Fundamentals come first, then complexity. Every fact shown is checked against real sources. The agent proposes 2–3 approaches with references before building, and a separate subagent reviews each part before delivery. Use this skill whenever the user wants to learn or understand how something works, asks to "show me", "visualize", "animate", "walk me through", "explain it like a video", "step by step" or "explain visually", says they are a visual learner, or asks for a diagram that moves, a simulation, a drill-down view, or a playground for a concept, even if they never say the word "visual".
license: MIT
---

# Visual learning explainers

The goal is not to fill in a format. The goal is for a human to **understand** a system by watching
it work: first the fundamentals and the big picture, then the complexity, at their own pace.

Everything below is guidance with reasons, not a recipe. Pick what serves the topic. A B-tree, a
TCP handshake and a Kafka cluster need different pictures, different views and different controls.

There are two formats, and one topic can use both, more than once: **guided walkthroughs** that
the learner watches, and **explorers** that the learner plays with. See "Choose the formats".

Three things are not optional:

- **Accuracy.** Every fact the visual teaches is backed by a source you actually checked.
- **Proposal first.** Propose approaches and present your sources before building anything.
- **Independent review.** After you build each part, a separate reviewer checks it. Fix what it
  finds before you deliver. See "Independent review after each part".

## Accuracy comes first

A learner can't tell a confident animation from a correct one. A wrong visual teaches
misinformation, and teaches it well, so it's worse than no visual. Treat every mechanism, default,
number, name and behavior the visual shows as a claim that needs evidence.

- **Research before you design.** Look things up with the tools you have (web search, fetching
  docs, reading source code in the repo). Don't rely on memory alone.
- **Prefer primary sources:**
  - official documentation
  - specs and RFCs
  - design proposals (for example Kafka KIPs or PostgreSQL docs and source)
  - the project's source code

  After those, well-known engineering write-ups from the maintainers or vendors. Avoid
  unattributed blog posts and forum answers unless nothing better exists, and say so when you use
  them.
- **Mind versions.** Behavior changes between releases. For example, Kafka's ZooKeeper vs KRaft
  modes, or its eager vs cooperative rebalancing. Note which version the visual describes.
- **Only cite what you read.** List a source only if you actually opened it in this session and it
  supports the claim. Never invent or guess a URL.
- **No silent assumptions.** If you can't find a source for something but believe you know how it
  works, say so up front, before building. For example: "I couldn't find a primary source for X.
  From general knowledge I believe it works like Y." Then let the user decide: build it as-is,
  research more, or leave it out.
- **Label simplifications.** A simulation always simplifies (fewer partitions, faster time, a
  sliding window). That's fine, but say so on the page so a simplification isn't mistaken for how
  the system works.
- **Keep the evidence with the explainer.** Every explainer gets a "Sources" list on the page with
  titles and links. When an element shows something you couldn't verify, end its tooltip with "Not
  verified in a source." That way the evidence outlives the planning conversation.
- **Same rule when iterating.** A new view, control or detail added later brings new claims. Check
  them the same way, and add their sources to the list.
- **Captions are claims too.** Every walkthrough caption follows the same rules. Flag anything
  unverified in the caption or tooltip, and list it in the simplifications.

## Who this is for: standing preferences

These came from the user directly. Treat them as defaults unless the user says otherwise:

- **A visual learner who learns by watching actions happen.** For example, a message drops into a
  queue and gets picked up. Motion should show the mechanism. Nothing moves just to decorate.
- **Simple, beautiful, easy to maintain beats lots of visual detail.** Use flat 2D shapes, a few
  colors and a calm layout. Don't use 3D. It was tried and rejected, so reach for it only if the
  user asks.
- **Fundamentals first, complexity on demand.** The first thing on screen teaches the core idea.
  Detail is available, but the learner chooses when to go there.
- **Interactive.** Controls let the learner change the system and see the consequence: rates,
  adding or removing workers, bursts, single steps, failures. A static picture felt incomplete.
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
  change that one thing, then show it again.
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
- **Layers of complexity:** what comes after the fundamentals, roughly in order: partitions, then
  consumer groups and rebalancing, then replication and failover, then retention and compaction.
- **The model:** the entities, what flows between them, and the state that changes (offsets, lag,
  queue depth). One simulated model should drive every view so the views never disagree.

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
  explorer. Examples: an UPDATE that creates a version and later gets cleaned up, a rebalance, a
  failover.
- **A topic can have several of each:** an overview walkthrough, deep-dive walkthroughs for hard
  parts, and explorer views for experimentation.
- **Link them.** A walkthrough ends with "try it yourself", linking to the explorer. An explorer
  view can link to "watch how this works".
- **Keep the style shared.** Both formats use the same colors, shapes, tooltips and Sources list,
  so moving from one to the other feels familiar.

## 4. Guided walkthroughs

A walkthrough is a script: chapters made of steps. Each step has a caption and a short animation.
The picture builds up from an empty canvas to the full system.

- **Build up, don't present.** Introduce every element before it moves. Add one part per step,
  and keep parts where they are, so the picture grows into its final layout. Never show a box the
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
  - A moving dot shows a copy, a write or a lookup. Prefer this to permanent link lines, which
    pile up and clutter the picture.
  - Show an arrow when you introduce a relationship, then remove it.
  - A banner names the operation that runs now, for example "Transaction 102: UPDATE …".
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
| Failure or what-if | How the system reacts to trouble | Stop a broker; a follower becomes leader |
| Timeline or sequence | Ordering and timing between parties | Producer ack, replication, commit, in time order |
| Storage | How it's laid out on disk or in memory | Segment files, index, retention cleanup |

Guidance for choosing:

- **The default view is the fundamentals view.** It's what loads first, and it should make sense in
  under 10 seconds. A learner who never clicks anything should still learn the core idea.
- **Views are peers, not a ladder.** Use buttons or tabs on the main screen to switch between them.
  Don't force the learner through one overview to reach everything.
- **Drill-down is a tool inside a view**, used where containment is the point: a cluster contains
  brokers, a page contains keys. Not every view needs it, and not every topic needs it.
- **Settings can change how a view shows the model**, when that teaches something. Examples: a
  detail toggle (simple vs full labels), slow motion, showing one key only, or failure on/off.
- **Start small.** One or two strong views beat five thin ones. Offer more views as next steps.

## 6. Pick the representation per view

Let the subject's shape decide the picture. Logs and queues read well as horizontal lanes. Trees
read well as trees. Protocols read well as two columns with messages crossing in time. Memory and
disk layouts read well as blocks. Ask: *what drawing would make the mechanism obvious with the
labels removed?*

## 7. Make explorers interactive where it teaches

For each view, and each zoom level within a view, ask: *what would an engineer want to turn up or
down here to see what happens?* Give it 2–4 controls. Show the consequence of every control in the
picture itself: lag numbers climb, an idle consumer turns gray, reading pauses during a rebalance.
Cut any control whose effect you can't see.

Useful kinds of control:

- rates and speed
- counts (producers, consumers, replicas)
- bursts
- single steps, so cause and effect is visible one event at a time
- failure injection
- pause and play

## 8. Tooltips: explain every element in simple words

Give every meaningful element a tooltip: boxes, records, pointers, numbers, legends, buttons and
sliders. Tips on moving things can include live values ("The consumer reads offset 27 next."),
which ties the words to the animation.

Write them in the spirit of ASD-STE100 Simplified Technical English. Follow it loosely, not as a
strict standard. The reader may have no technical background.

- One idea per sentence. Keep sentences short: aim for under 15 words and never go over 20.
  Use 1–3 sentences per tip.
- Active voice, present tense: "Kafka adds each record to the end", not "records are appended".
- Common words, with one meaning each. When a technical term matters (offset, partition, lag), name
  it and say what it is in the same tip: "Lag is the number of records that wait to be read."
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
signpost, not an alarm. Turn it off under `prefers-reduced-motion`. Reserve blue for this marker
and for links, so never use it for data. Zooming in should feel like flying into the box you
clicked, and zooming out should reverse it. Breadcrumbs, a zoom-out button and Escape all go back
up one level.

## Visual style

- Flat shapes, 0.5px borders, rounded corners, no gradients, shadows or glow.
- Wide, short SVG canvases, about 680×320, so a scene fits on screen without scrolling. Text at
  14px for labels and 12px for secondary text. Sentence case. At most two lines of text in a box.
- Color carries meaning, not decoration. Use 2–3 color ramps per scene: for example, one hue per
  key or category, teal for readers, and gray for structure and "done" states. Add a legend when
  color encodes something.
- Work in both light and dark mode.
- Motion is short (0.3–0.6s in explorers), eased and purposeful. In a walkthrough, each motion
  inside a step is about 0.5–2s, and the steps follow the pacing in "Guided walkthroughs". Things
  that persist in the real system should persist in the visual. In Kafka, records that have been
  read fade but stay.
- A walkthrough canvas can be taller (about 680×400), because it builds up a whole system.

## Worked examples and reusable parts

There are two examples, one per format. **Use them as references and parts bins, not molds.**

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
partitions and the consumer group. It's a single HTML file with no build step. **Use it as a
reference and a parts bin, not a mold.** Its structure fits Kafka; your topic may need different
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
  - color ramps that handle light and dark mode
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

For an explorer with a model behind it, run many random actions in a script and assert any
invariants. For example, two engines in a comparison must return the same rows to the same query.

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
  in the build. It only reads and reports. It doesn't edit files, so you make every fix. If you
  have no subagent tool (for example on Claude.ai), do a separate review pass of your own from a
  fresh read of the file. Tell the user that this review wasn't independent.
- **Every change is reviewed.** This includes changes the user asks for later. The review covers
  what changed: a color tweak gets a short visual check, and a new chapter gets the full brief.

### The reviewer's brief

Give the reviewer:

- the file paths, and how to serve them over `http://localhost`
- who the learner is and what they already know
- the outline the user agreed to
- the page's Sources list and the "could not verify" list from the proposal

Ask it to check:

1. **Claims.** Check every caption, tooltip, label, number, default and version against the
   sources. The reviewer opens each source itself instead of trusting the list. It looks for
   newer versions that change the answer. Flag any claim with no source, and any claim the
   source doesn't support.
2. **Teaching.** Each element is introduced before it moves. Each step has one idea. Fundamentals
   come before complexity. Analogies and simplifications are labeled.
3. **Behavior.** Run the checklist in "Verify before saying it's done" in a real browser,
   including dark mode. For an explorer, also try edge cases: reset during an animation, repeated
   clicks, long runs, timers that outlive a reset, and caps or limits in the model.
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
walkthrough. When the learner reacts, change that one thing and show it again. Keep their
preferred style stable across topics so each new topic feels familiar.

A change that adds new facts goes through the same accuracy check. Research it, say what you
couldn't verify, and update the Sources list. Every change also gets an independent review that
covers what changed.
