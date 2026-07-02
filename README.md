# frag-knowledge

Claude Code Skill for knowledge management. Zero external dependencies.

## What is this

A workflow skill for Claude Code that turns fleeting thoughts into structured, reviewable knowledge. The core philosophy: **AI asks questions, human summarizes.**

## Three flows

| Flow | Trigger | What it does |
|------|---------|-------------|
| Capture | "记一下xxx" | Save raw content, ask Socratic questions, co-create reflection |
| Browse | "翻一翻" | FSRS-scheduled review of saved notes |
| Review | "帮我审一下" | Audit knowledge base for stale/incomplete notes |

## How to install

Copy `SKILL.md` to `~/.claude/skills/frag-knowledge/SKILL.md`.

Then in Claude Code:
```
/frag-knowledge 帮我记一下xxx
```

## Storage

```
~/.frag-knowledge/
  inbox/              ← new captures (draft)
  reviewing/          ← being reworked
  mature/             ← archived
    concepts/         ← methodology, principles
    sources/          ← articles, talks, papers
    entities/         ← people, companies, tools
  review-state.json   ← FSRS scheduling state
  pending-cards.json  ← cards pending Anki sync
```

## License

MIT
