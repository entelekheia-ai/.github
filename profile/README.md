<h1 align="center">Entelékheia</h1>

<p align="center">
  <b>Intelligence, brought into form.</b><br>
  Revealing latent potential in people, systems, and technology.
</p>

<p align="center">
  <a href="https://entelekheia.ai">entelekheia.ai</a> ·
  <a href="#murici--a-desktop-chat-ui-for-running-agents">Murici</a> ·
  <a href="#cerrado--your-mailbox-drawn-as-a-landscape">Cerrado</a> ·
  <a href="#agent--an-open-standard-for-portable-agents">.agent</a> ·
  <a href="#vibe-ops--context-engineering-for-repositories">vibe-ops</a> ·
  <a href="#claude-code-plugins--small-tools-for-agentic-work">Plugins</a> ·
  <a href="#ref-id--one-identifier-for-anything-declared">ref-id</a> ·
  <a href="mailto:hello@entelekheia.ai">hello@entelekheia.ai</a>
</p>

---

Entelékheia develops foundational technologies, open protocols, and next-generation interfaces designed
for the era of native artificial intelligence.

The lab operates at the intersection of structural systems, cognitive computing, and intuitive
interfaces. This continuous practice guarantees that intelligent systems remain resilient, predictable,
and aligned with human intention.

---

## Murici — a desktop chat UI for running agents

<a href="https://github.com/entelekheia-ai/murici/releases"><img src="https://img.shields.io/github/v/release/entelekheia-ai/murici?label=release" alt="Latest release"></a>
<a href="https://github.com/entelekheia-ai/murici/blob/main/license"><img src="https://img.shields.io/badge/license-Apache--2.0%20%2B%20MIT-blue.svg" alt="Apache-2.0 and MIT"></a>
<a href="https://github.com/entelekheia-ai/murici"><img src="https://img.shields.io/badge/desktop-Electron-47848F?logo=electron&logoColor=white" alt="Electron desktop"></a>

<p align="center">
  <img src="https://github.com/entelekheia-ai/.github/raw/main/assets/murici.png" width="100%" alt="Murici running the Fridge Assistant agent: the conversation on the left, and on the right the agent's state history — responsive marked done, show_catalog in progress, and the remaining states still pending.">
</p>

A lightweight desktop app for running `.agent` behaviours — the portable agent format described below.
Drag a bundle onto the window and it compiles and starts. The panel on the right tracks
the run as it happens: which states are done, which one is executing, which are still ahead, alongside
the agent's own description and its execution graph. A conversation becomes something you watch rather
than infer.

Routing is deterministic. The model signals intent through a tool call, a WASM kernel decides the
transition, and the interface updates from the effects it returns. Chats, models and keys live in
IndexedDB on your own machine — there is no account and no server to sign into. Hosted providers and
locally discovered LLM servers connect on the same footing.

**[Download](https://github.com/entelekheia-ai/murici/releases/latest)** (macOS · Windows) ·
**[Source](https://github.com/entelekheia-ai/murici)** · open source

---

## Cerrado — your mailbox, drawn as a landscape

<a href="https://github.com/entelekheia-ai/gmail-addon/releases/latest"><img src="https://img.shields.io/github/v/release/entelekheia-ai/gmail-addon?sort=semver&label=release" alt="Latest release"></a>
<a href="https://github.com/entelekheia-ai/gmail-addon"><img src="https://img.shields.io/badge/Chromium-WebGPU-4285F4?logo=googlechrome&logoColor=white" alt="Chromium with WebGPU"></a>

<p align="center">
  <a href="https://daniloborg.es/experiment/mail-graph/"><img src="https://github.com/entelekheia-ai/.github/raw/main/assets/cerrado.png" width="100%" alt="The Cerrado landscape: correspondents as nodes, settled into territories of mail, each cluster a region of the mailbox."></a>
</p>

A mail client answers *what arrived*, and sorting by date is the only question it asks. It cannot show
that four people account for most of a decade of correspondence, or that a folder you think of as work is
three quarters receipts. Those are questions about the **shape** of a mailbox.

Cerrado draws that shape. A browser extension adds one item to Gmail's own navigation rail; select it and
the message list gives way to a WebGPU landscape — correspondents are nodes, conversations pull them
together, and the regions they settle into are the territories your mail actually has. Lenses cut the
same map several ways without moving anything, and a search paints its matches on the map instead of
filtering a list.

It reads only the page Gmail has already rendered in your tab, keeps what it read in that browser, and
makes no network request of any kind — no API key, no sign-in, no telemetry.

**[Try it without installing](https://daniloborg.es/experiment/mail-graph/)** (a generated mailbox) ·
**[Download](https://github.com/entelekheia-ai/gmail-addon/releases/latest)** (Chromium with WebGPU) ·
free for personal use

---

## .agent — an open standard for portable agents

<a href="https://www.npmjs.com/package/@dot-agent/cli"><img src="https://img.shields.io/npm/v/%40dot-agent%2Fcli?label=%40dot-agent%2Fcli" alt="npm @dot-agent/cli"></a>
<a href="https://open-vsx.org/extension/dot-agent/vscode-dot-agent"><img src="https://img.shields.io/open-vsx/v/dot-agent/vscode-dot-agent?label=.agent%20DSL" alt=".agent DSL editor extension"></a>
<a href="https://github.com/dot-agent-spec/platform/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="Apache-2.0"></a>
<a href="https://dot-agent.ai"><img src="https://img.shields.io/badge/spec-dot--agent.ai-1f6feb" alt="Specification"></a>

An agent today is a prompt inside somebody's codebase: it cannot be handed to another runtime, and the
only way to learn what it does is to talk to it and find out. `.agent` draws the line differently.
Everything a machine can guarantee — which states exist, which transitions are legal, which capabilities
may be called — is checked before a model is ever asked anything. Everything only a model can infer stays
in prose, where it belongs.

An agent is two files. A `.description` declares the contract — what it needs, what it can do, what it
returns. A `.behavior` declares the flow: a flat state machine where the prompt lives in `goal` and
`guide`, and the routing lives in `on intent`.

```
state responsive
  goal "Initiate the interaction and identify whether the user wants their text summarized or revised"
  guide "Greet the user and ask them to share the text along with whether they'd like a summary or a tone/clarity revision. Be concise and maintain a helpful tone."
  interact
  on intent "summarize" transition to summary
  on intent "revise" transition to revise
  on intent "end" transition to goodbye
  on offtopic transition to responsive

state summary
  goal "Generate and present the summary"
  guide "Analyze the provided text to generate a clear, accurate summary at the level of detail requested (executive, full, or bullet points). Present the result and invite the user to provide a new text or conclude the session."
  teach "knowledge/maintopics.md"
  interact
  on intent "new_text" transition to responsive
  on intent "end" transition to goodbye
  on offtopic transition to responsive
```

The toolchain packs those sources into a single portable `.agent` file, linting as it goes — so a broken
agent fails at authoring time instead of in front of a user.

```bash
npm install -g @dot-agent/cli
dot-agent init --name my-agent --domain example.com
dot-agent pack --dir . --out my-agent.agent
```

The grammar, parser and FSM kernel are Rust compiled to WASM, so the same behaviour runs in a browser, a
CLI or a desktop app without a second implementation to drift. The **.agent DSL** editor extension adds
highlighting, completion, go-to-definition and live diagnostics through a bundled language server —
on [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=dot-agent.vscode-dot-agent)
and [Open VSX](https://open-vsx.org/extension/dot-agent/vscode-dot-agent).

**[Specification](https://dot-agent.ai)** · **[Monorepo](https://github.com/dot-agent-spec/platform)**
· Apache-2.0, developed under independent governance

---

## vibe-ops — context engineering for repositories

<a href="https://github.com/entelekheia-ai/vibe-ops/actions/workflows/check.yml"><img src="https://github.com/entelekheia-ai/vibe-ops/actions/workflows/check.yml/badge.svg" alt="check"></a>
<a href="https://github.com/entelekheia-ai/vibe-ops/releases"><img src="https://img.shields.io/github/v/release/entelekheia-ai/vibe-ops?label=release" alt="Latest release"></a>
<a href="https://github.com/entelekheia-ai/vibe-ops/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="Apache-2.0"></a>

<p align="center">
  <img src="https://github.com/entelekheia-ai/.github/raw/main/assets/vibe-ops.png" width="820" alt="The vibe-ops slash commands listed in Claude Code: repo-setup, new-adr, new-rfc, new-plan, new-task, close-plan, close-task, authoring-agents-md, authoring-readme and license-setup.">
</p>

A repository's `AGENTS.md`, its rules and its skills are not documentation *about* the project — they are
the context an agent is handed before it does anything. This Claude Code plugin authors them, and governs
when each one loads: the map at session start, a rule scoped to the folder it governs, a skill only when
it is asked for.

Keeping that true is the harder half. An agent working through a task learns a great deal inside a single
run and forgets it at the end — the notes are deleted along with the work. So closing a task or a plan
here routes what the work taught back into those same files, and anything mechanically checkable becomes
a guard rather than a sentence, because a sentence is only followed by whoever read it. The repository
becomes the thing that remembers.

```bash
npm i -g @entelekheia/vibe-ops-cli          # the CLI: `vibe-ops` on PATH, and the gate

claude plugin marketplace add entelekheia-ai/public-plugin
claude plugin install vibe-ops@entelekheia  # the skills, agents and hooks
```

The plugin writes and the CLI checks: `vibe-ops check` is the commit gate, and the same checks are served
to the agent over MCP. Every skill reads the *target* repository's own templates and conventions, so one
installed plugin adapts to each repo instead of being copied into all of them and drifting apart.

**[Repository](https://github.com/entelekheia-ai/vibe-ops)** · Apache-2.0

---

## Claude Code plugins — small tools for agentic work

<a href="https://github.com/entelekheia-ai/public-plugin/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="Apache-2.0"></a>
<a href="https://github.com/entelekheia-ai/public-plugin"><img src="https://img.shields.io/badge/Claude%20Code-plugins-000?logo=anthropic&logoColor=white" alt="Claude Code plugins"></a>

The skills and subagents this lab runs its own work with, one plugin per job, each installable on its
own:

| Plugin | What it gives you |
|---|---|
| `delegation` | Decide where delegated work runs — the main loop, one subagent or a Workflow — and on which model and effort, from a routing table you tune to your own sessions; plus read-only `plan-scout`, `reviewer`, `fact-sheet` and `blind-run` agents, and an `implementer` |
| `method` | Carry a plan through its remaining tracks unattended, paced against the usage limit; route what a piece of work taught to the surface built for that kind of fact |
| `publishing` | Turn the skills and agents you wrote for your own setup into publishable ones — audit, generalize, blind-review, release |
| `release` | Stable and beta release channels for npm packages, and a first publish straight to CI through npm trusted publishing |
| `machine` | Keep a development machine working — Time Machine exclusions for regenerable caches, and small repairs after tool updates |
| `vibe-ops` | The repository governance described above |

```bash
claude plugin marketplace add entelekheia-ai/public-plugin
claude plugin install delegation@entelekheia

npx skills add entelekheia-ai/public-plugin   # skills only, for Codex, Cursor, OpenCode, Gemini CLI and others
```

**[Catalog](https://github.com/entelekheia-ai/public-plugin)** · Apache-2.0

---

## ref-id — one identifier for anything declared

<a href="https://www.npmjs.com/package/@entelekheia/ref-id"><img src="https://img.shields.io/npm/v/%40entelekheia%2Fref-id?label=npm" alt="npm @entelekheia/ref-id"></a>
<a href="https://crates.io/crates/ref-id"><img src="https://img.shields.io/crates/v/ref-id?label=crates.io" alt="crates.io ref-id"></a>
<a href="https://pypi.org/project/ref-id/"><img src="https://img.shields.io/pypi/v/ref-id?label=PyPI" alt="PyPI ref-id"></a>
<a href="https://github.com/entelekheia-ai/ref-id/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="Apache-2.0"></a>

A file path says *where* something is; a content hash says *what its bytes were*. Neither says *what it
is* for a thing that keeps living after an edit or a move — a rule a gate checks, a record in a
governance folder, an exported symbol. Each exists because a format declared it by name inside a scope,
and that declared name is the identity that survives a refactor.

`ref:` is a small scheme built on that rule. It wraps standards that already exist — Package URL, SWHID,
RFC 5147 — and adds only what they do not say together: which thing, in which state, over which
population. An identifier that carries a digest resolves to an envelope whose members recompute to it,
so the digest is a checkable claim rather than a dangling pointer.

```ts
import { parse } from "@entelekheia/ref-id"

parse("ref:pkg:npm/@acme/scanner-core@0.1.0#Observation")
// => { status: "ok", type: "pkg", locator: "npm/@acme/scanner-core@0.1.0", ... }
```

The specification ships as **data** — grammar, tables, digest rules and conformance vectors — so a port
is checked against vectors rather than re-derived from source. TypeScript, Rust, Swift and Python
implementations exist today, and a differential test runs all four over every input the specification
names, failing on the first disagreement.

**[Repository](https://github.com/entelekheia-ai/ref-id)** · **[Specification](https://github.com/entelekheia-ai/ref-id/tree/main/spec)** · Apache-2.0

---

<p align="center">
  <a href="https://entelekheia.ai">entelekheia.ai</a> ·
  <a href="mailto:hello@entelekheia.ai">hello@entelekheia.ai</a><br>
  <sub><i>ἐντελέχεια</i> — the realization of potential. Founded and directed by <a href="https://daniloborg.es">Danilo Borges</a>.</sub>
</p>
