# vishnu-skills

A Claude Code plugin marketplace containing educational and teaching skills for developers.

## Available Skills

### `learn` - Adaptive Teaching Skill
A teaching skill that learns a topic from real sources, finds what you already know, and teaches the gaps.
- Teaches any code change, codebase area, document, procedure, or concept step by step
- Adapts to what the learner already knows
- Creates knowledge docs and progress tracking
- Detects and corrects misconceptions

## Installation (Everyone)

### Step 1: Add the marketplace to Claude Code
Run this command in Claude Code:

```bash
/plugin marketplace add Blackbaud-VishnuVardhanReddyManikonda/vishnu-skills
```

### Step 2: Install the learn plugin
After adding the marketplace:
```bash
/plugin install learn --from vishnu-skills
```

Or use the Claude Code plugin UI to install from the `vishnu-skills` marketplace.

### Step 3: Use the skill
```bash
/learn:learn <topic>
```

Examples:
```bash
/learn:learn this PR about authentication
/learn:learn how the payment pipeline works
/learn:learn what dependency injection is
```

## For Your Repository

### Add to .gitignore
Keep personal learning progress private (don't commit it):

```bash
echo ".claude/learning/" >> .gitignore
```

### Share with team members
To add this marketplace to your entire repository (so teammates get it automatically):

```bash
claude plugin marketplace add Blackbaud-VishnuVardhanReddyManikonda/vishnu-skills --scope project
```

Then commit the `.claude/settings.json` file that gets created.

## Development

### Adding a New Skill to This Marketplace

1. **Create a new skill directory** under `plugins/learn/skills/`:
   ```
   plugins/learn/skills/my-new-skill/
   ├── SKILL.md
   └── references/
   ```

2. **Define the skill** in `SKILL.md` following the same format as `plugins/learn/skills/learn/SKILL.md`

3. **Update versions** in:
   - `.claude-plugin/marketplace.json` (marketplace version)
   - `plugins/learn/.claude-plugin/plugin.json` (plugin version)

4. **Test locally** before publishing:
   ```bash
   claude plugin validate ./plugins/learn
   ```

### Publishing Updates

1. Edit skill files under `plugins/learn/skills/`
2. Bump "version" in both:
   - `.claude-plugin/marketplace.json`
   - `plugins/learn/.claude-plugin/plugin.json`
3. Push to GitHub

### Validation

Before publishing, validate the marketplace and plugin:

```bash
claude plugin validate ./plugins/learn
```

For full testing, see [TESTING.md](./TESTING.md).
