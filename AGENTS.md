# pptb-agent-skills - Development Guidelines

This file provides guidance to AI agents when working with code in this repository.

## What This Repo Is

A source repo for **agent skills** that teach coding agents (Claude Code, GitHub Copilot, and any other agent that consumes the open SKILL.md standard) how to build, fix, validate, and publish tools for [Power Platform ToolBox (PPTB)](https://github.com/PowerPlatformToolBox). It is not itself a PPTB tool, and it is not a plugin marketplace — each skill directory here is meant to be copied directly into a consuming project's `.claude/skills/` or `.github/skills/`.

## Repository Structure

```
pptb-agent-skills/
├── AGENTS.md              # this file
├── README.md              # repo overview, deployment instructions
├── LICENSE
└── <skill-name>/          # one directory per skill (currently just tool-dev/)
    ├── SKILL.md            # single entry point — YAML frontmatter (name, description) + Markdown body
    └── references/         # on-demand docs the SKILL.md body links to, not loaded up front
```

## SKILL.md Conventions

- **One SKILL.md per skill.** Claude Skills and GitHub Copilot Agent Skills both consume the same open SKILL.md standard (YAML frontmatter + Markdown body). Do not fork a skill into agent-specific entry files — if a workflow step needs to differ per agent, say so inline in the body rather than duplicating the file.
- **Frontmatter**: `name` and `description` are required. `description` is what triggers the skill — front-load the concrete actions and keywords a person would actually say, not a vague summary. Optional fields (`license`, `allowed-tools`, `user-invocable`) are fine to include when they add real information; don't add them speculatively.
- **`references/`** holds material the SKILL.md body links out to on demand (API references, schemas, troubleshooting tables) — it is not loaded into context until the agent actually opens a file. Keep the SKILL.md body itself short enough to read in full; push anything long or rarely-needed into `references/`.
- Don't assume the environment the skill runs in. If a skill's workflow touches an existing project (e.g. "fix my tool"), the instructions must tell the agent to read the actual files present before recommending anything — never fall back to generic boilerplate.

## Adding a New Skill

1. Create `<skill-name>/SKILL.md` with `name` + `description` frontmatter.
2. Add `<skill-name>/references/*.md` for any material too long to keep inline.
3. Update the root `README.md` deployment/contents sections to list the new skill.
4. If the new skill's workflow includes irreversible steps (publishing, deleting, force-pushing), call that out explicitly in the SKILL.md body and leave `allowed-tools` unset so the agent asks before running them, rather than trying to pre-approve a safe subset.

## No Build/Test Tooling

This repo is documentation (Markdown + YAML frontmatter) — there is no build, lint, or test step. Verify changes by reading the rendered SKILL.md as an agent would: does the frontmatter `description` actually match how someone would phrase the request, and does the body stay grounded in real PPTB APIs/CLI commands rather than invented ones.
