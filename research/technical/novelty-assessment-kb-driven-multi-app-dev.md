---
title: "Novelty Assessment: KB-Driven Multi-App Development"
date: 2026-02-26
status: draft
tags:
  - novelty
  - sdd
  - research
  - methodology
---

# Novelty Assessment: KB-Driven Multi-App Development

## Executive Summary

After thorough searching across academic papers (arXiv), GitHub repositories, industry blogs, and tooling from major vendors (GitHub, Anthropic, AWS/Kiro, Augment Code, Oracle), **the individual components of this approach all have precedent, but the specific combination — a standalone KB repo as a multi-app coordination layer with a business-domain knowledge graph for AI grounding, triple-duty tests, cross-app event contracts, and planner/what-if agents — does not appear to exist as a unified, articulated methodology anywhere.**

---

## Component 1: KB/Spec Repo as Multi-App Coordination Layer

### What Exists

| Tool | What It Does | Gap |
|---|---|---|
| **GitHub spec-kit** | Spec-driven dev with `.specify/` folders | Single-repo, not multi-app coordination |
| **Augment Code Intent** | Multi-repo coordination via Coordinator agent | Runtime orchestration, not persistent KB |
| **AWS Kiro** | Steering files in `.kiro/steering/` | Per-repo, not separate coordination layer |
| **Codified Context paper** (arXiv:2602.20478) | 34 on-demand spec docs for 108K-line C# system | Single codebase, not multi-app |
| **Claude Swarm** | MCP-connected agents across repos | Agent coordination, not persistent spec KB |

### What Does NOT Exist

A **standalone, separate repository** that is pure specifications, domain knowledge, and tests — serving as the authoritative coordination layer for multiple independent applications. No tool or paper describes this pattern where the KB repo is the primary artifact and code repos are downstream consumers.

---

## Component 2: Domain Knowledge Graph for AI Agent Grounding

### What Exists

| Tool/Paper | What It Does | Gap |
|---|---|---|
| **Stardog, Neo4j, ZBrain** | Enterprise KGs for AI agent grounding | For data/business queries, not code implementation |
| **Potpie** | Parses repos into code-level KG (Neo4j) | Code-level graph, not business domain |
| **KGACG** (arXiv:2510.19868) | Knowledge-guided multi-agent code gen | Architectural knowledge, not business domain |
| **GENIUS** (arXiv:2511.01348) | Architectural reasoning + domain knowledge for code gen | Closest academically |

### What Does NOT Exist

A **business-domain knowledge graph** (e.g., "franchise application," "survey assessment," "regional head permit") used specifically to help AI coding agents make better **implementation decisions** — not for querying or retrieval, but for grounding the agent's understanding of the problem domain during code generation.

---

## Component 3: Triple-Duty Tests

### What Exists

| Tool | What It Does | Gap |
|---|---|---|
| **Pact, Specmatic, PactFlow** | Cross-service contract testing | No AI planner invariant role |
| **AsyncAPI + Specmatic** | Pub/sub event contract testing | Separate from TDD concerns |
| **Augment Code Verifier** | Checks implementation against specs | Single purpose, not triple-duty |

### What Does NOT Exist

The specific concept of **the same test suite serving three distinct purposes simultaneously**:
1. Traditional implementation validation (TDD)
2. Planner agent invariants (AI reads tests before proposing changes)
3. Cross-app event contract verification

Existing tools treat these as separate concerns with separate test suites.

---

## Component 4: Planner/What-If Agents Against Structured Specs

### What Exists

| Tool | What It Does | Gap |
|---|---|---|
| **LangGraph, ReWOO, MetaGPT** | Plan-and-execute agents | Don't read from separate KB, no cross-app what-if |
| **Augment Code Coordinator** | Analyzes specs, breaks into tasks | Runtime tool, not what-if against persistent KB |

### What Does NOT Exist

A planner agent that:
- Reads structured specs + tests from a **separate KB repo** (not from code)
- **Traces impact across multiple independent application boundaries** via event contracts
- Performs **what-if analysis** without touching any code
- Uses a domain knowledge graph to understand business implications of proposed changes

---

## Component 5: The Full Combination

### Partial Combinations That Exist

| Combination | Closest Match | Gap |
|---|---|---|
| Spec repo + AI agents | GitHub Spec Kit, Kiro | Single-repo, not multi-app |
| Multi-repo + AI coordination | Augment Code Intent, Claude Swarm | Runtime, not persistent KB |
| Knowledge graph + AI coding | Potpie, KGACG | Code-level, not business domain |
| Contract testing + event schemas | Pact + AsyncAPI | No AI agent invariant role |
| Planner agents + specs | ReWOO, MetaGPT | Don't read from separate KB |
| Codified context + distributed system | arXiv:2602.20478 | Single codebase, not multi-app |

### What Definitively Does NOT Exist

The **full synthesis**:

1. A **standalone KB repo** (separate from all app repos) as the coordination layer
2. Containing a **business-domain knowledge graph** for AI agent grounding
3. With **tests that simultaneously** validate implementations, serve as AI planner invariants, and verify cross-app event contracts
4. Enabling **planner/what-if agents** that reason about system-wide changes by reading only the KB
5. With **graph visualization** for non-technical stakeholder review
6. All governing **multiple independent applications** communicating via pub/sub events

No paper, tool, blog post, or GitHub repository describes this complete architecture.

---

## Where the Novelty Lies

The novelty is not in any single component but in three key architectural decisions:

### 1. KB Repo as Primary Artifact (Architectural Inversion)

Everyone else puts specs in the code repo. This approach inverts it: the KB repo is the source of truth, and code repos are downstream consumers. The KB is not documentation — it's the system's coordination layer.

### 2. Business-Domain Knowledge Graph for AI Coding Grounding

Knowledge graphs for AI agents exist for data querying (Stardog, ZBrain). Knowledge graphs for code exist for repository understanding (Potpie). Using a **business-domain** knowledge graph to help AI agents make better **code implementation** decisions — bridging the business-to-code gap — is a novel application.

### 3. Triple-Duty Tests as Unification Mechanism

The idea that the same test artifact simultaneously serves TDD, AI planner invariants, and cross-app contract verification is novel. Existing approaches treat these as separate testing concerns with separate tooling. Unifying them in the KB creates a single verification layer that all agents (implementers, planners, reviewers) use.

---

## The Combination Creates Emergent Properties

The combination produces something more than the sum of its parts:

- A non-technical stakeholder can review a domain graph
- A planner agent can reason about cross-app impact
- An implementation agent can validate its work
- A review agent can check for spec drift

**All from the same set of artifacts in one KB repo.**

---

## Naming

"Spec-Driven Development" is already claimed by GitHub/Augment/Kiro. Possible names for this extended approach:

- **KB-Driven Multi-App Development**
- **Domain-Grounded Specification Architecture**
- **Coordinated Spec-Driven Development (CSDD)**
- **Knowledge-First Development (KFD)**

---

## Closest Related Work

| Work | Similarity | Key Difference |
|---|---|---|
| Codified Context (arXiv:2602.20478) | Structured docs for AI, domain agents | Single codebase, not multi-app |
| Augment Code Intent | Multi-repo coordination, Coordinator agent | Runtime tool, not persistent KB |
| GitHub Spec Kit | Spec-driven, AI-optimized | Single repo, no domain KG |
| GENIUS (arXiv:2511.01348) | Domain knowledge + code gen | No multi-app, no contract tests |
| Pact + AsyncAPI | Cross-service contracts | No AI agent role, no KG |

---

## Sources

- [GitHub Spec Kit](https://github.com/github/spec-kit)
- [Spec-Driven Development — GitHub Blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [Augment Code Intent](https://www.augmentcode.com/product/intent)
- [Kiro — AWS](https://kiro.dev/)
- [Codified Context (arXiv:2602.20478)](https://arxiv.org/abs/2602.20478)
- [KGACG (arXiv:2510.19868)](https://arxiv.org/abs/2510.19868)
- [GENIUS (arXiv:2511.01348)](https://arxiv.org/html/2511.01348v2)
- [KG-Based Repo-Level Code Gen (arXiv:2505.14394)](https://arxiv.org/html/2505.14394v1)
- [Stardog — Agentic AI Knowledge Graph](https://www.stardog.com/agentic-ai-knowledge-graph/)
- [ZBrain — Knowledge Graphs for Agentic AI](https://zbrain.ai/knowledge-graphs-for-agentic-ai/)
- [Specmatic — AsyncAPI Contract Testing](https://specmatic.io/demonstration/contract-testing-google-pub-sub-using-asyncapi-as-executable-contracts/)
- [PactFlow AI](https://pactflow.io/ai/)
- [Potpie](https://github.com/potpie-ai/potpie)
- [Oracle Open Agent Spec](https://github.com/oracle/agent-spec)
- [Claude Swarm Multi-Repo](https://parruda.net/posts/claude-swarm-how-ai-agent-teams-can-handle-your-multi-repo-reality/)
- [Augment Code — SDD & AI Agents](https://www.augmentcode.com/guides/spec-driven-development-ai-agents-explained)
- [InfoQ — SDD Enterprise Scale](https://www.infoq.com/articles/enterprise-spec-driven-development/)
