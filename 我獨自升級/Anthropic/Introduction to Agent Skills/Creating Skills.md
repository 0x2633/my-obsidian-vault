```table-of-contents
```
- Step 1:
	```bash
	mkdir -p ~/.claude/skills/pr-description
	```
- Step 2:
	```markdown
	--- 
	name: pr-description 
	description: Writes pull request descriptions. Use when creating a PR, writing a PR, or when the user asks to summarize changes for a pull request. 
	--- 
	
	When writing a PR description: 
	
	1. Run `git diff main...HEAD` to see all changes on this branch 
	2. Write a description following this format: 
	   
	## What 
	One sentence explaining what this PR does. 
	
	## Why 
	Brief context on why this change is needed 
	
	## Changes 
	- Bullet points of specific changes made 
	- Group related changes together 
	- Mention any files deleted or renamed
	```

The **name** identifies your skill. The **description** tells Claude when to use it — *this is the matching criteria*.

## Testing Your Skill
---
Claude Code loads skills **at startup**.![[Pasted image 20260814170243.png]]

## How Skill Matching Works
---
When Claude Code starts, it scans four locations for skills but only loads the **name and description** — not the full content.

When send a request, Claude compares your message against the **descriptions** of all available skills. Once a match is found, Claude asks you to confirm loading the skill. After you confirm, Claude reads the complete `SKILL.md` file and follows its instructions.

## Skill Priority
---
1. **Enterprise** — managed settings, highest priority
2. **Personal** — your home directory (`~/.claude/skills`)
3. **Project** — the `.claude/skills` directory inside a repository
4. **Plugins** — installed plugins, lowest priority

**Enterprise** level saves the **managed-settings.json** at **system level(OS level)** location.

| OS          | PATH                                                            |
| ----------- | --------------------------------------------------------------- |
| macOS       | `/Library/Application Support/ClaudeCode/managed-settings.json` |
| Linux / WSL | `/etc/claude-code/managed-settings.json`                        |
| Windows     | `C:\ProgramData\ClaudeCode` or `C:\Program Files\ClaudeCode\`   |

## Updating and Removing Skills
---
To update a skill, edit its `SKILL.md` file. To remove one, delete its directory. Restart Claude Code after any changes for them to take effect.




Next to [[Configuration and Multi-file Skills]]