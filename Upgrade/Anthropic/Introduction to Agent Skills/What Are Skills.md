```table-of-contents
```
*Reusable markdown files that teach Claude Code how to handle specific tasks automatically*.

- **Skills are folders of instructions** that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a `SKILL.md` file with a name and description in its frontmatter.
- **Claude uses the description to match skills to requests.** When you ask Claude to do something, it compares your request against available skill descriptions and activates the ones that match
- **Personal skills** go in `~/.claude/skills` and follow you across all projects. **Project skills** go in `.claude/skills` inside a repository and are shared with anyone who clones it
- **Skills load on demand** — unlike **CLAUDE.md** (which *loads into every conversation*) or slash commands (which require explicit invocation), skills activate automatically when Claude recognizes the situation
- If you find yourself **explaining the same thing to Claude repeatedly**, that's a skill waiting to be written.

Skills are **folders** of *instructions and resources* that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a `SKILL.md` file with a name and description in its frontmatter.![[Pasted image 20260814100922.png]]

The **description** is how Claude decides whether to use the skill. Below the frontmatter, you write the actual instructions.

## Where Skills Live
---
- **Personal skills** go in `~/.claude/skills`. These follow you across all your projects — your commit message style, your documentation format, how you like code explained.
- **Project skills** go in `.claude/skills` inside the root directory of your repository. Anyone who clones the repo gets these skills automatically. This is where team standards live, like your company's brand guidelines, preferred fonts, and colors for web design.

## Skills vs. CLAUDE.md vs. Slash Commands
---
Skills are unique because they're **automatic** and **task-specific**. Here's how they compare:

- **CLAUDE.md** files load into every conversation. If you want Claude to always use TypeScript's strict mode, that goes in CLAUDE.md.
- **Skills** load on demand when they match your request. *Claude only loads the name and description initially*, so they don't fill up your entire context window. Your PR review checklist doesn't need to be in context when you're debugging — it loads when you actually ask for a review.
- **Slash commands** require you to explicitly type them. Skills don't. Claude applies them when it recognizes the situation.

When Claude matches a skill to your request, you'll see it load in the terminal:![[Pasted image 20260814164723.png]]

## When to Use Skills
---
- Code review standards your team follows
- Commit message formats you prefer
- Brand guidelines for your organization
- Documentation templates for specific types of docs
- Debugging checklists for particular frameworks

The rule of thumb is simple: *if you find yourself explaining the same thing to Claude* **repeatedly**, that's a skill waiting to be written.



Next to [[Creating Skills]]