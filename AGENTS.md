# improve-agents-md Skill

An OpenCode skill for iteratively improving AGENTS.md files through agentic testing.

## Installation

Copy both files to your OpenCode skills directory:

```bash
mkdir -p ~/.opencode/skills/improve-agents-md
cp SKILL.md AGENTS.md ~/.opencode/skills/improve-agents-md/
```

## Usage

```
/improve-agents-md <path-to-agents-md>
```

Examples:
```bash
# Local template
/improve-agents-md ~/templates/my-template/AGENTS.md

# GitHub PR
/improve-agents-md https://github.com/cloudflare/templates/pull/960

# Batch mode
/improve-agents-md batch ~/path/to/templates/
```

## What It Does

1. **Spawns a no-context agent** to build something using only the AGENTS.md
2. **Tracks metrics**: file reads, errors, time to deploy
3. **Generates feedback** in `feedback-01/` directory:
   - `what-was-used.md`
   - `what-was-missing.md`
   - `what-was-not-used.md`
   - `improvement-opportunities.md`
4. **Suggests edits** ranked by impact/effort
5. **Iterates** until quality bar is met

## Quality Bar

An AGENTS.md passes when:

| Metric | Target |
|--------|--------|
| Skills/MCP loaded | 0 (for simple deploy) |
| File reads before first edit | < 5 |
| Errors for common scenarios | 0 |
| Time to deploy | < 5 min |

## Configuration

No configuration needed. The skill uses defaults:

| Setting | Default |
|---------|---------|
| Workspace | `~/Develop/<template>-eval/` |
| Feedback dir | `feedback-01/` |
| Agent type | `general` |

## High-Impact Patterns

The skill looks for and suggests these proven improvements:

### 1. Quick Start for Agents
```markdown
## Quick Start (For Agents)

**To customize**: `path/to/main.tsx`, `config.json`
**Deploy**: `npm run deploy`
**Common issues**: Multi-account? Add `account_id` to config
```

### 2. Troubleshooting Section
```markdown
## Troubleshooting

### "More than one account available"
Add `account_id` to wrangler.json or export `CLOUDFLARE_ACCOUNT_ID`.
```

### 3. Trigger-Based Resources
```markdown
## When to Load Skills

| Situation | Skill |
|-----------|-------|
| Stuck on D1 queries | `cloudflare` |

## When to Connect MCP Servers

| Situation | Server |
|-----------|--------|
| Create databases | `mcp.cloudflare.com` |
```

### 4. Inline Code Patterns
```markdown
## Key Types
\`\`\`typescript
interface Env { DB: D1Database; }
\`\`\`
```

### 5. Verification Checkpoints
```markdown
## Verification

After deploy:
- [ ] URL accessible
- [ ] Data persists
```

## Output

After running, you get:
- Feedback files in `<template>/feedback-01/`
- Suggested edits with priority rankings
- Metrics comparison (before/after)

## Based On

This skill was developed from testing 5 Cloudflare templates:
- d1-template → [d1-guestbook-demo](https://d1-guestbook-demo.cameronwhiteside.workers.dev/)
- react-router-starter → [vibe-check-demo](https://vibe-check-demo.cameronwhiteside.workers.dev/)
- postgres-hyperdrive → [postgres-hyperdrive-demo](https://postgres-hyperdrive-demo.cameronwhiteside.workers.dev/)
- saas-admin-template → [cam-saas-admin-demo](https://cam-saas-admin-demo.cameronwhiteside.workers.dev/)
- durable-chat-template → [fun-chat-demo](https://fun-chat-demo.cameronwhiteside.workers.dev/)

## Documentation

Full documentation: [Wiki](https://wiki.cfdata.org/pages/viewpage.action?pageId=1372553856)
