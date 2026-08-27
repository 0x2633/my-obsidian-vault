```table-of-contents
```
## Key Takeaways

- **Project skills** in `.claude/skills` are shared automatically through Git — anyone who clones the repo gets them
- **Plugins** let you distribute skills across repositories via marketplaces for broader community use
- **Enterprise managed settings** deploy skills organization-wide with *the highest priority*, ideal for mandatory standards and compliance
- **Subagents DON't automatically see your skills** — you must explicitly list skills in a *custom agent*'s frontmatter `skills` field
- Built-in agents (Explorer, Plan, Verify) **CAN'T access skills at all** — **ONLY** *custom subagents* defined in `.claude/agents` can.

## Committing Skills to Repository
---
The simplest sharing method is committing skills directly to your repository. Place them in `.claude/skills`, and anyone who clones the repo gets those skills automatically — no extra installation needed.

When you push updates, everyone gets them on the next pull. This approach works well for:

- Team coding standards
- Project-specific workflows
- Skills that reference your codebase structure

The `.claude` directory contains your agents, hooks, skills, and settings — all version-controlled and shared with the team through normal Git workflows.

## Distributing Skills Through Plugins
---
Plugins are a way to extend Claude Code with custom functionality designed to be shared across teams and projects. In your plugin project, create a `skills` directory that follows a similar file structure to the `.claude` directory — each skill gets its own folder with a `SKILL.md` file inside.

## Enterprise Deployment Through Managed Settings
---
Administrators can deploy skills organization-wide through managed settings. Enterprise skills take *the highest priority*.

![[Pasted image 20260818153758.png]]

This is the right choice for mandatory standards, security requirements, compliance workflows, and coding practices that _must_ be consistent across the organization. The keyword here is "must."

## Skills and Subagents
---
Subagents **DON'T** automatically see your skills. When you delegate a task to a subagent, it starts with a fresh, clean context.

There are important distinctions to understand:

- **Built-in agents** (like Explorer, Plan, and Verify) **CAN'T** access skills at all
- **Custom subagents** you define _can_ use skills, but only when you *explicitly* list them
- Skills are *loaded when the subagent starts*, not on demand like in the main conversation

When you delegate to below subagent, it has both skills loaded and applies them to every review. First make sure the skills exist in your `.claude/skills` directory.

```markdown
---
name: frontend-security-accessibility-reviewer
description: "Use this agent when you need to review frontend code for accessibility..."
tools: Bash, Glob, Grep, Read, WebFetch, WebSearch, Skill...
model: sonnet
color: blue
skills: accessibility-audit, performance-check
---
```

This pattern works really well when:

- You want *isolated task delegation* with specific expertise
- Different subagents need different skills (frontend reviewer vs. backend reviewer)
- You want to enforce standards in delegated work without relying on prompts



Next to [[Troubleshooting Skills]]