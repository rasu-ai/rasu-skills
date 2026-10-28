# Rasu AI Agent Skills

Agent skills for making films with [Rasu AI](https://rasu.ai): the studio that turns stories into video, with consistent characters and shots from one episode to the next.

You can [read SKILL.md on GitHub](https://github.com/rasu-ai/rasu-skills/blob/main/skills/rasu/SKILL.md) or fetch its [raw Markdown](https://raw.githubusercontent.com/rasu-ai/rasu-skills/main/skills/rasu/SKILL.md).

## Install

The skill works through the Rasu AI MCP server, so connect that first, then install the skill with one of the methods below.

### Connect Rasu AI

In Claude Code, run:

```bash
claude mcp add --transport http rasu https://api.rasu.ai/mcp --scope user
```

In any other agent, add a remote MCP server named `rasu` with the URL `https://api.rasu.ai/mcp` (Streamable HTTP). No API key is needed: the first time your agent calls Rasu AI, approve the sign-in in the browser window that opens.

### Claude Code plugin

Run in your terminal:

```bash
claude plugin marketplace add rasu-ai/rasu-skills
claude plugin install rasu@rasu-ai
```

### Other agents via skills.sh

```bash
npx skills add rasu-ai/rasu-skills --skill rasu
```

Select your agent when prompted. Installation is project-local by default; add `-g` to install globally.

See the [setup guide](https://docs.rasu.ai/docs/setup/agent-skill) for a prompt to copy to your agent that does all of this in one paste.

## Use

Ask your agent, for example:

> Using Rasu AI, plan the opening sequence of my short film: a lighthouse keeper finds a message in a bottle.

In Claude Code, you can explicitly invoke the plugin skill with `/rasu:rasu`.

| Skill | Purpose |
|---|---|
| [rasu](skills/rasu/SKILL.md) | Find the right project, direct shots with Rasu AI's shot method, write screenplays, quote credits before generating, and recover from refusals |

## Source

Published from Rasu AI's platform repository, where the tests that check the Rasu AI MCP server also check that every tool this skill names still exists. Changes made here directly are overwritten on the next publish.

## License

[MIT](LICENSE)
