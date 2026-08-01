<h1 align="center">Entelékheia</h1>

<p align="center">
  <b>Intelligence, brought into form.</b><br>
  Revealing latent potential in people, systems, and technology.
</p>

<p align="center">
  <a href="https://entelekheia.ai">entelekheia.ai</a> ·
  <a href="#murici--a-desktop-chat-ui-for-running-agents">Murici</a> ·
  <a href="#agent--an-open-standard-for-portable-agents">.agent</a> ·
  <a href="#vibe-ops--repositories-that-start-organized">vibe-ops</a> ·
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

<p align="center">
  <a href="https://entelekheia.ai">entelekheia.ai</a> ·
  <a href="mailto:hello@entelekheia.ai">hello@entelekheia.ai</a><br>
  <sub><i>ἐντελέχεια</i> — the realization of potential. Founded and directed by <a href="https://daniloborg.es">Danilo Borges</a>.</sub>
</p>
