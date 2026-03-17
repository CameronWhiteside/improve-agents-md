---
name: improve-agents-md
description: Iterative AGENTS.md improvement through no-context demo sessions. Spawns agents to build from templates, collects friction data, suggests tight changes.
---

# /improve-agents-md: Iterative AGENTS.md Quality System

## Purpose

Systematically improve AGENTS.md files by:
1. Running no-context demo sessions with templates
2. Collecting structured feedback on what worked/didn't
3. Generating tight, impactful changes
4. Iterating until agentic developer experience is smooth

## Invocation

```
/improve-agents-md <path-to-agents-md>
```

Or with a GitHub URL:
```
/improve-agents-md https://github.com/cloudflare/templates/pull/960
```

Or batch mode:
```
/improve-agents-md batch <directory-with-templates>
```

---

## Core Insight (From 5-Template Experiment)

AGENTS.md files are **tested by agents building things**, not by humans reading them.

**What agents need:**
- Triggers, not lists
- Code inline, not file references
- Error handling upfront
- Verification checkpoints

**What agents ignore:**
- Passive MCP/Skills lists
- Architectural prose
- Information not needed for current task

---

## Workflow Phases

### Phase 1: Setup

1. Clone or locate the template
2. Record original AGENTS.md content
3. Create workspace: `~/Develop/<template-slug>-eval/`
4. Initialize feedback directory: `feedback-01/`

### Phase 2: No-Context Demo Session

**CRITICAL**: Spawn a fresh agent with NO prior context about the template.

Prompt for spawned agent:
```
Build and deploy something fun/useful using this template.
You have access to the AGENTS.md - that's your primary guidance.
Report back: what worked, what didn't, what was missing.
```

**Capture during session:**
- Every file read (count them)
- Errors encountered
- Time to first successful deploy
- Skills/MCP servers loaded
- Questions the agent couldn't answer from AGENTS.md

### Phase 3: Structured Feedback Collection

Create these files in `feedback-01/`:

#### what-was-used.md
```markdown
# What Was Used from AGENTS.md

| Section | Used? | How |
|---------|-------|-----|
| Setup | Yes | Followed exact commands |
| Configuration | Partial | Needed more context on X |
| ... | ... | ... |

## Usage Statistics
- Total sections: X
- Heavily used: Y
- Partially used: Z
- Not used: W
```

#### what-was-missing.md
```markdown
# What Was Missing

## 1. [Gap Name]
**What happened**: [describe the blocker/friction]
**What was needed**: [what would have helped]
**Impact**: HIGH/MEDIUM/LOW - [explanation]

**Suggested addition**:
```markdown
[exact markdown to add]
```
```

#### what-was-not-used.md
```markdown
# Sections NOT Used

| Section | Why Not |
|---------|---------|
| Skills | No triggers, just passive list |
| MCP Servers | Would have helped IF deployment failed |
```

#### improvement-opportunities.md
```markdown
# Opportunities to Improve

## 1. [Improvement Name]

### Current State
[what it says now]

### Recommended
[what it should say]

**Why**: [brief justification]

---

## Priority Matrix

| Improvement | Effort | Impact | Priority |
|-------------|--------|--------|----------|
| ... | Low | High | P0 |
```

### Phase 4: Generate Targeted Edits

Based on feedback, generate specific edits ranked by impact/effort.

**High-Impact, Low-Effort Patterns (Always Include):**

1. **Quick Start for Agents section**
   ```markdown
   ## Quick Start (For Agents)
   
   **To customize**: `path/to/main.tsx`, `config.json`
   **Deploy**: `npm run deploy`
   **Common issues**: Multi-account? Add `account_id` to config
   **After deploy**: `https://{name}.workers.dev`
   ```

2. **Troubleshooting section**
   ```markdown
   ## Troubleshooting
   
   ### "More than one account available"
   Add `account_id` to wrangler.json or export `CLOUDFLARE_ACCOUNT_ID`.
   
   ### [Other common error]
   [Fix]
   ```

3. **Actionable Skills/MCP triggers**
   ```markdown
   ## When to Load Skills
   
   | Situation | Skill |
   |-----------|-------|
   | Stuck on [X] | `skill-name` |
   
   ## When to Connect MCP Servers
   
   | Situation | Server |
   |-----------|--------|
   | [Specific trigger] | `server-url` |
   ```

4. **Key code inline**
   ```markdown
   ## Key Types
   ```typescript
   interface Env { /* essential types */ }
   ```
   
   ## Core Pattern
   ```typescript
   // The 10-20 lines that matter most
   ```
   ```

5. **Verification checkpoints**
   ```markdown
   ## Verification
   
   After setup:
   - [ ] Config file updated
   - [ ] Types regenerated
   - [ ] Dev server works
   
   After deploy:
   - [ ] URL accessible
   - [ ] Data persists
   ```

### Phase 5: Apply & Iterate

1. Apply edits to AGENTS.md
2. Spawn new no-context session
3. Compare metrics:
   - File reads before first edit
   - Errors encountered
   - Time to deploy
4. If metrics improved, document and continue
5. If not, revert and try different approach

### Phase 6: Output

Final report structure:
```markdown
# AGENTS.md Improvement Report

**Template**: [name]
**Sessions run**: [N]
**Improvement delta**: [metrics]

## Changes Applied

### 1. [Change Name]
- **Before**: [snippet]
- **After**: [snippet]
- **Impact**: [measured improvement]

## Metrics

| Metric | Before | After |
|--------|--------|-------|
| File reads before edit | X | Y |
| Errors encountered | X | Y |
| Deploy time (approx) | Xm | Ym |
| Skills loaded | X | 0 |

## Remaining Gaps

[Issues not yet addressed]
```

---

## Key Patterns Discovered (Experiment output Example)

### What Works

1. **Quick Deploy blocks** - Complete copy-paste sequences
2. **Build quirks** - Critical non-obvious steps get used
3. **Configuration tables** - Binding names, types, requirements
4. **Troubleshooting sections** - Multi-account is universal blocker

### What Gets Ignored

1. **Passive Skills/MCP lists** - No triggers = no usage
2. **Architecture prose** - Only matters when customizing
3. **Deployment sections** - If Quick Deploy exists, deployment section ignored
4. **Maintaining this file** - Meta-instructions not read by agents

### What Was Missing Across All Templates

| Gap | Frequency | Impact |
|-----|-----------|--------|
| Verification steps | 5/5 | MEDIUM |
| Inline code patterns | 4/5 | MEDIUM |
| Error recovery | 5/5 | HIGH |
| Project structure | 4/5 | MEDIUM |
| Expected output  | 3/5 | LOW |

---

## Quality Bar for AGENTS.md

An AGENTS.md passes the quality bar when:

1. **Zero Skills/MCP loaded** for simple deploy task
2. **< 5 file reads** before first edit
3. **No errors** for common scenarios (multi-account, etc.)
4. **< 5 minutes** to working deployment
5. **Agent confidence** - no "I'm not sure" hedging

---

## Spawning Evaluation Agents

Use the Task tool with `general` agent:

```
Task(
  subagent_type="general",
  description="AGENTS.md eval session",
  prompt="""
You are evaluating an AGENTS.md file by building something with this template.

CRITICAL: You have NO prior knowledge of this template. The AGENTS.md is your ONLY guide.

Task: Build and deploy something fun using this template.

Track:
1. Every file you read (keep count)
2. Every error you encounter
3. What was clear vs confusing
4. What information was missing

After deployment succeeds (or fails), report back with structured feedback.

AGENTS.md location: [path]
Workspace: [path]
"""
)
```

---

## Batch Mode

For evaluating multiple files in tandem

```bash
/improve-agents-md batch ~/path/to/templates/
```

Outputs:
- Individual feedback in each template's `feedback-n/` and iterate
- Summary report in `~/agents-md-eval/summary.md`
- Ranked improvements by impact

---

## Example Session Output

```
## /improve-agents-md Results: react-router-starter

**Sessions**: 2 (1 before, 1 after improvements)

### Session 1 Metrics (Before)
- File reads: 10
- Errors: 1 (multi-account)
- Time to deploy: ~8 min
- Skills loaded: 0

### Changes Applied
1. Added "Quick Start (For Agents)" section
2. Added Troubleshooting with multi-account fix
3. Added Project Structure tree
4. Made MCP servers trigger-based

### Session 2 Metrics (After)
- File reads: 3
- Errors: 0
- Time to deploy: ~4 min
- Skills loaded: 0

### Delta
- File reads: -70%
- Errors: -100%
- Time: -50%

Improved AGENTS.md committed to: <path>
```

---

## Maintenance

Update this skill when:
- New common patterns emerge across templates
- Cloudflare tooling changes (new MCPs, Skills)
- Agent capabilities change

Last updated: 2026-03-17
Based on: 5-template experiment (saas-admin, postgres-hyperdrive, react-router, d1-guestbook, durable-chat)
