---
layout: section
---

# Architecture

Important Principles, Possibilities & Techniques

---
layout: why
---

# Why Architecture Matters

The team needs to agree on repetitive patterns that smooth the development process. Clear concepts and boundaries.

- Messy codebases without agreed structure
- Hard onboarding for new team members
- Slow builds and unclear dependencies
- Unclear boundaries between domains

---
layout: little-what
---

# What is Architecture (in this context)?

From Project-Structure to Release-Strategy

---
layout: sub-section
---

# Project/Code-Structure

---
layout: why
---

# Why Project Structure Matters

A consistent structure makes navigation predictable and reduces cognitive load. Screaming Architecture (Robert C. Martin) means the folder structure reveals what the app does.

---
layout: little-what
---

# Project Structure Options

Feature-based (by domain) vs. layer-based (components/services/models). Monorepo vs multi-repo. Libraries and shared modules. Microfrontends.

---
layout: default
---

# Feature-based vs. Layer-based

- **Feature-based**: Organize by domain (`users/`, `orders/`, `products/`)
- **Layer-based**: Organize by technical role (`components/`, `services/`, `models/`)
- **Screaming Architecture**: Structure should "scream" the application's purpose

---
layout: default
---

# Example: Feature-based Structure

<WindowMockup codeblock title="src/app/" padding="2rem">
```
src/app/
├── users/
│   ├── user-list.component.ts
│   ├── user.service.ts
│   └── public-api.ts
├── orders/
│   ├── order-list.component.ts
│   ├── order.service.ts
│   └── public-api.ts
├── shared/
│   ├── ui/
│   └── public-api.ts
└── app.component.ts
```
</WindowMockup>

---

<tldraw class="inset-0 w-full h-full" doc="tldraw/doc-h6AKUjxNJ8w7tyRwBKyuy.json"></tldraw>

---
layout: task
---

# Project Structure

---
layout: sub-section
---

# Component Architecture

---
layout: why
---

# Why Component Architecture Matters

Smart vs. dumb components improve testability and reusability. Clear separation of concerns makes changes safer.

---
layout: little-what
---

# Smart vs. Dumb Components

- **Smart (Container)**: Handles data, state, side effects; talks to services/facades
- **Dumb (Presentational)**: Pure UI; receives `input`, emits `output`
- Design systems and UI libraries typically use dumb components

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Smart & Dumb Components

::left::

**Data flow:**

- Component (Smart) fetches data via Facade
- Component passes data to Dumb children via `input`
- Dumb children emit events via `output`
- Smart component handles events and updates state

::right::

```mermaid
flowchart TB
    SmartComponent[Smart Component]
    DumbComponent[Dumb Component]
    Facade[Facade]
    Service[Service / Store]
    SmartComponent --> Facade
    SmartComponent --> DumbComponent
    Facade --> Service
    DumbComponent -->|output| SmartComponent
```

::bottom::
<Callout type="info">
Dumb components are easier to test and reuse in design systems
</Callout>

---

<tldraw class="inset-0 w-full h-full" doc="tldraw/doc-Z191fYHEpzHZWxBJMS4rc.json"></tldraw>

---
layout: sub-section
---

# State Management & Data Flow

---
layout: why
---

# Why State Management Matters

Centralized vs. decentralized state affects complexity and debugging. Clear data flow reduces bugs and improves predictability.

---
layout: little-what
---

# State Management Options

- Signal Store, Redux, or simple RxJS
- Centralized vs. decentralized store solutions
- Command Query Segregation (CQS) for clear boundaries

---
layout: default
---

# State Management Considerations

- Where does state live? (component, service, store)
- Unidirectional data flow vs. two-way binding
- CQS: Commands change state, Queries return data

---

<tldraw class="inset-0 w-full h-full" doc="tldraw/doc-YECbies_Jnf50p9Ds1nTa.json"></tldraw>

---
layout: sub-section
---

# Layering & Abstractions

---
layout: why
---

# Why Layering Matters

Clear layers prevent circular dependencies and keep the codebase maintainable. Abstractions hide implementation details.

---
layout: little-what
---

# Typical Layers

UI (components) → Facade → Domain / Services → API

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Layering: UI → Facade → Domain → API

::left::

**Layer responsibilities:**

- **UI**: Components, templates, presentational logic
- **Facade**: Orchestrates services, exposes simple API to components
- **Domain**: Business logic, models
- **API**: HTTP, external integrations

::right::

```mermaid
flowchart TB
    UI[UI Components]
    Facade[Facade]
    Domain[Domain / Services]
    API[API Layer]
    UI --> Facade
    Facade --> Domain
    Domain --> API
    API --> Domain
    Domain --> Facade
    Facade --> UI
```

::bottom::
<Callout type="info">
Components should only talk to Facades, not directly to services
</Callout>

---

<tldraw class="inset-0 w-full h-full" doc="tldraw/doc-jQA9kKH_j7IsifeHqVJ-e.json"></tldraw>

---
layout: sub-section
---

# Module/Domain Boundaries

---
layout: why
---

# Why Boundaries Matter

Without boundaries, domains import from each other freely. Refactoring becomes risky and dependencies grow uncontrolled.

---
layout: little-what
---

# Enforcing Boundaries

- [Sheriff](https://www.berger-engineering.io/articles/scalable-architecture-with-sheriff): Angular boundary rules
- [eslint-plugin-boundaries](https://github.com/javierbrea/eslint-plugin-boundaries): ESLint rules for architectural layers

---

<tldraw class="inset-0 w-full h-full" doc="tldraw/doc-ZOagltEnYMckDqnjuq5YD.json"></tldraw>

---
layout: task
---

# Enforce Module Boundaries

---
layout: sub-section
---

# Performance Architecture

---
layout: why
---

# Why Performance Architecture Matters

Large bundles and unnecessary change detection slow down the app. Strategic optimizations improve UX and reduce costs.

---
layout: little-what
---

# Performance Techniques

Lazy loading, code splitting, change detection strategy, SSR/SSG, caching.

---
layout: default
---

# Performance Checklist

- Lazy load feature routes
- Use `OnPush` change detection where possible
- Consider SSR/SSG for first paint
- Cache HTTP responses and computed values

---

<tldraw class="inset-0 w-full h-full" doc="tldraw/doc-QEnVijoF_Yc4seJZjG32z.json"></tldraw>

---
layout: sub-section
---

# Build & Deployment Architecture

---
layout: why
---

# Why Build & Deployment Matters

Monorepo tooling (e.g. Nx), CI/CD, and release strategies determine how fast and safely you can ship.

---
layout: little-what
---

# Build & Deployment Topics

- Monorepo (Nx)
- CI/CD pipelines
- Microfrontends
- Release strategies

---
layout: default
---

# Promote Artifacts, Don't Rebuild

Build once and promote the same artifact through stages (dev → test → prod).

- **Less build time** — no redundant builds per environment
- **More predictable deployments** — identical artifact across all stages

---

<tldraw class="inset-0 w-full h-full" doc="tldraw/doc-YQRk8I2X6-z5V3_nLT7An.json"></tldraw>

---
layout: task
---

# Runtime Configuration

---
layout: sub-section
---

# Documenting Architecture Decisions

---
layout: why
---

# Why Document Decisions

Decisions get forgotten. ADRs (Architecture Decision Records) capture the context and rationale for future readers.

---
layout: little-what
---

# Architecture Decision Records (ADRs)

Lightweight docs: Context, Decision, Consequences. Store in `docs/adr/` or similar.

---
layout: two-cols-header
---

# ADR Template

::left::

- One ADR ≈ one architecturally significant decision
- ~1–2 pages; written for future readers

::right::

<WindowMockup codeblock title="my-decision-record.md">

```md
# Title (short noun phrase)

## Status

proposed | accepted | deprecated | superseded

## Context

Forces at play — technological, political, project local
(value-neutral, no advocacy)

## Decision

"We will …" — full sentences, active voice

## Consequences

Resulting context; positive, negative, neutral
```

</WindowMockup>

---
layout: why
---

# What Happens Without ADRs?

- Confused team — no shared understanding of past choices
- Knowledge lost — why decisions were made fades over time
- Impact of decisions unclear — hard to revisit or improve
- Less motivation to change — rationale invisible, improvement feels risky

---
layout: default
---

# Outlook: ADRs + AI

If ADRs are present, architecture documentation (e.g. Arc42) can be generated quickly. AI tools use ADRs as context to draft and refine docs — a strong what-if for documentation automation.

---
layout: task
---

# Architecture Decision Record

---
layout: sub-section
---

# When to Use Nx — When Not

---
layout: why
---

# When Nx Helps

- Creating and sharing libraries (better DX than Angular CLI alone)
- Extremely long build times (cached builds, affected commands)
- Full-stack platform with harmonized tooling
- You value the approach and know the ecosystem

---
layout: little-what
---

# When Nx Might Not Fit

- You only want to extract libraries (Angular CLI can suffice)
- You want Angular + Storybook/Playwright without extra indirection
- You prefer fewer tooling layers for faster Angular upgrades

---
layout: two-cols
---

# Use Nx when...

::left::

- Sharing libraries across apps
- Need cached/affected builds
- Full-stack (Angular + .NET/Java/Node)
- Long build times hurting DX

::right::

# Skip Nx when...

- Simple library extraction only
- Want minimal tooling (Angular + Storybook)
- Prefer direct Angular updates without Nx indirection
