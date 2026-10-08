# pptb-agent-skills

Agent skills that teach Claude Code, GitHub Copilot, and OpenAI Codex how to build, fix, validate, publish, and review tools for [Power Platform ToolBox (PPTB)](https://github.com/PowerPlatformToolBox). Grounded directly in the official docs (docs.powerplatformtoolbox.com), marketplace policies, and the `generator-pptb` scaffolding tool — not generic Electron/web-app boilerplate.

A single `SKILL.md` per skill (Claude Skills, GitHub Copilot Agent Skills, and Codex all consume the same open SKILL.md standard — YAML frontmatter + Markdown body — so there's no need for agent-specific entry files), plus a `references/` folder per skill for docs that should only load on demand, and a root `AGENTS.md` for agents working in this repo itself. See [AGENTS.md](AGENTS.md) for the full conventions.

> **Status:** Not yet tested end-to-end against real build prompts. Recommend running it against a couple of realistic prompts (see [Try it](#try-it) below) before relying on it in a pipeline or generator. The agent-integration workflow (`invokeHeadless`) also describes a runtime that may be ahead of what a given installed `desktop-app` version actually ships — see [`tool-dev/SKILL.md`](tool-dev/SKILL.md) for how it's flagged inline.

## Try it

Available skills:

- **[pptb-tool-dev](tool-dev/SKILL.md)** — scaffold, fix, debug, validate, and publish PPTB tools, including API calls and inter-tool or agent integration.
- **[intake-policy-review](intake-policy-review/SKILL.md)** — review a PPTB tool or repository against current marketplace and AI-assisted-development policies, with evidence-based findings and prioritized pre-submission changes.
- **[tool-verification](tool-verification/SKILL.md)** — check a tool repository URL against every current Verified badge criterion, including dependencies, UI, maintenance, and marketplace usage; distinguish failures, missing evidence, and reviewer exceptions.

Once the skills are deployed (see [Deploying](#deploying)), just ask your agent naturally — the `description` in each `SKILL.md` is what triggers it. Example prompts and what the agent should do in response:

- _"Scaffold a PPTB tool that lets me bulk-update account records"_ → runs `yo pptb`, picks a framework, wires up `dataverseAPI` calls, writes a manifest that passes `pptb-validate`.
- _"My tool's `package.json` keeps failing `pptb-validate`, here's the file"_ → skips scaffolding, reads the actual file, fixes it against the schema in `tool-dev/references/manifest.md`.
- _"Make my tool launchable from another PPTB tool"_ → reads `tool-dev/references/invocation.md` and wires up `launchTool`/`getLaunchContext`/`returnData`, without re-scaffolding anything.
- _"Expose my tool to an agent via MCP"_ → reads `tool-dev/references/agent-integration.md` and adds the `agents` block + `invokeHeadless` entry point.
- _"Review this PPTB repository for marketplace intake readiness"_ → verifies the current policy pages, inspects the README, manifest, source, and testing evidence, then reports findings and a readiness verdict using `intake-policy-review`.
- _"Check https://github.com/my-org/my-tool against all PPTB verification requirements"_ → uses `tool-verification` to inspect the candidate release, source, issues, and available marketplace evidence, then reports every required and optional criterion with fixes and evidence gaps.

## Prerequisites

- **Node.js + npm** — the development workflow runs `yo`, `generator-pptb`, `@pptb/types` (which provides the `pptb-validate` CLI), and `npm publish`. None of these are installed by the skill itself; the agent installs them per-project as it works through the steps in `tool-dev/SKILL.md`.
- **PPTB desktop app** installed locally if you want to exercise the Step 5 debug loop (`dev-watch` → Load Local Tool) — not required just to scaffold or validate a tool.
- **Claude Code / Cowork**, **GitHub Copilot**, or **OpenAI Codex** with project or repo skills enabled, so the deployed `SKILL.md` actually gets picked up.
- **Policy and repository access** for `intake-policy-review` — the agent must be able to read the current public policy pages and the tool's source files. Node.js/npm is needed if it runs validation or dependency-audit commands; the PPTB desktop app is needed for real-environment testing.
- **Verification evidence access** for `tool-verification` — repository files, issue/comment history, releases, and current maturity/API docs. Node.js/npm supports audit and validation; PPTB supports runtime checks. Marketplace usage, ratings, and ownership may require dated owner-provided evidence. Missing access produces explicit evidence gaps rather than assumed passes.

## Deploying

**Claude** (Claude Code / Cowork project skills):

```sh
cp -r tool-dev/ <project>/.claude/skills/pptb-tool-dev/
cp -r intake-policy-review/ <project>/.claude/skills/intake-policy-review/
cp -r tool-verification/ <project>/.claude/skills/tool-verification/
```

**GitHub Copilot** (VS Code, Copilot CLI, Copilot cloud agent):

```sh
cp -r tool-dev/ <repo>/.github/skills/pptb-tool-dev/
cp -r intake-policy-review/ <repo>/.github/skills/intake-policy-review/
cp -r tool-verification/ <repo>/.github/skills/tool-verification/
```

**OpenAI Codex** (repository skills):

```sh
mkdir -p <repo>/.agents/skills
cp -r tool-dev/ <repo>/.agents/skills/pptb-tool-dev/
cp -r intake-policy-review/ <repo>/.agents/skills/intake-policy-review/
cp -r tool-verification/ <repo>/.agents/skills/tool-verification/
```

Copy whichever skills you need, including each skill's `references/` directory when present. Each skill uses the same folder and `SKILL.md` for all three agents, read directly from their respective skill locations. The copy commands above use POSIX shell syntax (for example, Bash or Git Bash).

## Using this in the tool generator

The generator can drop `tool-dev/references/manifest.md` and `tool-dev/references/build-and-csp.md` directly into a scaffolded project (e.g. as `.ai-context/` or similar) so an agent working _inside_ an already-generated tool has the manifest/CSP rules on hand without needing the full skill loaded. `SKILL.md` itself is meant for the _meta_ level — teaching an agent how to build a PPTB tool from scratch — not for bundling into every generated tool's output.

## Scope note

The `pptb-tool-dev` skill scaffolds via `generator-pptb` (`yo pptb`) only. `PowerPlatformToolBox/sample-tools` is intentionally not used as a scaffold source (outdated per maintainer). `Power-Maverick/PPTB-Tools` is referenced only as a pattern/convention example for real shipped tools, never as a template.

The `intake-policy-review` skill reviews existing tools against live policies and repository evidence. Its assessment does not replace marketplace maintainer approval or real-environment human testing.

The `tool-verification` skill targets the Verified maturity review after marketplace intake. It checks every required and optional criterion, but cannot grant verification or reviewer waivers. A repository URL starts the assessment; private marketplace data and live PPTB behavior may need additional evidence.

## Contents

```
AGENTS.md                     # conventions for agents working in this repo
intake-policy-review/
└── SKILL.md                   # marketplace and AI-assisted-development policy review
tool-verification/
├── SKILL.md                   # repository URL to Verified readiness assessment
└── references/
    ├── checklist.md           # evidence workflow for all maturity criteria
    └── reviewer-response.md   # reviewer decision guidance and response templates
tool-dev/
├── SKILL.md                   # shared Claude + Copilot + Codex entry point
└── references/
    ├── manifest.md             # package.json + pptb.config.json schema & validation rules
    ├── apis.md                 # toolboxAPI / dataverseAPI / powerplatformAPI quick reference
    ├── build-and-csp.md        # IIFE bundling gotcha, CSP exceptions, debugging setup
    ├── invocation.md           # inter-tool invocation (launchTool/getLaunchContext/returnData)
    ├── agent-integration.md    # MCP/agent exposure, invokeHeadless, execution modes
    └── debugging-publishing.md # dev-watch loop, pptb-validate CLI, npm publish, registry submission
```
