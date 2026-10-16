# visual-learning

An [Agent Skill](https://agentskills.io) that teaches Claude to build animated visual explainers
for technical systems, such as Kafka, database internals, networking, Kubernetes or compilers.

It builds two formats that work together:

- **Guided walkthroughs.** Narrated, chaptered animations that build the picture up step by step.
  They have captions, a scrubber, speed control, chapter buttons, and a pause after each step.
- **Interactive explorers.** Switchable views of one shared model, with drill-down, controls and
  plain-language tooltips on every element.

Every fact shown is checked against real sources, and each explainer lists those sources on the
page. Before building, Claude proposes 2–3 approaches with the sources it checked, and waits for
you to pick one.

## Install

### Claude Code

Clone the repository into your personal skills folder. The folder name must be `visual-learning`,
to match the skill's `name`.

```bash
git clone https://github.com/andrewalumkal/VisualLearning.Skill.git ~/.claude/skills/visual-learning
```

To share it with a team in one project, clone it into that project's `.claude/skills/visual-learning`
folder and commit it there.

To update it later:

```bash
git -C ~/.claude/skills/visual-learning pull
```

### Claude.ai

Zip a folder named `visual-learning` that contains `SKILL.md` and `assets/`, then upload the zip
as a custom skill. Leave out `.git`, `.DS_Store` and `__MACOSX` files.

## Use

Ask Claude to explain something visually, for example:

- "Walk me through how Kafka consumer groups rebalance."
- "Show me how a B-tree page split works."
- "Explain like a video how PostgreSQL and SQL Server store a row."

## Contents

| Path | What it is |
|---|---|
| `SKILL.md` | The skill instructions |
| `assets/example-walkthrough.html` | A short guided-walkthrough template with the full player |
| `assets/example-kafka-explorer.html` | A finished explorer example (Kafka) |

Each example is a single HTML file with no build step. Open it over `http://localhost`, for
example with `python3 -m http.server`.

## License

[MIT](LICENSE)
