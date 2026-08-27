```table-of-contents
```
## Key takeaways

- Start with the **skills validator tool** — it catches structural problems before you spend time debugging other things
- If a skill **doesn't trigger**, the cause is almost always the description — add trigger phrases that match how you actually phrase requests
- If a skill **doesn't load**, check that `SKILL.md` is inside a named directory (not at the skills root) and the file name is exactly `SKILL.md`
- If the **wrong skill gets used**, your descriptions are too similar — make them more distinct
- For **runtime errors**, check dependencies, file permissions (`chmod +x`), and path separators (use forward slashes everywhere)

## Categories
---
### Skill Doesn't Trigger

- Check description against how you're actually phrasing requests
- Add trigger phrases users would actually say
- Test with variations like "help me profile this," "why is this slow?", "make this faster"
- If any variation fails to trigger, add those keywords to your description

### Skill Doesn't Load

- The `SKILL.md` file must be inside a named directory, not at the skills root
- The file name must be exactly `SKILL.md` — all caps on "SKILL", lowercase "md"

### Wrong Skill Gets Used

If Claude uses the wrong skill or seems confused between skills, your descriptions are probably too similar. Make them distinct. Being as specific as possible doesn't just help Claude decide when to use your skill — it also prevents conflicts with other similar-sounding skills.

### Skill Priority Conflicts

For example, if there's an *enterprise* "code-review" skill and you also have a personal "code-review" skill, the enterprise one wins every time. Your options:

1. Rename your skill to something more distinct (this is usually the easier path)
2. Talk to your admin about the enterprise skill

### Plugin Skills Not Appearing

Installed a plugin but can't see its skills? Clear the cache, restart Claude Code, and reinstall.

If skills still don't appear after that, the plugin structure might be wrong. This is when the validator tool really earns its keep.

### Runtime Errors

- **Missing dependencies:** If your skill uses external packages, they must be installed. Add dependency info to your skill description so Claude knows what's needed.
- **Permission issues:** Scripts need execute permission. Run `chmod +x` on any scripts your skill references.
- **Path separators:** Use forward slashes everywhere, even on Windows