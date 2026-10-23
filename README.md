# skill-design-canvas

An [Agent Skill](https://agentskills.io) that teaches design agents how to behave on a shared design canvas: new work in new rows, never touch or delete what's already there, no overlaps, humans make the decisions, notes record the conversation, comments get a reply before they're resolved, and pages stay small.

Version 0.2.0.

## Contents

- `skills/skill-design-canvas/SKILL.md`: the rules and the step-by-step workflow
- `skills/skill-design-canvas/references/notes-style.md`: note tokens and paste-ready snippets
- `skills/skill-design-canvas/references/housekeeping.md`: pages, Contents boards, moving boards
- `skills/skill-design-canvas/references/paper.md`: optional, how the rules map onto one example tool (Paper)

## Install

**skills CLI** (Claude Code, Codex, Cursor and other agents):

```bash
npx skills add nan-labs/skill-design-canvas
```

**Claude Code plugin:**

```bash
claude plugin marketplace add nan-labs/skill-design-canvas
claude plugin install skill-design-canvas@skill-design-canvas
```

Or in one step inside Claude Code: `/plugin install skill-design-canvas --marketplace nan-labs/skill-design-canvas`

**By hand:** copy `skills/skill-design-canvas/` to

- Claude Code: `~/.claude/skills/` (all projects) or `.claude/skills/` (one project)
- Codex: `~/.agents/skills/` or `.agents/skills/`
- Cursor: `~/.cursor/skills/` or `.cursor/skills/`

Any other agent that reads Agent Skills can load the folder; otherwise paste `SKILL.md` into its instructions.

The agent needs a connection to your canvas tool (usually its MCP server) to act on these rules.

## License

MIT © 2026 Ronan Flynn-Curran. See `LICENSE`.
