# visual-learning

A [skill](SKILL.md) that teaches an AI agent to build animated visual explainers for any subject
with parts that interact or steps that happen in order. It's plain Markdown instructions plus two
example HTML files, so it isn't tied to one AI model or tool.

It builds two formats that work together:

- **Guided walkthroughs.** Narrated, chaptered animations that build the picture up step by step.
  They have captions, a scrubber, speed control, chapter buttons, and a pause after each step.
- **Interactive explorers.** Switchable views of one shared model, with drill-down, controls and
  plain-language tooltips on every element.

Both formats follow the system's light or dark mode.

Every fact shown is checked against real sources, and each explainer lists those sources on the
page. Before building, the agent proposes 2–3 approaches with the sources it checked, and waits
for you to pick one.

After each part is built, a separate reviewer agent (a subagent) reviews it. The reviewer checks
the claims against the sources, how well it teaches, and how it behaves in a browser. The agent
fixes the findings and checks the fixes before it delivers the part, and reports what was fixed and
what was skipped. Where no subagent is available, the agent does its own fresh review and says it
wasn't independent.

## Install

### Any AI agent

The agent needs to read files, search the web and write HTML. A browser it can drive, and the
ability to start a subagent, make the checks and reviews stronger.

- If your tool supports skills, put this folder where it loads them. `SKILL.md` starts with a short
  YAML header (`name` and `description`) that tells the tool when to use it.
- Otherwise, give the agent `SKILL.md` as its instructions, for example as a custom instructions
  file, or by asking it to read `SKILL.md` before it starts. Keep `assets/` next to it so the agent
  can open the examples.

The steps below are for two tools that load skills from a folder.

### Claude Code

Clone the repository into your personal skills folder. The folder name must be `visual-learning`,
to match the skill's `name`.

```bash
git clone https://github.com/andrewalumkal/VisualLearning.Skill.git ~/.claude/skills/visual-learning
```

To share it with a team in one project, clone it into that project's
`.claude/skills/visual-learning` folder and commit it there.

To update it later:

```bash
git -C ~/.claude/skills/visual-learning pull
```

### Claude.ai

Zip a folder named `visual-learning` that contains `SKILL.md` and `assets/`, then upload the zip
as a custom skill. Leave out `.git`, `.DS_Store` and `__MACOSX` files.

## Use

Ask the agent to explain something visually, for example:

- "Walk me through how Kafka consumer groups rebalance."
- "Show me how compound interest grows."
- "Explain like a video how a bill becomes law."

## Contents

| Path | What it is |
|---|---|
| [`SKILL.md`](SKILL.md) | The skill instructions |
| [`assets/example-walkthrough.html`](assets/example-walkthrough.html) | A short guided-walkthrough template with the full player |
| [`assets/example-kafka-explorer.html`](assets/example-kafka-explorer.html) | A finished explorer example (Kafka) |

Each example is a single HTML file with no build step. Open it over `http://localhost`, for
example with `python3 -m http.server`.

## License

[MIT](LICENSE)
