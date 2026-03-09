---
name: project-bootstrapper
description: |
  Meta-skill that bootstraps any software project by auto-generating a complete suite of hyper-detailed, project-specific coding skills BEFORE any code is written. Analyzes the idea, determines tech stack, then generates 15-30+ skills covering architecture, security, performance, privacy, testing, error handling, accessibility, observability, data modeling, API design, DevOps, and every relevant domain. Each generated skill enforces zero-bug, zero-vulnerability standards with concrete code examples, anti-patterns, checklists, and measurable budgets. Triggers on: "bootstrap", "new project", "start from scratch", "build me", "I want to create", "set up project", "generate skills", "skill suite", or any project idea description. Use BEFORE writing any code.
---

# Project Bootstrapper

> **Philosophy**: Define how code must be written before writing any code. Bugs are prevented at design time, not discovered at runtime.

This is a **meta-skill** — it does not write application code. It generates the **rules, patterns, guardrails, and quality standards** that govern all code written afterward, by any developer or AI assistant.

## How It Works

```
[Idea] → [Interview] → [Tech Stack] → [Skill Map] → [Generate Skills] → [Validate] → [Code]
```

## Activation

This skill activates when:
- User describes a new software project idea
- User says "bootstrap", "new project", "start from scratch", "set up project"
- User wants to generate a skill suite for an existing codebase
- User asks for coding standards, project scaffolding, or development guardrails
- Any context where skills should be created before development begins

## Important: Read References First

Before generating ANY skills, you MUST read these reference files in order:
1. `references/skill-catalog.md` — Full catalog of 40+ skill domains
2. `references/skill-template.md` — Universal template every skill must follow
3. `references/generation-guide.md` — Domain-specific generation instructions with code
4. `references/quality-standards.md` — Quality checklist for generated skills
5. `references/cross-cutting-concerns.md` — Rules that span all skills

---

## Phase 1: Project Intelligence Gathering

### 1.1 — Understand the Idea

Extract or ask about:

**What** (Product):
- What does this project do? (one sentence)
- What type is it? (web app, mobile app, desktop app, CLI, library/SDK, API service, browser extension, IoT, embedded, game, data pipeline, ML platform, monorepo with multiple products)
- Who is the end user? (developers, consumers, enterprise, internal team)
- What is the revenue model? (open-source, freemium, SaaS subscription, one-time purchase, marketplace, ad-supported, enterprise license)

**How Big** (Scale):
- Expected users at launch? At 12 months?
- Data volume? (records, files, events/sec, storage)
- Geographic scope? (single region, continental, global)
- Availability requirement? (99.9%, 99.95%, 99.99%)

**How** (Constraints):
- Required technologies? (must use React, must deploy on AWS, etc.)
- Existing codebase? (greenfield vs brownfield)
- Team size? (solo, small team 2-5, medium 5-15, large 15+)
- Timeline? (hackathon/weekend, MVP in weeks, production in months)
- Budget constraints? (free tier only, moderate, enterprise)
- Compliance requirements? (GDPR, HIPAA, SOC2, PCI-DSS, COPPA, CCPA)

**If the user already provided details**, extract answers from their message instead of asking. Only ask what's missing and genuinely needed to make tech stack decisions.

### 1.2 — Determine Tech Stack

Based on the answers, recommend a complete tech stack. Organize as a layered decision table:

```
┌─────────────────────────────────────────────────────┐
│  TECH STACK PROPOSAL                                │
├────────────────┬────────────────────────────────────┤
│ Category       │ Choice → Rationale                 │
├────────────────┼────────────────────────────────────┤
│ Language       │                                    │
│ Runtime        │                                    │
│ Framework      │                                    │
│ Database       │                                    │
│ ORM/Query      │                                    │
│ Cache          │                                    │
│ Auth           │                                    │
│ UI Library     │                                    │
│ CSS/Styling    │                                    │
│ State Mgmt     │                                    │
│ API Style      │                                    │
│ Validation     │                                    │
│ Testing        │                                    │
│ CI/CD          │                                    │
│ Hosting        │                                    │
│ Monitoring     │                                    │
│ Email          │                                    │
│ File Storage   │                                    │
│ Search         │                                    │
│ Queue/Jobs     │                                    │
│ Analytics      │                                    │
└────────────────┴────────────────────────────────────┘
```

Only include rows relevant to the project. Each choice gets a one-line rationale.

**Wait for user confirmation before proceeding.** The tech stack determines everything that follows.

### 1.3 — Generate Skill Map

Based on confirmed tech stack, produce a skill map — the complete list of skills to generate. Read `references/skill-catalog.md` for the full domain catalog.

**Mandatory skills** (generated for every project):
1. `project-architecture` — folder structure, module boundaries, naming
2. `{language}-standards` — language-level coding rules
3. `security-hardening` — defense in depth, input/output, secrets, deps
4. `error-handling` — error hierarchy, propagation, recovery
5. `data-validation` — schema validation, sanitization, boundaries
6. `testing-strategy` — test types, coverage, mocking, fixtures
7. `performance-optimization` — budgets, profiling, caching, lazy loading
8. `git-workflow` — branches, commits, PRs, releases
9. `documentation-standards` — code docs, API docs, READMs, ADRs
10. `privacy-compliance` — PII handling, data lifecycle, consent, GDPR/CCPA
11. `dependency-management` — versioning, auditing, update policy, lockfiles

**Conditional skills** (generated when the project needs them):
- `{framework}-patterns` — framework-specific conventions
- `database-design` — schema, migrations, indexing, queries
- `api-design` — endpoints, versioning, pagination, rate limiting
- `ui-engineering` — components, styling, responsive, a11y
- `state-management` — client/server/URL/form state patterns
- `auth-patterns` — authn, authz, sessions, tokens, MFA
- `devops-pipeline` — CI/CD, environments, deployment, rollback
- `observability` — logging, metrics, tracing, alerting
- `accessibility-standards` — WCAG compliance, ARIA, keyboard nav
- `internationalization` — i18n, l10n, RTL, pluralization
- `payment-integration` — billing, subscriptions, webhooks, PCI
- `file-handling` — uploads, storage, processing, CDN
- `realtime-system` — WebSocket, SSE, pub/sub, presence
- `email-system` — transactional, templates, queue, compliance
- `search-implementation` — engine, indexing, relevance, autocomplete
- `background-jobs` — queues, scheduling, retry, dead letter
- `mobile-patterns` — navigation, offline, push, deep links
- `desktop-patterns` — window mgmt, tray, IPC, auto-update
- `cli-design` — commands, args, output, config, shell completion
- `monorepo-management` — workspaces, boundaries, versioning
- `ai-integration` — LLM calls, prompts, streaming, cost, safety
- `caching-strategy` — layers, invalidation, CDN, stale-while-revalidate
- `rate-limiting` — algorithms, tiers, headers, distributed limiting
- `feature-flags` — rollout, targeting, kill switches, cleanup
- `migration-strategy` — zero-downtime, data migrations, backward compat
- `container-orchestration` — Docker, K8s, health checks, resources
- `infrastructure-as-code` — Terraform/Pulumi, state, modules
- `event-driven-architecture` — event sourcing, CQRS, sagas
- `graphql-patterns` — schema design, resolvers, N+1, batching
- `websocket-patterns` — connection management, rooms, reconnection
- `microservice-patterns` — service boundaries, communication, discovery

Present the skill map organized by generation layer. **Wait for user confirmation.**

---

## Phase 2: Skill Generation Engine

### 2.1 — Generation Order (Dependency Layers)

Generate skills in strict dependency order — later skills can reference earlier ones:

```
Layer 0: project-architecture
         (defines folder structure, module boundaries, naming — everything else references this)

Layer 1: {language}-standards, git-workflow, documentation-standards
         (foundational coding and process standards)

Layer 2: security-hardening, error-handling, data-validation, privacy-compliance,
         dependency-management
         (cross-cutting safety and quality concerns)

Layer 3: database-design, api-design, auth-patterns, caching-strategy
         (data and communication layer)

Layer 4: {framework}-patterns, ui-engineering, state-management,
         accessibility-standards
         (presentation and interaction layer)

Layer 5: testing-strategy, performance-optimization
         (quality assurance — needs all other skills to exist first)

Layer 6: devops-pipeline, observability, container-orchestration,
         infrastructure-as-code
         (operations layer)

Layer 7: Domain-specific skills (payments, i18n, email, search, realtime,
         background-jobs, feature-flags, AI, etc.)
         (only relevant domains)
```

### 2.2 — Skill File Structure

Every generated skill MUST produce this file tree:

```
{skill-name}/
├── SKILL.md              # Main instructions (< 500 lines)
├── references/
│   ├── patterns.md       # Approved patterns with full code examples
│   ├── anti-patterns.md  # Forbidden patterns with severity + explanation
│   └── checklist.md      # Pre-commit/pre-merge verification checklist
└── templates/            # (optional) Code templates, configs
    └── *.template.*
```

### 2.3 — Content Requirements

Read `references/skill-template.md` for the exact skeleton. Read `references/generation-guide.md` for domain-specific content requirements.

Every generated skill MUST contain:

1. **YAML frontmatter** — name + aggressive description for reliable triggering
2. **Activation conditions** — exact triggers (file types, directories, user phrases)
3. **Project context** — references to actual project tech, paths, decisions
4. **Numbered core rules** (15-40 per skill) — each with rationale + code examples
5. **Approved patterns** — copy-pasteable code showing the right way
6. **Anti-patterns with severity** (🔴 CRITICAL / 🟠 HIGH / 🟡 MEDIUM / 🟢 LOW)
7. **Performance budgets** — concrete measurable numbers, not vague goals
8. **Security checklist** — domain-specific security verification items
9. **Error scenarios table** — what fails, how to detect, how to recover
10. **Edge cases** — documented with handling instructions
11. **Integration points** — how this skill connects to other generated skills
12. **Pre-commit checklist** — verification items before code can be committed

### 2.4 — Writing Principles

- **Explain the WHY**: Claude and developers are smart — reasoning > rigid commands
- **Concrete over abstract**: Real code examples > descriptions of code
- **Project-specific**: Reference actual tech choices, paths, and decisions — never generic
- **Opinionated**: Pick one best approach and enforce it, don't offer menus
- **Testable**: Every rule must be verifiable (lint rule, test, code review check)
- **Examples compile**: All code examples must work if copy-pasted, no pseudocode
- **Both sides**: Show correct AND incorrect for every critical rule

### 2.5 — Cross-Skill Consistency

After generating all skills, verify:
- [ ] No contradictions between skills
- [ ] Shared terminology is consistent across all skills
- [ ] Import paths reference actual project structure
- [ ] Tech versions match across all skills
- [ ] Error handling patterns are uniform
- [ ] Logging format is identical everywhere
- [ ] Validation approach is the same everywhere
- [ ] Security rules don't conflict

---

## Phase 3: Output

### 3.1 — Directory Layout

```
{project-root}/
├── .claude/
│   └── skills/
│       ├── project-architecture/
│       │   ├── SKILL.md
│       │   └── references/
│       ├── {language}-standards/
│       │   ├── SKILL.md
│       │   ├── references/
│       │   └── templates/
│       ├── security-hardening/
│       │   ├── SKILL.md
│       │   └── references/
│       ├── ... (all generated skills)
│       └── _bootstrap-manifest.json
├── .gitignore
└── ... (application code comes AFTER bootstrap)
```

### 3.2 — Bootstrap Manifest

Generate `_bootstrap-manifest.json`:

```json
{
  "project": "{name}",
  "bootstrapped_at": "{ISO-8601}",
  "tech_stack": {},
  "skills_generated": [
    {
      "name": "{skill-name}",
      "path": ".claude/skills/{skill-name}/",
      "layer": 0,
      "depends_on": [],
      "domains_covered": ["architecture", "folder-structure", "naming"]
    }
  ],
  "total_skills": 0,
  "total_rules": 0,
  "total_anti_patterns": 0,
  "coverage": {
    "security": true,
    "performance": true,
    "privacy": true,
    "testing": true,
    "accessibility": true,
    "error_handling": true,
    "documentation": true,
    "observability": true
  }
}
```

---

## Phase 4: Validation

Before declaring bootstrap complete, run through:

1. **Completeness** — Every tech stack component is covered by at least one skill
2. **Contradictions** — No two skills give conflicting advice
3. **Dependencies** — Every skill's `depends_on` targets exist
4. **Coverage** — Security, performance, privacy, testing, error handling all covered
5. **Specificity** — Skills reference actual project names, paths, versions
6. **Quality** — Run `scripts/validate_bootstrap.py` against the generated skills

Present a summary table:

```
╔═══════════════════════════════════════════════════╗
║  BOOTSTRAP COMPLETE                               ║
╠═══════════════════════════════════════════════════╣
║  Project: {name}                                  ║
║  Skills Generated: {N}                            ║
║  Total Rules: {N}                                 ║
║  Total Anti-Patterns: {N}                         ║
║  Security Rules: {N}                              ║
║  Performance Budgets: {N}                         ║
║  Privacy Controls: {N}                            ║
║  Test Requirements: {N}                           ║
╠═══════════════════════════════════════════════════╣
║  ✅ No contradictions found                       ║
║  ✅ All dependencies resolved                     ║
║  ✅ Full coverage verified                        ║
║  ✅ Ready to code                                 ║
╚═══════════════════════════════════════════════════╝
```

---

## Phase 5: Handoff

After bootstrap, tell the user all skills are generated and placed in the project directory. Every file created or edited from now on will be governed by the relevant skills automatically.

Then transition to actual development.

---

## Rules for This Skill

- This skill generates OTHER skills — it never generates application code
- Generated skills must be project-specific, not generic boilerplate
- When in doubt, generate MORE skills rather than fewer
- If the user's project needs a domain not in the catalog, invent a new skill for it
- Follow the user's language preference (English by default)
- The tech stack is tech-agnostic: works for TypeScript, Python, Go, Rust, Java, C#, Swift, Kotlin, PHP, Ruby, or any combination
- For polyglot projects, generate per-language skills
- Read ALL reference files before generating ANY skills
- Use `scripts/validate_bootstrap.py` after generation
