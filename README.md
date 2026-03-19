# Secretary Assistant — Priority Decision Assistant

> A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) Plugin
>
> 思いついたことを声で話すだけ。AIが自動で構造化・優先順位付けし、Googleカレンダーにスケジュールしてくれる Claude Code プラグインです。

**Just speak what's on your mind. AI automatically structures, prioritizes, and schedules it on Google Calendar.**

Speak freely for 2–5 minutes about everything on your mind — using voice input like [Aqua Voice](https://withaqua.com/) — and this Claude Code plugin will automatically classify your tasks with the Eisenhower Matrix, prioritize them with AI reasoning, and schedule time blocks on your Google Calendar.

> Speak → Classify → Schedule. That's it.

## Features

- **Voice-first workflow** — Designed for unstructured voice input; just talk, and the AI extracts and organizes your tasks
- **Eisenhower Matrix classification** — AI reads context from task names and infers urgency/importance without asking follow-up questions
- **Google Calendar integration** — Detects free slots and creates color-coded time blocks per quadrant
- **AI supplemental advice** — Workload assessment, time allocation suggestions, and dependency detection between tasks
- **Multi-day roadmaps** — Supports scheduling tasks that span multiple days
- **Markdown output** — Saves daily priorities to `data/YYYY-MM-DD-priorities.md` for your records

## Demo

### Voice input style (recommended)

Just talk naturally. The AI handles messy, unstructured input:

```
Uhh, so the big thing today is finishing the project proposal — that's due today.
I also need to prep for the team meeting. I think it's this afternoon?
Emails… I've got a few I really need to reply to.
Oh, and I should start putting together slides for next week's study group.
done
```

### Structured input

Of course, clean bullet-point input works too:

```
Write project proposal
Prep for team meeting
Reply to emails
Create study group slides
done
```

### Output

```
┌───────────────────────────────────┬───────────────────────────────────┐
│  🔴 Q1: Urgent & Important        │  🟡 Q2: Important, Not Urgent     │
│  (Do it now)                      │  (Schedule it)                    │
│                                   │                                   │
│  • Write project proposal         │  • Create study group slides      │
│  • Prep for team meeting          │                                   │
├───────────────────────────────────┼───────────────────────────────────┤
│  🟠 Q3: Urgent, Not Important     │  ⚪ Q4: Neither                    │
│  (Delegate / batch)               │  (Eliminate)                      │
│                                   │                                   │
│  • Reply to emails                │                                   │
└───────────────────────────────────┴───────────────────────────────────┘
```

Each task gets a priority ranking with reasoning, estimated duration, and optimization tips.

## Why This Plugin?

I built this as a **personal secretary agent**. The idea is simple:

1. Wake up, open Claude Code, say `/prioritize`
2. Talk for 2–5 minutes about everything you need to do — stream of consciousness, no structure needed
3. Get back a prioritized, time-blocked day with calendar events created

**No code was written to build this.** The entire agent is a single prompt file (`SKILL.md`) powered by Claude Code's [Skills](https://docs.anthropic.com/en/docs/claude-code) system and [MCP](https://modelcontextprotocol.io/) for Google Calendar integration. This is what the Claude Code plugin ecosystem makes possible.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- For Google Calendar integration: Google Calendar MCP server enabled in Claude Code
- For voice input (optional but recommended): [Aqua Voice](https://withaqua.com/) or any speech-to-text tool

## Installation

```bash
# Clone the repository
git clone https://github.com/miyata09x0084/my-priority-decision-assistant.git

# Install as a Claude Code plugin
claude mcp add-plugin ./my-priority-decision-assistant
```

Or simply open Claude Code in the project directory:

```bash
cd my-priority-decision-assistant
claude
```

## Usage

Invoke the skill in Claude Code:

```
/prioritize
```

Or use natural language:

```
I want to organize what I need to do today
Prioritize my tasks
What should I tackle first?
```

### Workflow

1. **Input** — Speak or type your tasks, end with `done`
2. **Classify** — AI sorts tasks into the Eisenhower Matrix (4 quadrants)
3. **Prioritize** — Outputs a ranked list with reasoning and time estimates
4. **Calendar sync** — Optionally creates time blocks on Google Calendar
5. **Save** — Writes results to `data/YYYY-MM-DD-priorities.md`

### Google Calendar Integration

When you choose to sync, time blocks are created with quadrant-specific rules:

| Quadrant | Scheduling Rule | Color |
|----------|----------------|-------|
| 🔴 Q1 (Urgent & Important) | Earliest available slot | Red |
| 🟡 Q2 (Important, Not Urgent) | Afternoon focus block | Yellow |
| 🟠 Q3 (Urgent, Not Important) | Batched into short gaps | Orange |
| ⚪ Q4 (Neither) | Not scheduled | — |

Features include free-time detection, duplicate prevention, multi-day roadmap scheduling, and reminders for urgent tasks.

## Project Structure

```
my-priority-decision-assistant/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata
├── skills/
│   └── prioritize/
│       └── SKILL.md         # Skill definition (the entire agent logic)
├── data/                    # Output files (.gitignored)
├── .gitignore
└── README.md
```

## How It Works

This project leverages Claude Code's **Skills** system:

- `skills/prioritize/SKILL.md` — Defines the agent's behavior as a prompt
- `.claude-plugin/plugin.json` — Plugin metadata (name, description, version)
- Google Calendar API is called via Claude Code's MCP server integration

The entire agent is a prompt — no application code, no backend, no build step. Claude Code's skill + MCP ecosystem handles everything.

## The Eisenhower Matrix

A decision-making framework that classifies tasks on two axes: **urgency** and **importance**.

- **Q1 (Urgent & Important)** — Do it now. Deadlines, crises, time-sensitive deliverables.
- **Q2 (Important, Not Urgent)** — Schedule it. Strategic work, learning, planning. *This is where the most valuable work lives.*
- **Q3 (Urgent, Not Important)** — Delegate or batch. Reactive tasks, minor requests, routine responses.
- **Q4 (Neither)** — Eliminate. Habitual busywork, distractions.

This plugin uses AI context understanding to infer the correct quadrant from task names alone — no manual tagging required.

## Customization

Edit `skills/prioritize/SKILL.md` to customize:

- Classification criteria and keywords
- Default time block durations
- Output format
- Calendar color mapping
- Timezone (default: `Asia/Tokyo`)

> **Note:** This skill supports multiple languages. The default prompts are in English, but it handles input in any language Claude supports.

## License

MIT

## Author

[miyata_ryo](https://github.com/miyata09x0084)
