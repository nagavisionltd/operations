# NagaVision Operations

> Live operational state for the NagaVision Empire. Hitlists, agent configs, daily digests, project tracking. Phone-editable.

## Layout

```
.
├── README.md           # you are here
├── state.json          # current hitlist (active tasks, open Qs, decisions, deadlines)
├── agents/             # permanent cron agent configs
│   ├── hitlist-ops.md
│   └── funding-watcher.md
├── rules/              # business rules, preferences, guardrails
│   └── preferences.md
├── projects/           # one file per active project, current status
│   ├── project-solander.md
│   ├── journey-into-x.md
│   ├── tgbss.md
│   ├── naga-hosting.md
│   ├── uk-city-beat-em-up.md
│   ├── rebourne-26.md
│   └── voidborn.md
└── digests/            # daily logs from cron agents
    ├── hitlist-ops/
    └── funding-watcher/
```

## How it works

- **Rose** reads `state.json` every morning for the daily hitlist digest.
- **Cron agents** write their digests into `digests/<agent>/YYYY-MM-DD.md`.
- **You (Curtis)** can edit anything on phone via GitHub mobile — Rose picks up on next session.
- **Conventions:** small commits, descriptive messages, never commit secrets.

## Quick edit (phone)

1. Open github.com/nagavisionltd/operations on mobile
2. Tap any file → pencil icon → edit
3. Commit directly to `main`

Rose sees your changes on the next session start.
