# Engineering Best Practices Audit — ruby-style-guide

| | |
|---|---|
| **Audit date** | 2026-09-08 |
| **Auditor** | Claude — gauge-repo skill |
| **Rubric version** | `item-credit-v1` — 2026-09-04 (`references/best-practices.md`) |

## Repo profile

`patterninc/ruby-style-guide` is a configuration/docs-only repository: it publishes a single shared RuboCop configuration (`rubocop.yml`) that Pattern Ruby projects inherit remotely via raw GitHub URLs, plus a `README.md` with consumer installation and update procedures, a Keep-a-Changelog `CHANGELOG.md`, an MIT `LICENSE`, and a Backstage catalog entry (`backstage.yaml`, owner `dev-integrations`). There is no application code, no Gemfile or dependencies of its own, no CI pipeline, no tests, no database, no deployment, no runtime, and no UI; "release" means merging to `main` (consumed at `/main/`) or cutting a semver git tag (tags `1.0.0` and `1.0.1` exist on origin). Contributor history is squashed to a single commit by `patterninc-gha-runner`. GitHub ownership is verified as `patterninc` (`gh repo view` confirms `patterninc/ruby-style-guide`), so Pattern's inherited Wiz and Toolsmith controls apply. No AWS footprint. This profile justifies the large number of N/A verdicts below: most runtime, testing, and environment practices genuinely add no value to a one-file YAML config repo.

## Scorecard

| Metric | Value |
|--------|-------|
| **Critical gates** | **RED** |
| **Adjusted compliance** | **52.9%** |

Critical gates are RED because three applicable gates are incomplete: item 2 (AGENTS.md — Gap), item 16 (required CI checks — Gap), and item 24 (integration tests — Gap). Gates 6, 15, 19, 20, and 48 are Met; gates 23 and 40 are N/A with profile-backed rationale. Adjusted compliance is calculated independently:

`(8 Met + 0.5 × 2 Partial) / (49 total - 32 justified N/A) = 9 / 17 = 52.9%`

### Status totals

| Status | Items |
|--------|------:|
| Met | 8 |
| Partial | 2 |
| Gap | 7 |
| N/A | 32 |
| **Total** | **49** |

### Per-category breakdown

| Category | Met | Partial | Gap | N/A |
|----------|----:|--------:|----:|----:|
| Documentation & Context | 2 | 2 | 3 | 2 |
| Guardrails & Enforcement | 3 | 0 | 2 | 8 |
| Testing & Feedback Loops | 0 | 0 | 1 | 12 |
| Environment & Tooling | 3 | 0 | 0 | 10 |
| Agent dispatch | 0 | 0 | 1 | 0 |
| **Total** | **8** | **2** | **7** | **32** |

## Documentation & Context

| # | Practice | Status | Evidence | Recommendation / rationale |
|---|----------|--------|----------|----------------------------|
| 1 | Skills / reusable prompt workflows | **Gap** | No `.claude/skills/`, `.claude/commands/`, or equivalent | Add a skill for the repo's one recurring workflow: change a cop, update `CHANGELOG.md`, tag a semver release, and notify consumers to regenerate `.rubocop_todo.yml`. |
| 2 | AGENTS.md | **Gap** | No `AGENTS.md`, `CLAUDE.md`, or `.cursorrules` | Add `AGENTS.md` covering: rule-change etiquette (every deviation needs an inline rationale comment), semver + changelog requirements, tagging procedure, and the consumer blast radius of edits to `rubocop.yml`. |
| 3 | Architecture decision records | **Partial** | Inline rationale comments in `rubocop.yml` explain most rule deviations (e.g., `Style/Documentation`, `Layout/LineLength`) | Keep the inline rationales; additionally capture repo-level decisions (plugin set, remote-inherit distribution model, versioning strategy) as short dated notes in `docs/`. |
| 4 | Runbooks | **Met** | `README.md` documents the recurring operational procedures: updating the style guide and regenerating `.rubocop_todo.yml` in consuming projects | — |
| 5 | API contract docs | **Not applicable** | No API surface | The artifact is a RuboCop config whose schema is validated by RuboCop itself. |
| 6 | README with setup & run instructions | **Met** | `README.md` gives full consumer installation (Gemfile gems, `.rubocop.yml` inherit, todo generation, CircleCI snippet, `.gitignore` entry) and maintainer update steps | — |
| 7 | Changelog with migration notes | **Partial** | `CHANGELOG.md` follows Keep a Changelog + SemVer; `1.0.0` documented with migration guidance in `README.md` | Backfill the missing `[1.0.1]` entry (tag exists on origin) and record the Backstage onboarding commit; keep the changelog current with tags. |
| 8 | On-call playbooks | **Not applicable** | Nothing deployed and no incident surface | The failure mode (a rule change breaking consumer CI) is covered by the regenerate-todo procedure in `README.md`. |
| 9 | CODEOWNERS | **Gap** | No `CODEOWNERS`; `backstage.yaml` names owner `dev-integrations` | Add `CODEOWNERS` assigning `rubocop.yml` to the dev-integrations team so the org-required PR review is routed to the owners of an org-wide config. |

## Guardrails & Enforcement

| # | Practice | Status | Evidence | Recommendation / rationale |
|---|----------|--------|----------|----------------------------|
| 10 | Linters | **Gap** | No lint/validation of the repo's own artifact; no CI | Add a YAML lint plus a RuboCop config validation step (e.g., `rubocop --config rubocop.yml --show-cops` with the plugin gems installed) so a malformed edit cannot land. |
| 11 | Formatters | **Not applicable** | No source code | One YAML file; style is covered by the item-10 validation lint. |
| 12 | Type checking | **Not applicable** | No code | Nothing to type-check. |
| 13 | Pre-commit hooks | **Not applicable** | Single-file YAML repo with org-enforced PR review | Local hook framework is ceremony at this scale; validation belongs in CI (items 10/16). |
| 14 | Commit message conventions | **Not applicable** | Changes are rare and the changelog is manually curated per Keep a Changelog (`CHANGELOG.md`, `README.md`) | Commit-convention automation (changelog generation) adds no value at this change frequency. |
| 15 | Branch protection rules | **Met** | Active org ruleset `require-pr-review` on the default branch (`gh api repos/patterninc/ruby-style-guide/rulesets`): blocks deletion and force-push, requires a PR with 1 approving review, dismisses stale reviews | — |
| 16 | Required CI checks before merge | **Gap** | No `.github/workflows/`; the effective ruleset has no required status checks | Add the item-10/24 validation workflow and make it a required status check so an unloadable config cannot merge to `main` (which consumers pull live). |
| 17 | Dependency allow-lists / deny-lists | **Not applicable** | No `Gemfile` or dependency manifest | The plugin gems named in `README.md` are installed by consumers, not this repo. |
| 18 | License compliance scanning | **Not applicable** | No dependencies | Nothing to scan; the repo itself ships `LICENSE` (MIT). |
| 19 | Secret scanning | **Met** | Inherited Pattern Wiz policy (verified `patterninc` owner) | — |
| 20 | SAST / static analysis gates | **Met** | Inherited Pattern Wiz policy (verified `patterninc` owner) | — |
| 21 | Max complexity limits | **Not applicable** | No executable code in this repo | Notably, this repo is the org-wide source of complexity ceilings for consumers (`Metrics/*` cops in `rubocop.yml`). |
| 22 | Import boundary enforcement | **Not applicable** | No code modules | No architectural layers to protect. |

## Testing & Feedback Loops

| # | Practice | Status | Evidence | Recommendation / rationale |
|---|----------|--------|----------|----------------------------|
| 23 | Unit tests | **Not applicable** | No functions or components | Docs/config-only repo; the config-load check (item 24) is the meaningful automated test. |
| 24 | Integration tests | **Gap** | No test that the shipped config actually loads | Add a CI job that installs `rubocop` plus the six plugin gems and runs RuboCop with `rubocop.yml` against a small fixture Ruby file, catching renamed/removed cops on new RuboCop releases before consumers do. |
| 25 | Snapshot / golden-file tests | **Not applicable** | No generated output | The config-load check (item 24) covers the regression surface of one YAML file. |
| 26 | Contract tests | **Not applicable** | No service API | Nothing to verify contracts against. |
| 27 | End-to-end tests | **Not applicable** | No application or UI | Nothing to drive end to end. |
| 28 | Visual regression tests | **Not applicable** | No visual surface | — |
| 29 | Test coverage thresholds | **Not applicable** | No code to cover | — |
| 30 | Mutation testing | **Not applicable** | No code or test suite to mutate | — |
| 31 | Load / performance benchmarks | **Not applicable** | Nothing executes | — |
| 32 | Flaky test quarantine | **Not applicable** | No test suite; the recommended item-24 check is a single deterministic job | — |
| 33 | Structured CI output | **Not applicable** | No CI matrix or test suite | The single recommended validation job's native GitHub Actions check output is sufficient; no parsing problem exists at this scale. |
| 34 | Deterministic test fixtures | **Not applicable** | No test data or stateful tests | The item-24 fixture file would be static by construction. |
| 35 | Smoke tests for deploys | **Not applicable** | Nothing is deployed | "Deploy" is a merge to `main`; the pre-merge validation (items 16/24) is the equivalent check. |

## Environment & Tooling

| # | Practice | Status | Evidence | Recommendation / rationale |
|---|----------|--------|----------|----------------------------|
| 36 | Devcontainer config | **Not applicable** | Editing one YAML file requires no environment | — |
| 37 | One-command setup | **Not applicable** | Nothing to bootstrap; consumer setup is documented in `README.md` | — |
| 38 | Seed scripts for local databases | **Not applicable** | No database | — |
| 39 | MCP servers for external tools | **Met** | Toolsmith-managed MCP access (verified `patterninc` owner) | — |
| 40 | Scoped secrets per environment | **Not applicable** | No credentials, environments, or deployment | Repo holds only public configuration. |
| 41 | Preview environments per PR | **Not applicable** | Nothing to deploy | A PR diff of `rubocop.yml` is fully reviewable as text. |
| 42 | Hot-reload / watch mode | **Not applicable** | No build/run loop | — |
| 43 | Structured logging (JSON) | **Not applicable** | No runtime | — |
| 44 | Observable traces and metrics | **Not applicable** | No runtime | — |
| 45 | Feature flags with local overrides | **Not applicable** | No runtime behavior to toggle | Consumers stage rule adoption via `.rubocop_todo.yml`, documented in `README.md`. |
| 46 | Database migration tooling | **Not applicable** | No database | — |
| 47 | Dependency update automation | **Met** | Org-wide Wiz (verified `patterninc` owner) | — |
| 48 | Reproducible builds (lockfiles) | **Met** | Equivalent: SemVer git tags `1.0.0` and `1.0.1` exist on origin and `README.md` documents pinning `inherit_from` to a tagged raw URL, so consumers can lock to an exact config version | — |

## Documentation & Context (agent dispatch)

| # | Practice | Status | Evidence | Recommendation / rationale |
|---|----------|--------|----------|----------------------------|
| 49 | Agent-dispatch manifest | **Gap** | No `.agents/pattern-agents.json` (or `.yml`/`.yaml`) | Add the manifest with `schema_version`, `github.repo`, `clickup_list_id`, `slack_channel`, `datadog.service`/`env`, and `skills.plugins`; no `aws[]` needed (no AWS footprint). |

## Prioritized recommendations

1. **[S] Gap — AGENTS.md (critical gate, item 2):** Add `AGENTS.md` documenting rule-change etiquette (inline rationale required), SemVer + changelog rules, the tagging procedure, and the org-wide blast radius of `rubocop.yml` edits.
2. **[S] Gap — required CI checks (critical gate, item 16):** Add a GitHub Actions workflow that validates `rubocop.yml` and make it a required status check before merge to `main`.
3. **[S] Gap — integration test (critical gate, item 24):** In that workflow, install `rubocop` and the six plugin gems and run RuboCop with `rubocop.yml` against a fixture Ruby file to prove the config loads and lints.
4. **[S] Gap — CODEOWNERS (item 9):** Add `CODEOWNERS` routing `rubocop.yml` reviews to the `dev-integrations` team named in `backstage.yaml`.
5. **[S] Gap — linters (item 10):** Add YAML linting of `rubocop.yml` alongside the config-load check.
6. **[S] Gap — agent-dispatch manifest (item 49):** Add `.agents/pattern-agents.json` with the core fields (GitHub, ClickUp, Slack, Datadog, skills).
7. **[M] Gap — skills (item 1):** Add a reusable skill/command for the rule-change release workflow (edit cop, changelog entry, tag, consumer todo-regeneration notice).
8. **[S] Partial — ADRs (item 3):** Capture repo-level decisions (plugin set, remote-inherit model, versioning strategy) as short dated notes in `docs/`.
9. **[S] Partial — changelog (item 7):** Backfill the `[1.0.1]` entry and the Backstage onboarding change; keep `CHANGELOG.md` in lockstep with tags.

## Declined practices

| # | Practice | Rationale |
|---|----------|-----------|
| 5 | API contract docs | No API; the artifact's schema is validated by RuboCop itself. |
| 8 | On-call playbooks | Nothing deployed; the only failure mode (broken consumer CI) is covered by the README regenerate-todo procedure. |
| 11 | Formatters | No source code; one YAML file covered by the recommended validation lint. |
| 12 | Type checking | No code. |
| 13 | Pre-commit hooks | Hook framework is ceremony for a one-file repo with org-enforced PR review; validation belongs in CI. |
| 14 | Commit message conventions | Manually curated Keep-a-Changelog at very low change frequency; convention automation adds no value. |
| 17 | Dependency allow/deny lists | No dependency manifest; plugin gems are installed by consumers. |
| 18 | License compliance scanning | No dependencies to scan. |
| 21 | Max complexity limits | No executable code here (the repo instead defines these limits org-wide). |
| 22 | Import boundary enforcement | No code modules. |
| 23 | Unit tests | No functions/components; the config-load check is the meaningful test. |
| 25 | Snapshot / golden-file tests | No generated output; config-load check covers the regression surface. |
| 26 | Contract tests | No service API. |
| 27 | End-to-end tests | No application or UI. |
| 28 | Visual regression tests | No visual surface. |
| 29 | Test coverage thresholds | No code to cover. |
| 30 | Mutation testing | No code or test suite. |
| 31 | Load / performance benchmarks | Nothing executes. |
| 32 | Flaky test quarantine | No test suite; recommended check is single and deterministic. |
| 33 | Structured CI output | Single trivially-readable validation job once CI exists; no parsing problem at this scale. |
| 34 | Deterministic test fixtures | No test data; the recommended fixture is static by construction. |
| 35 | Smoke tests for deploys | Nothing deployed; pre-merge validation is the equivalent. |
| 36 | Devcontainer config | No environment needed to edit one YAML file. |
| 37 | One-command setup | Nothing to bootstrap. |
| 38 | Seed scripts | No database. |
| 40 | Scoped secrets per environment | No credentials or deployment; public configuration only. |
| 41 | Preview environments per PR | Nothing to deploy; PR diff is fully reviewable as text. |
| 42 | Hot-reload / watch mode | No build/run loop. |
| 43 | Structured logging | No runtime. |
| 44 | Traces and metrics | No runtime. |
| 45 | Feature flags | No runtime behavior; consumers stage adoption via `.rubocop_todo.yml`. |
| 46 | Database migration tooling | No database. |

## Beyond the checklist

- Nearly every rule deviation in `rubocop.yml` carries an inline rationale comment citing its source (rubystyle.guide, Airbnb, Azure guides) — exactly the "why" context an agent needs when asked to change a cop.
- The distribution model (remote `inherit_from` over raw GitHub URLs with optional tag pinning) gives every consuming repo a zero-copy upgrade path, and `README.md` documents both the pinned and tracking modes.
- The consumer rollout procedure explicitly handles the migration problem of a shared linter config: `.rubocop_todo.yml` regeneration with `--auto-gen-only-exclude` keeps consumer builds green while new rules land.
- `backstage.yaml` registers the repo in Pattern's Backstage catalog with a named owning team (`dev-integrations`), giving agents and humans a discoverable ownership record.
