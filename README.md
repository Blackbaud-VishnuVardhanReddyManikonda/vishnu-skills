# team-learning marketplace

Contains one plugin, `learn`: a teaching skill for code changes, code areas,
documents, procedures, and concepts.

## Install (each teammate)

    /plugin marketplace add <your-org>/<this-repo>

Then install the `learn` plugin from the `team-learning` marketplace, and run
the skill as `/learn:learn <topic>`.

## Once per repo where you use it

Add this to `.gitignore` so private learning progress is not committed:

    .claude/learning/

## Share with a whole repo's contributors

From inside that repo, run once, then commit the `.claude/settings.json` it writes:

    claude plugin marketplace add <your-org>/<this-repo> --scope project

## Before publishing

    claude plugin validate ./learn-marketplace
    claude plugin validate ./learn-marketplace/plugins/learn

Then add the marketplace from the local folder and test with TESTING.md.

## Updating

Edit files under plugins/learn/skills/learn/, bump "version" in both
.claude-plugin/marketplace.json and plugins/learn/.claude-plugin/plugin.json,
and push.
