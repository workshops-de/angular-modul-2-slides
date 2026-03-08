# Bonus Challenges

## Bonus 1: Apply to a Second Feature

Extend the same architecture to another feature in the app: feature folder, `public-api.ts`, Facade, and ensure boundaries are respected.

## Bonus 2: Add Boundary Rules for More Domains

Expand your eslint-plugin-boundaries config to cover additional domains (e.g. `shared`, `core`). Define which domains can import from which (e.g. features can import from `shared`, but `shared` cannot import from features).

## Bonus 3: Document the Architecture

Create an ADR (Architecture Decision Record) that describes your folder structure, boundary rules, and Facade usage. Store it in `docs/adr/001-feature-architecture.md`.
