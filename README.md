# pptb-agent-skills

Agent skills that teach Claude Code and GitHub Copilot how to build, fix, validate, and publish tools for [Power Platform ToolBox (PPTB)](https://github.com/PowerPlatformToolBox). Grounded directly in the official docs (docs.powerplatformtoolbox.com) and the `generator-pptb` scaffolding tool — not generic Electron/web-app boilerplate.

A single `SKILL.md` per skill (Claude Skills and GitHub Copilot Agent Skills both consume the same open SKILL.md standard — YAML frontmatter + Markdown body — so there's no need for agent-specific entry files), plus a `references/` folder per skill for docs that should only load on demand, and a root `AGENTS.md` for agents working in this repo itself. See [AGENTS.md](AGENTS.md) for the full conventions.

> **Status:** Not yet tested end-to-end against real build prompts. Recommend running it against a couple of realistic prompts (see [Try it](#try-it) below) before relying on it in a pipeline or generator. The agent-integration workflow (`invokeHeadless`) also describes a runtime that may be ahead of what a given installed `desktop-app` version actually ships — see [`tool-dev/SKILL.md`](tool-dev/SKILL.md) for how it's flagged inline.

## Try it

Once the skill is deployed (see [Deploying](#deploying)), just ask your agent naturally — the `description` in `tool-dev/SKILL.md` is what triggers it. Example prompts and what the agent should do in response:

- *"Scaffold a PPTB tool that lets me bulk-update account records"* → runs `yo pptb`, picks a framework, wires up `dataverseAPI` calls, writes a manifest that passes `pptb-validate`.
- *"My tool's `package.json` keeps failing `pptb-validate`, here's the file"* → skips scaffolding, reads the actual file, fixes it against the schema in `tool-dev/references/manifest.md`.
- *"Make my tool launchable from another PPTB tool"* → reads `tool-dev/references/invocation.md` and wires up `launchTool`/`getLaunchContext`/`returnData`, without re-scaffolding anything.
- *"Expose my tool to an agent via MCP"* → reads `tool-dev/references/agent-integration.md` and adds the `agents` block + `invokeHeadless` entry point.

## Prerequisites

- **Node.js + npm** — the workflow runs `yo`, `generator-pptb`, `@pptb/types` (which provides the `pptb-validate` CLI), and `npm publish`. None of these are installed by the skill itself; the agent installs them per-project as it works through the steps in `tool-dev/SKILL.md`.
- **PPTB desktop app** installed locally if you want to exercise the Step 5 debug loop (`dev-watch` → Load Local Tool) — not required just to scaffold or validate a tool.
- **Claude Code / Cowork** or **GitHub Copilot** with project or repo skills enabled, so the deployed `SKILL.md` actually gets picked up.

## Deploying

**Claude** (Claude Code / Cowork project skills):
```
cp -r tool-dev/ <project>/.claude/skills/pptb-tool-dev/
```

**GitHub Copilot** (VS Code, Copilot CLI, Copilot cloud agent):
```
cp -r tool-dev/ <repo>/.github/skills/pptb-tool-dev/
```

Same folder, same `SKILL.md` — both agents read the same file directly from either location.

## Using this in the tool generator

The generator can drop `tool-dev/references/manifest.md` and `tool-dev/references/build-and-csp.md` directly into a scaffolded project (e.g. as `.ai-context/` or similar) so an agent working *inside* an already-generated tool has the manifest/CSP rules on hand without needing the full skill loaded. `SKILL.md` itself is meant for the *meta* level — teaching an agent how to build a PPTB tool from scratch — not for bundling into every generated tool's output.

## Scope note

This skill scaffolds via `generator-pptb` (`yo pptb`) only. `PowerPlatformToolBox/sample-tools` is intentionally not used as a scaffold source (outdated per maintainer). `Power-Maverick/PPTB-Tools` is referenced only as a pattern/convention example for real shipped tools, never as a template.

## Contents

```
AGENTS.md                     # conventions for agents working in this repo
tool-dev/
├── SKILL.md                   # shared Claude + Copilot entry point
└── references/
    ├── manifest.md             # package.json + pptb.config.json schema & validation rules
    ├── apis.md                 # toolboxAPI / dataverseAPI / powerplatformAPI quick reference
    ├── build-and-csp.md        # IIFE bundling gotcha, CSP exceptions, debugging setup
    ├── invocation.md           # inter-tool invocation (launchTool/getLaunchContext/returnData)
    ├── agent-integration.md    # MCP/agent exposure, invokeHeadless, execution modes
    └── debugging-publishing.md # dev-watch loop, pptb-validate CLI, npm publish, registry submission
```
