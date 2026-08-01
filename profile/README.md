<h1 align="center">Entelékheia</h1>

<p align="center">
  <b>Intelligence, brought into form.</b><br>
  An independent research lab building the open protocols, runtimes and interfaces
  for the era of native AI.
</p>

<p align="center">
  <a href="https://entelekheia.ai">entelekheia.ai</a> ·
  <a href="#agent--an-open-standard-for-portable-agents">.agent</a> ·
  <a href="#murici--a-chat-ui-that-runs-them">Murici</a> ·
  <a href="#vibe-ops--repositories-that-start-organized">vibe-ops</a> ·
  <a href="#research">Research</a> ·
  <a href="mailto:hello@entelekheia.ai">hello@entelekheia.ai</a>
</p>

---

Models got capable faster than the systems around them did. An agent today is a prompt in someone's
codebase: it cannot be handed to another runtime, its behaviour cannot be checked before it runs, and
the only way to find out what it does is to talk to it and see.

We think the missing piece is structure. Everything a machine can guarantee — which states exist, which
transitions are legal, which capabilities may be called — should be checked before a model is ever asked
anything. Everything only a model can infer should stay in prose, where it belongs. Draw that line
clearly and an agent stops being an artifact of one codebase and becomes a file you can pass around.

That conviction shows up in three layers of work: **open specifications** that nobody has to ask
permission to implement, **core runtimes** that execute them the same way twice, and **applied tools**
that put both in front of a person.

---

## .agent — an open standard for portable agents

<a href="https://www.npmjs.com/package/@dot-agent/cli"><img src="https://img.shields.io/npm/v/%40dot-agent%2Fcli?label=%40dot-agent%2Fcli" alt="npm @dot-agent/cli"></a>
<a href="https://github.com/dot-agent-spec/platform/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="Apache-2.0"></a>
<a href="https://dot-agent.ai"><img src="https://img.shields.io/badge/spec-dot--agent.ai-1f6feb" alt="Specification"></a>

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
CLI or a desktop app without a second implementation to drift.

**[Specification](https://dot-agent.ai)** · **[Monorepo](https://github.com/dot-agent-spec/platform)**
· Apache-2.0, developed under independent governance

---

## Murici — a chat UI that runs them

<a href="https://github.com/entelekheia-ai/murici/releases"><img src="https://img.shields.io/github/v/release/entelekheia-ai/murici?label=release" alt="Latest release"></a>
<a href="https://github.com/entelekheia-ai/murici/blob/main/license"><img src="https://img.shields.io/badge/license-Apache--2.0%20%2B%20MIT-blue.svg" alt="Apache-2.0 and MIT"></a>
<a href="https://github.com/entelekheia-ai/murici"><img src="https://img.shields.io/badge/desktop-Electron-47848F?logo=electron&logoColor=white" alt="Electron desktop"></a>

<!--
  SCREENSHOT SLOT — drop the capture at assets/murici.png in this repo, then uncomment.
  <p align="center">
    <img src="https://github.com/entelekheia-ai/.github/raw/main/assets/murici.png" width="100%" alt="Murici running an agent, with its state graph beside the conversation">
  </p>
-->

A lightweight desktop and web chat UI for running `.agent` behaviours. Drag a bundle onto the window and
it compiles and starts; the state graph beside the conversation shows which state you are in, which
states you have visited, and which transition just fired — so a run is something you watch rather than
infer.

Routing is deterministic: the model signals intent through a tool call, the WASM kernel decides the
transition, and the UI updates from the effects it returns. Chats, models and keys live in IndexedDB on
your own machine — there is no account and no server to sign into. It connects to hosted providers and
discovers local LLM servers on the same footing.

**[Download](https://github.com/entelekheia-ai/murici/releases/latest)** (macOS · Windows) ·
**[Source](https://github.com/entelekheia-ai/murici)** · open source

---

## vibe-ops — repositories that start organized

<a href="https://github.com/entelekheia-ai/vibe-ops/actions/workflows/check.yml"><img src="https://github.com/entelekheia-ai/vibe-ops/actions/workflows/check.yml/badge.svg" alt="check"></a>
<a href="https://github.com/entelekheia-ai/vibe-ops/releases"><img src="https://img.shields.io/github/v/release/entelekheia-ai/vibe-ops?label=release" alt="Latest release"></a>
<a href="https://github.com/entelekheia-ai/vibe-ops/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="Apache-2.0"></a>

A Claude Code plugin. One command lays down the governance, docs, licensing and agent configuration a
project needs; the remaining skills keep them true as the work moves — and every skill that touches a
file an agent reads at session start validates the repository afterwards.

```bash
claude plugin marketplace add entelekheia-ai/vibe-ops
claude plugin install vibe-ops@entelekheia
```

It is the tooling we built because our own repositories kept drifting from their own documentation. It
reads your repository and writes nothing into it, so there is no per-repo copy to maintain.

**[Repository](https://github.com/entelekheia-ai/vibe-ops)** · Apache-2.0

---

## Research

The lab works at the intersection of artificial intelligence, cognitive science and foundational systems
design. Three questions run underneath everything above:

- **Cognitive orchestration** — coordinating memory, reasoning and action across intelligent systems.
- **Agent ecosystems** — portable architectures for composing and evolving autonomous agents.
- **Distributed knowledge** — organizing collective intelligence across human networks and AI nodes.

---

## Principles

**Clarity before complexity.** The most powerful technical ideas have an inherent simplicity; maturity
shows up as architecture someone else can read.

**Systems outlast tools.** We build structures meant to still be standing when the current generation of
tooling is gone.

**Intelligence requires structure.** Raw capability becomes coherent behaviour only inside an
architecture that constrains it.

**Technology should amplify thought.** Software that merges into the work, rather than demanding
attention for itself.

**Long-term vision over trend cycles.** Technical conviction over the hype window.

---

<p align="center">
  <a href="https://entelekheia.ai">entelekheia.ai</a> ·
  <a href="mailto:hello@entelekheia.ai">hello@entelekheia.ai</a><br>
  <sub><i>ἐντελέχεια</i> — the realization of potential. Founded and directed by <a href="https://daniloborg.es">Danilo Borges</a>.</sub>
</p>
