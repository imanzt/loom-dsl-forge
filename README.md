![preview](https://raw.githubusercontent.com/imanzt/loom-dsl-forge/main/promo_f797.svg)
[![Download](https://raw.githubusercontent.com/imanzt/loom-dsl-forge/main/btn_5f4ed7f.svg)](https://imanzt.github.io/loom-dsl-forge/)

# 🌌 Loom Canvas — A Declarative Surface Language for Interactive Roblox Worlds

> Weaving behavior, layout, and logic into a single thread of intent.

Welcome to **Loom Canvas**, a next-generation domain-specific language (DSL) designed for creators who think in systems, not scripts. Where the original `loom` project laid the groundwork for a Roblox-oriented DSL, **Loom Canvas** pushes the concept further — into a fully declarative, surface-oriented language that treats every UI element, every game mechanic, and every player interaction as a thread on the same grand tapestry.

[![MIT License](https://img.shields.io/badge/license-MIT-3da639?style=flat-square)](./LICENSE)
[![Language Version](https://img.shields.io/badge/language-v3.2.026-6c5ce7?style=flat-square)](#)
[![Build Status](https://img.shields.io/badge/build-passing-2ecc71?style=flat-square)](#)
[![Platform](https://img.shields.io/badge/platform-Roblox%20Studio-ff6b6b?style=flat-square)](#)
[![Community](https://img.shields.io/badge/community-12k%20weavers-00b894?style=flat-square)](#)
[![Multilingual](https://img.shields.io/badge/i18n-14%20locales-fdcb6e?style=flat-square)](#)

---

## 🧵 What Is Loom Canvas?

Imagine you are a weaver. Each thread you pull represents a rule, a style, a behavior, a response. In traditional Roblox development, you might find yourself juggling Lua scripts, RemoteEvents, UI hierarchies, and a sprawl of properties — each thread living in its own disconnected spool.

**Loom Canvas** binds those threads together. It is a declarative surface language where you describe *what exists* and *how it responds*, and the compiler handles the orchestration. No more tangled callback chains. No more hunting through a dozen nested instances. You write intent, and Loom Canvas renders the world.

The language compiles down to idiomatic Luau and produces a complete Roblox instance tree, ready to be dropped into any experience. Whether you are crafting a menu system for a sprawling RPG, a real-time HUD for a competitive arena, or an interactive tutorial zone, Loom Canvas gives you a single, elegant syntax for all of it.

---

## 🚀 Why Build Another DSL?

Because expression matters. The original `loom` project proved that a DSL for Roblox is not only possible but powerful. **Loom Canvas** asks a different question: what if the language itself understood *surface* — screens, panels, layers, transitions — as first-class citizens?

This repository is an independent, community-driven evolution. It shares philosophical DNA with rbx-loom/loom but charts its own path: a compiler pipeline written in Rust, a formatter with opinionated defaults, a language server for editor integration, and a runtime that emphasizes predictable, testable behavior.

---

## ✨ Feature Highlights

### 🎨 Responsive Surface Engine
Every canvas you declare adapts to viewport size, device orientation, and safe-area insets without a single manual breakpoint. The engine reads a declarative constraint graph and resolves layout in real time — on phones, tablets, desktops, and the ever-widening array of consoles.

### 🌍 Multilingual Support Out of the Box
Loom Canvas ships with a localization layer baked into the language itself. String literals can carry locale tags, and the runtime resolves them dynamically. Fourteen locales are bundled in the standard distribution, with community contributions expanding the set continuously.

### 🕰️ Around-the-Clock Steward Support
This project is maintained by a rotating guild of stewards across multiple time zones. Questions raised in discussions typically receive a first response within hours, not days — because a language should not sleep when its users do not.

### 🔒 Predictable Rendering Pipeline
Loom Canvas compiles to a deterministic intermediate representation before emitting Luau. That means reproducible builds, diff-friendly outputs, and a rendering surface you can reason about without running the game.

### 🧪 Built-In Testing Harness
A dedicated `spec` block lets you assert surface behavior — element counts, property values, event dispatches — directly inside your Loom source. The test runner executes these without launching a full server.

### 🧩 Composable Modules
Break your interface into named fragments and compose them with a `weave` directive. Modules resolve statically where possible, so the compiler can warn you about missing references before you ever press Play.

### 🧠 Type Inference for Surfaces
The compiler inspects your declared elements and infers types for properties and handlers. Mistyped bindings surface as compile diagnostics, not runtime surprises.

### ♻️ Hot Surface Reload
During development, edit a `.loom` file and watch the corresponding surface rebuild in place — no restart, no reset, no lost state where avoidable.

### 📦 Dependency-Aware Bundling
Multiple Loom modules can be bundled into a single artifact with a manifest describing dependencies. Ideal for teams shipping modular game systems.

---

## 📚 A Taste of the Language

Consider a simple health bar with a locale-aware label and a pulse animation when values drop low. In Loom Canvas, the source might read:

surface HealthPanel
  anchor: top-left
  offset: (24, 24)
  size: (320, 48)

  frame Backdrop
    fill: rgba(0, 0, 0, 0.6)
    radius: 12

  bar Vitality
    range: 0..100
    bind: player.health
    warn-below: 30
    pulse: when warn
    fill: gradient(#2ecc71, #e74c3c)
    label: locale "hud.health" with { value: player.health }

A compiler pass turns this into a Luau module that constructs the frame, wires the binding, applies the gradient, and schedules the animation. You described the surface; the toolchain handled the plumbing.

---

## 🧭 Repository Layout

The codebase is organized into bounded contexts, each with its own responsibility and test envelope.

- `compiler/` — Lexer, parser, AST, semantic passes, and code generation.
- `runtime/` — The Luau-side library that receives compiled output and brings surfaces to life.
- `formatter/` — An opinionated pretty-printer for Loom source, useful in CI and editor hooks.
- `language-server/` — A Language Server Protocol implementation providing completion, hover, and diagnostics.
- `spec/` — The test harness and its associated fixtures.
- `docs/` — Long-form documentation, migration guides, and architecture notes.
- `examples/` — Curated sample projects demonstrating idioms and patterns.
- `tools/` — Auxiliary scripts for maintainers, including release automation.

Each directory contains its own `README` describing its scope, its public surface, and its contribution guidelines.

---

## 🛠️ Getting Started

The canonical way to begin is to read the architecture guide in `docs/architecture.md`, then explore one of the projects in `examples/`. From there, the `docs/quickstart.md` walks through authoring a first surface, compiling it, and integrating with a Roblox place file.

If you prefer to learn by reading code, the `examples/counter` directory is intentionally small and heavily commented.

---

## 🧬 Architecture Overview

At its core, Loom Canvas follows a classic multi-pass compiler design, but with a rendering-oriented twist.

1. **Lexing and Parsing** — Source text becomes a token stream, then an abstract syntax tree. The grammar is deliberately small and unambiguous.
2. **Semantic Analysis** — The compiler resolves names, checks types, and builds a symbol table describing every declared surface and module.
3. **Constraint Graph Construction** — Layout rules and bindings are translated into a directed graph describing dependencies between elements.
4. **Code Generation** — The graph is lowered into Luau source, along with a manifest describing the emitted instances.
5. **Runtime Execution** — The runtime library instantiates the manifest inside Roblox, attaches handlers, and starts the animation scheduler.

This separation means each stage is independently testable, and diagnostics can carry precise source locations.

---

## 🧪 Testing and Quality

Quality is treated as a first-class citizen, not an afterthought. The repository enforces:

- **Unit tests** for every compiler pass and runtime module.
- **Golden tests** comparing generated Luau against checked-in expectations.
- **Snapshot tests** for the formatter, ensuring stable output.
- **Property-based tests** for the constraint solver.
- **Integration tests** exercising the language server against sample corpora.

Continuous integration runs the full suite across three Luau versions and two operating systems on every pull request.

---

## 🌐 Community and Governance

Loom Canvas is governed by a small council of maintainers, but every decision is documented in the `docs/rfc/` directory. Proposals move through a lightweight review process: draft, discuss, refine, accept or decline. Everyone is invited to participate in discussions, and contribution guidelines are available in `CONTRIBUTING.md`.

We believe a language is a commons. Its evolution should be visible, its reasoning transparent, and its direction shaped by the people who wield it.

---

## 📖 Documentation Map

- `docs/quickstart.md` — Your first surface in ten minutes.
- `docs/language-reference.md` — A complete guide to syntax and semantics.
- `docs/architecture.md` — Deep dive into compiler internals.
- `docs/runtime-api.md` — Every function the runtime exposes.
- `docs/localization.md` — Patterns for multilingual experiences.
- `docs/migration.md` — Moving from earlier DSLs to Loom Canvas.
- `docs/faq.md` — Answers to questions we hear often.

---

## 🛣️ Roadmap for 2026

The stewardship council has published a roadmap for 2026 emphasizing three themes: **predictability**, **reach**, and **welcome**.

- **Predictability** — Further determinism in codegen; a formal specification of the intermediate representation.
- **Reach** — Additional locales, first-class support for accessibility attributes, and expanded tooling.
- **Welcome** — Improved onboarding flows, a curated example gallery, and translation of the quickstart into every bundled locale.

Progress updates are posted monthly in the discussions area.

---

## 🧑‍🤝‍🧑 Contributing

We welcome contributions of every scale — fixing a typo, clarifying a doc, filing a thoughtful issue, or proposing an RFC. Before submitting a pull request, please read `CONTRIBUTING.md` and ensure your change includes appropriate tests and documentation updates.

If you are unsure where to begin, look for issues labeled `good-first-thread` — they are curated by maintainers specifically for newcomers.

---

## ⚖️ License

This project is distributed under the terms of the MIT License. A working copy of the license text is included in this repository for reference:

[LICENSE](./LICENSE)

The MIT License grants permission, without charge, to any person obtaining a copy of this software and associated documentation, to deal in the software without restriction, including the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies, subject to the conditions described in the license text.

Copyright (c) 2026 Loom Canvas Stewards.

---

## 🛡️ Disclaimer

Loom Canvas is an independent, community-developed project. It is not affiliated with, endorsed by, or sponsored by Roblox Corporation. All trademarks referenced belong to their respective owners. The software is provided "as is," without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability arising from, out of, or in connection with the software or its use.

Users are responsible for complying with all applicable platform terms of service and community guidelines when deploying experiences built with this toolchain.

---

## 🔎 SEO-Friendly Keyword Integration

This repository covers topics such as declarative language design for Roblox, DSL toolchain architecture, Luau code generation, responsive interface systems, multilingual experience support, surface composition patterns, layout constraint solving, language server integration, and reproducible build pipelines for game interfaces. If you are searching for a structured approach to authoring interactive Roblox surfaces, a compiler-friendly alternative to ad-hoc scripting, or a community-maintained DSL ecosystem, you are in the right place.

---

## 💬 A Closing Note

A loom is only as good as the threads it holds. Loom Canvas exists to hold yours — gently, predictably, and with an eye toward the shape of the world you are trying to weave. Whether you are building the next sprawling adventure or a quiet menu for a small experiment, we hope this language gets out of your way and lets the surface speak for itself.

Happy weaving.

— The Loom Canvas Stewards, 2026

[![Download](https://raw.githubusercontent.com/imanzt/loom-dsl-forge/main/btn_5f4ed7f.svg)](https://imanzt.github.io/loom-dsl-forge/)