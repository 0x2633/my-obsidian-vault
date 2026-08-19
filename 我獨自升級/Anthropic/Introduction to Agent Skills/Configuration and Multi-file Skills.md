## Key Takeaways
---
- **`name` and `description` are required** — `allowed-tools` and `model` are optional but powerful additions
- A good description **answers two questions**: *What* does the skill do? *When* should Claude use it?
- **`allowed-tools`** restricts which tools Claude can use when the skill is active — useful for read-only or security-sensitive workflows
- **Progressive disclosure**: keep SKILL.md *under 500 lines* and *link to supporting files (references, scripts, assets)* that Claude reads only when needed
- **Scripts execute without loading their contents into context** — only the output consumes tokens, keeping context efficient.

## Skill Metadata Fields
---
- **name** (required) — Identifies your skill. Use lowercase letters, numbers, and hyphens only. Maximum *64* characters. Should match your directory name.
- **description** (required) — Tells Claude when to use the skill. Maximum *1,024* characters. This is the most important field because Claude uses it for matching.
- **allowed-tools** (optional) — Restricts which tools Claude can use when the skill is active.
- **model** (optional) — Specifies which Claude model to use for the skill.

## Writing Effective Descriptions
---
A good description answers two questions:

1. *What* does the skill do?
2. *When* should Claude use it?

If skill isn't triggering when you expect it to, try adding more keywords that match how you actually phrase your requests. The description is what Claude uses to decide whether a skill is relevant.

## Restricting Tools with allowed-tools
---
![[Pasted image 20260818142235.png]]

If omitting `allowed-tools` entirely, the skill *doesn't restrict* anything.

## Progressive Disclosure
---
Keep essential instructions in **SKILL.md** and put *detailed reference material* in separate files that Claude reads only when needed.

The open standard suggests organizing your skill directory with:

- **scripts/** — Executable code
- **references/** — Additional documentation
- **assets/** — Images, templates, or other data files

Then in **SKILL.md**, link to the supporting files with clear instructions about when to load them:

![[Pasted image 20260818142452.png]]

## Using Scripts Efficiently
---
*Scripts in your skill directory can run without loading their contents into context*. The script executes and only the output consumes tokens. The key instruction to include in your **SKILL.md** is to tell Claude to _run_ the script, **not** _read_ it.

This is particularly useful for:

- Environment validation
- Data transformations that need to be consistent
- Operations that are more reliable as tested code than generated code




Next to [[Skills VS Other Claude Code Features]]