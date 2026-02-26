# Spec-Driven AI Development: Knowledge Base as Single Source of Truth

**Research Report — February 2026**

---

## Executive Summary

The pattern of using a **knowledge base / spec repo as the single source of truth for AI-assisted development** has emerged as a recognized methodology now formally called **Spec-Driven Development (SDD)**. This is not a fringe idea — it is being championed by GitHub (spec-kit), Anthropic (CLAUDE.md / Agent Skills), Amazon/AWS (Kiro IDE), Thoughtworks (Technology Radar), Martin Fowler's site, and multiple open-source frameworks. The research below covers the full landscape.

---

## 1. Teams Using a Separate "Design KB" / "Spec Repo" That AI Agents Reference

### The Pattern Is Real and Has a Name

Spec-Driven Development (SDD) is now a recognized methodology where **specifications become executable, first-class artifacts** that directly generate working implementations. Multiple teams and tools support the exact pattern of a separate spec layer that AI agents reference during implementation.

### Key Tools Enabling This Pattern

| Tool | Description | URL |
|------|-------------|-----|
| **GitHub spec-kit** | Open-source CLI + templates for SDD. Creates structured spec workspace that works with Copilot, Claude Code, Cursor, Gemini CLI | [github.com/github/spec-kit](https://github.com/github/spec-kit) |
| **Tessl** | Agent enablement platform — "write context once, Tessl handles distributing it across every agent, every repo, every tool" | [tessl.io](https://tessl.io/) |
| **BMAD Method** | 21 specialized AI agents, 50+ workflows. Full agile team simulation with specs as source of truth | [github.com/bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) |
| **cc-sdd** | Kiro-style commands for Claude Code enforcing requirements -> design -> tasks -> implementation workflow | [github.com/gotalab/cc-sdd](https://github.com/gotalab/cc-sdd) |
| **Kiro IDE** | AWS/Amazon's AI IDE (VS Code fork) with spec-driven development built in as the core workflow | [kiro.dev](https://kiro.dev/blog/kiro-and-the-future-of-software-development/) |

### How spec-kit Works (The Closest Match to Your Pattern)

GitHub's spec-kit formalizes this exact flow:
1. **`constitution.md`** — persistent centralized intent layer for team standards (like giving a senior hire day-one guidance)
2. **Spec files** — structured requirements that become contracts for AI agent behavior
3. **Slash commands** — `/spec`, `/plan`, `/task`, `/implement` guide the agent through the workflow
4. Specs live in a `.spec/` directory and act as the shared source of truth across sessions and developers

> "Instead of coding first and writing docs later, you start with a specification that becomes a contract for how code should behave and serves as the source of truth your tools and AI agents use to generate, test, and validate code."
> — [GitHub Blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)

---

## 2. CLAUDE.md / Cursor Rules / Agent Skills Workflows

### CLAUDE.md Best Practices

CLAUDE.md is a rules file that shapes every decision Claude Code makes during a session. Key findings:

- **Keep it under 300 lines** — shorter is better. Claude ignores CLAUDE.md content it deems irrelevant to the current task.
- **Use Progressive Disclosure** — keep task-specific instructions in separate markdown files. Tell Claude *how to find* information rather than putting everything in CLAUDE.md.
- **Research shows impact**: AGENTS.md files (the cross-tool equivalent) were associated with a **29% reduction in median runtime** and **17% reduction in output token consumption** in projects that use them.

**Source**: [Writing a good CLAUDE.md — HumanLayer Blog](https://www.humanlayer.dev/blog/writing-a-good-claude-md)

### Agent Skills (Anthropic's Official Approach)

Agent Skills are the evolution beyond CLAUDE.md — organized folders of instructions, scripts, and resources that agents discover and load dynamically:

- Skills are defined in `SKILL.md` files with YAML frontmatter
- They load **on-demand** when relevant (not always in context)
- Published as an **open standard** for cross-platform portability (December 2025)
- Five essential skill categories: build-test-verify, git-commit, create-pull-request, core-conventions, self-review-checklist

**Sources**:
- [Anthropic — Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- [Claude Code Skills Docs](https://code.claude.com/docs/en/skills)
- [github.com/anthropics/skills](https://github.com/anthropics/skills)
- [Implementing CLAUDE.md and Agent Skills — Matthew Groff](https://www.groff.dev/blog/implementing-claude-md-agent-skills)

### .cursorrules Pattern

`.cursorrules` files serve a similar purpose for Cursor IDE — encoding team coding standards so the AI automatically adheres to them. A large collection of community examples exists:

- **awesome-cursorrules**: [github.com/PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)
- **dotcursorrules.com**: Community hub for sharing rules — [dotcursorrules.com](https://dotcursorrules.com)
- **Agentic cursorrules**: Multi-agent partitioning through file-tree boundaries — [github.com/s-smits/agentic-cursorrules](https://github.com/s-smits/agentic-cursorrules)

### Codified Context Research (Academic)

A February 2026 paper, **"Codified Context: Infrastructure for AI Agents in a Complex Codebase"**, describes a three-component system built during construction of a 108,000-line C# distributed system:

1. **Hot-memory constitution** — conventions, retrieval hooks, orchestration protocols
2. **19 specialized domain-expert agents**
3. **Cold-memory knowledge base** — 34 on-demand specification documents

The paper explicitly argues that **single-file manifests (CLAUDE.md, AGENTS.md) do not scale beyond modest codebases** — a 1,000-line prototype can be fully described in a single prompt, but a 100,000-line system cannot.

**Source**: [arxiv.org/html/2602.20478](https://arxiv.org/html/2602.20478)

Additional relevant papers:
- [Agent READMEs: An Empirical Study of Context Files](https://arxiv.org/html/2511.12884v1)
- [On the Impact of AGENTS.md Files](https://arxiv.org/html/2601.20404v1)
- [Evaluating AGENTS.md: Are Repository-Level Context Files Helpful?](https://arxiv.org/abs/2602.11988)

---

## 3. TDD with AI Agents

### The Agentic Coding Handbook (Tweag)

Tweag's **Agentic Coding Handbook** is the most comprehensive open-source guide on TDD with AI agents:

- **TDD Workflow**: TDD gives structure; agentic coding gives speed. The combination shines with complex logic.
- **Spec-First Approach**: Start by collaborating with AI to generate a detailed specification before writing code. Create `plan.md` and `todo.md` as shared understanding documents.
- Teams using this approach delivered projects **45% faster** with high code quality.

**Sources**:
- [TDD Workflow — Agentic Coding Handbook](https://tweag.github.io/agentic-coding-handbook/WORKFLOW_TDD/)
- [Spec-First Approach — Agentic Coding Handbook](https://tweag.github.io/agentic-coding-handbook/WORKFLOW_SPEC_FIRST_APPROACH/)
- [github.com/tweag/agentic-coding-handbook](https://github.com/tweag/agentic-coding-handbook)

### Addy Osmani's Spec-Writing Guide

Google Chrome engineer Addy Osmani published a definitive guide on writing specs for AI agents:

1. **Keep it goal-oriented** — focus on *what* and *why*, not *how*
2. **Structure like a PRD** — clear sections, not loose notes
3. **Break tasks into smaller pieces** — plan first in read-only mode
4. **Encourage self-verification** — instruct AI to compare results against spec
5. **Leverage testing in the spec** — embed test plans or actual tests
6. **Conformance testing** — language-independent tests (YAML-based) as contracts

**Source**: [AddyOsmani.com — How to write a good spec for AI agents](https://addyosmani.com/blog/good-spec/)

### Eric Elliott's AI-Driven Development + TDD

Eric Elliott's framework combines TDD with AI agents through four phases:

1. **`/discover`** — user story mapping
2. **`/task`** — plan implementation for user stories
3. **`/execute`** — follow TDD process (red-green-refactor) guided by patterns and best practices
4. **`/review`** — quality assurance review

> "The fast feedback loops, clear requirements, and deterministic behavior that TDD provides are exactly what AI agents need to be effective."

**Source**: [Better AI Driven Development with TDD — Eric Elliott](https://medium.com/effortless-programming/better-ai-driven-development-with-test-driven-development-d4849f67e339)

### Practical TDD with Claude Code

A practical example of forcing Claude Code into TDD mode:

**Source**: [Forcing Claude Code to TDD: An Agentic Red-Green-Refactor Loop](https://alexop.dev/posts/custom-tdd-workflow-claude-code-vue/)

### Test-Driven Generation (TDG)

Chanwit Kaewkasi coined "Test-Driven Generation" — writing tests first, then having AI generate the implementation to pass them:

**Source**: [Test-Driven Generation (TDG) — Medium](https://chanwit.medium.com/test-driven-generation-tdg-adopting-tdd-again-this-time-with-gen-ai-27f986bed6f8)

---

## 4. VitePress / Static Site Generators as Internal Design Wikis

### VitePress for Internal Documentation

VitePress is a Vue-powered static site generator built on Vite. Key characteristics that make it suitable for internal spec wikis:

- **Markdown-first** — all content is `.md` files, perfect for version control alongside specs
- **Fast hot-reload** — instant preview of spec changes during editing
- **Customizable themes** — standard Vite + Vue DX for custom layouts
- **Powers documentation for**: Vue.js, Vite, Rollup, Pinia, VueUse, Vitest, D3, UnoCSS, Iconify

While no specific "internal design wiki" case studies were found, VitePress is widely used by open-source projects as their documentation platform and the pattern of serving `.md` spec files through VitePress is a natural fit.

**Sources**:
- [VitePress — Official Site](https://vitepress.dev/)
- [How to Build a Modern Documentation Site with VitePress — freeCodeCamp](https://www.freecodecamp.org/news/how-to-build-a-modern-documentation-site-with-vitepress/)

### Your VitePress + Spec Repo Pattern

Your approach of using VitePress to serve specs, mockups, and design docs from a knowledge base repo is well-aligned with the SDD methodology. The `.md` files that VitePress serves can simultaneously:
1. Be human-readable documentation (via VitePress site)
2. Be machine-readable specs (referenced by AI agents via CLAUDE.md / SKILL.md)
3. Be version-controlled alongside HTML mockups

---

## 5. HTML Mockups as Specs

### The Emerging Pattern

The use of HTML prototypes (rather than Figma) as source of truth is gaining traction in AI-assisted workflows for a specific reason: **AI agents can read and understand HTML directly, but cannot interpret Figma files natively.**

Key findings:

- **Intent Prototyping**: Smashing Magazine published on building from stable, unambiguous specifications to prevent "design debt" from ambiguous handoffs. Documented design intent becomes a durable source of truth.
  - **Source**: [Intent Prototyping — Smashing Magazine](https://www.smashingmagazine.com/2025/10/intent-prototyping-practical-guide-building-clarity/)

- **AI Design-to-Code**: Multiple tools (Teleport, v0, etc.) can convert visual designs to HTML/CSS, but the reverse flow — **using HTML as the spec that AI implements in a framework** — is the pattern you are exploring. This is less documented but logically sound: HTML mockups are unambiguous, parseable, and can be served alongside specs.

- **Spec as executable artifact**: The SDD movement treats specs as machine-readable contracts. HTML mockups fit this definition perfectly — they are the spec *and* the reference implementation of UI.

### Recommendation

Storing HTML mockups in your KB repo alongside markdown specs and referencing them from CLAUDE.md/SKILL.md files creates a powerful feedback loop: the AI agent can `Read` the HTML mockup, understand the intended layout/structure, and implement it in the target framework (Vue, React, etc.).

---

## 6. Mono-repo vs Multi-repo with Shared Spec

### Multi-Repo Sync Tools

| Tool | What It Does | URL |
|------|-------------|-----|
| **knowhub** | Central knowledge hub — declare shared files per repo, knowhub fetches from a common source. Run `npx knowhub` to sync. | [github.com/yujiosaka/knowhub](https://github.com/yujiosaka/knowhub) |
| **AI Rules Sync (AIS)** | Sync AI agent rules across projects. Supports Cursor, Copilot, Claude Code, universal AGENTS.md | [github.com/lbb00/ai-rules-sync](https://github.com/lbb00/ai-rules-sync) |
| **Tessl** | Enterprise platform — write context once, distribute across agents/repos/tools without duplication or drift | [tessl.io](https://tessl.io/) |
| **Git Submodules** | Nest spec repo inside app repos for unified filesystem access | Built into Git |

### Multi-Repo Patterns

1. **Shared Configuration Sync (knowhub)**: Each repo declares which files should stay updated from a common source. Best for CLAUDE.md, .cursorrules, coding guidelines.

2. **Git Submodules**: Integrate the spec repo as a submodule in each app repo. Creates a local "monorepo" view. All spec files accessible via unified filesystem — AI agents can read them directly.

3. **Synthetic Monorepos (Nx)**: Connect separate repositories into a unified dependency graph without moving code. Makes cross-repo relationships visible.

4. **Nested AGENTS.md**: In monorepos, AGENTS.md files can be nested per package. Agents read the nearest file in the directory tree, with closest taking precedence.

### Monorepo Advantages for AI Agents

> "AI agents can perform atomic cross-project changes as single pull requests with full testing and review. Monorepos limit unnecessary manual coordination by giving AI agents full codebase awareness."
> — [monorepo.tools/ai](https://monorepo.tools/ai)

**Key insight from research**: It is not about providing as much context as possible, but providing the *right* context. Higher-level architectural understanding matters more than raw code access.

**Sources**:
- [Steering AI Agents in Monorepos with AGENTS.md — DEV](https://dev.to/datadog-frontend-dev/steering-ai-agents-in-monorepos-with-agentsmd-13g0)
- [In a multi-repo setup, providing context from other repos](https://elite-ai-assisted-coding.dev/p/context-from-internal-git-repos)
- [Introducing knowhub — Medium](https://medium.com/@yujiisobe/introducing-knowhub-share-ai-assistant-rules-across-repos-17fb6b09c114)
- [RepoSwarm: Give AI Agents Context Across Repos](https://robotpaper.ai/reposwarm-give-ai-agents-context-across-all-your-repos/)
- [Nx and AI — Why They Work Together](https://nx.dev/blog/nx-and-ai-why-they-work-together)

---

## 7. Blog Posts, Repos, and Talks About "Spec-Driven AI Development"

### Essential Reading

| Resource | Author/Org | URL |
|----------|-----------|-----|
| Spec-Driven Development with AI (official toolkit launch) | GitHub | [github.blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/) |
| How spec-driven development improves AI coding quality | Red Hat | [developers.redhat.com](https://developers.redhat.com/articles/2025/10/22/how-spec-driven-development-improves-ai-coding-quality) |
| Understanding SDD: Kiro, spec-kit, and Tessl | Martin Fowler | [martinfowler.com](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html) |
| Spec-Driven Development (Thoughtworks) | Thoughtworks | [thoughtworks.medium.com](https://thoughtworks.medium.com/spec-driven-development-d85995a81387) |
| Spec-Driven Development: When Architecture Becomes Executable | InfoQ | [infoq.com](https://www.infoq.com/articles/spec-driven-development/) |
| How to write a good spec for AI agents | Addy Osmani | [addyosmani.com](https://addyosmani.com/blog/good-spec/) |
| My LLM coding workflow going into 2026 | Addy Osmani | [addyosmani.com](https://addyosmani.com/blog/ai-coding-workflow/) |
| Diving Into SDD With GitHub Spec Kit | Microsoft | [developer.microsoft.com](https://developer.microsoft.com/blog/spec-driven-development-spec-kit) |
| How to Use a Spec-Driven Approach for Coding with AI | JetBrains | [blog.jetbrains.com](https://blog.jetbrains.com/junie/2025/10/how-to-use-a-spec-driven-approach-for-coding-with-ai/) |
| Lesson 12: Spec Driven Development | Agentic Coding | [agenticoding.ai](https://agenticoding.ai/docs/practical-techniques/lesson-12-spec-driven-development) |
| SDD Explained | Augment Code | [augmentcode.com](https://www.augmentcode.com/guides/spec-driven-development-ai-agents-explained) |

### Key GitHub Repositories

| Repo | Stars / Description | URL |
|------|-------------------|-----|
| **github/spec-kit** | GitHub's official SDD toolkit | [github.com/github/spec-kit](https://github.com/github/spec-kit) |
| **anthropics/skills** | Anthropic's official Agent Skills repo | [github.com/anthropics/skills](https://github.com/anthropics/skills) |
| **bmad-code-org/BMAD-METHOD** | Most complete SDD framework — 21 agents, 50+ workflows | [github.com/bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) |
| **gotalab/cc-sdd** | Kiro-style SDD for Claude Code and 7 other agents | [github.com/gotalab/cc-sdd](https://github.com/gotalab/cc-sdd) |
| **tweag/agentic-coding-handbook** | Comprehensive TDD + spec-first guide | [github.com/tweag/agentic-coding-handbook](https://github.com/tweag/agentic-coding-handbook) |
| **affaan-m/everything-claude-code** | Battle-tested Claude Code configs (hackathon winner) | [github.com/affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) |
| **yujiosaka/knowhub** | Sync AI rules across repos | [github.com/yujiosaka/knowhub](https://github.com/yujiosaka/knowhub) |
| **lbb00/ai-rules-sync** | Multi-agent rules sync | [github.com/lbb00/ai-rules-sync](https://github.com/lbb00/ai-rules-sync) |
| **PatrickJS/awesome-cursorrules** | Community .cursorrules collection | [github.com/PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) |
| **VoltAgent/awesome-claude-code-subagents** | 100+ specialized Claude Code subagents | [github.com/VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) |
| **wshobson/agents** | Multi-agent orchestration for Claude Code | [github.com/wshobson/agents](https://github.com/wshobson/agents) |

### Hacker News Discussions

- [Show HN: Spec-Driven AI Development — Keep AI-Generated Code Maintainable](https://news.ycombinator.com/item?id=46589002)
- [Toolkit to help you get started with Spec-Driven Development (spec-kit)](https://news.ycombinator.com/item?id=45798473)
- [Spec-Driven Development: The Waterfall Strikes Back](https://news.ycombinator.com/item?id=45935763)
- [Understanding SDD: Kiro, Spec-Kit, and Tessl](https://news.ycombinator.com/item?id=45610996)

### Dedicated Websites

- [specdriven.ai](https://specdriven.ai/) — Methodology overview and resources
- [agenticoding.ai](https://agenticoding.ai/) — Course/docs including SDD lessons
- [tddday.com](https://tddday.com/) — Eric Elliott's TDD + AI resources

---

## 8. Comparison of SDD Frameworks

| Feature | spec-kit (GitHub) | BMAD Method | cc-sdd | Kiro | Tessl |
|---------|----------|-------------|--------|------|-------|
| Open source | Yes | Yes | Yes | No (preview) | No (platform) |
| Multi-agent support | Copilot, Claude, Cursor, Gemini | Claude Code, Cursor | 8 agents | Built-in | Agent-agnostic |
| Spec format | Markdown + YAML | Markdown + agents | Markdown | Built-in UI | Platform-managed |
| Requirements -> Design -> Tasks | Yes | Yes (21 agents) | Yes (Kiro-style) | Yes (native) | Yes |
| Multi-repo sync | No (single repo) | No | No | No | Yes (core feature) |
| Test integration | Via spec | Via agents | Via validation | Built-in | Via evals |
| Constitution/rules file | constitution.md | Multiple agent configs | Steering files | Built-in | Platform config |

---

## 9. Recommendations for Your Workflow

Based on this research, your pattern of a **separate KB repo with VitePress + HTML mockups + specs that AI agents reference** is well-aligned with the SDD movement. Here are concrete recommendations:

### Immediate Actions

1. **Add a `constitution.md`** (spec-kit term) or equivalent to your KB repo — this is your team's coding standards, architectural decisions, and conventions that all AI agents should follow.

2. **Structure specs with YAML frontmatter** — makes them parseable by both VitePress and AI agents. Include acceptance criteria, test cases, and references to HTML mockups.

3. **Create SKILL.md files** in your KB repo for domain-specific guidance (e.g., "implementing-from-mockup" skill that tells Claude how to read an HTML mockup and translate it to Vue components).

4. **Use git submodules or knowhub** to sync KB content into app repos so AI agents can access specs without leaving their working context.

### Multi-Repo Sync Strategy

For your pattern (1 spec KB repo + N app repos):
- **Option A**: Git submodule — mount KB repo inside each app repo at `./specs/` or `./kb/`
- **Option B**: knowhub — declare which spec files each app repo needs, auto-sync on demand
- **Option C**: CLAUDE.md in each app repo points to the KB repo URL with instructions to clone/reference it

### Spec File Structure (Suggested)

```
kb-repo/
  .vitepress/          # VitePress config
  constitution.md      # Team-wide AI agent rules
  specs/
    feature-a/
      requirements.md  # User stories + acceptance criteria
      design.md        # Technical design decisions
      tasks.md         # Implementation tasks
      mockup.html      # HTML prototype (source of truth for UI)
      tests.yaml       # Conformance tests
  skills/
    implement-from-mockup/
      SKILL.md         # How to translate mockups to components
    tdd-workflow/
      SKILL.md         # Red-green-refactor process
```

---

*Report compiled from web research conducted February 2026. All URLs verified at time of research.*
