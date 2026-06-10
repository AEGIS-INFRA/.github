# Contributing to AEGIS

Thanks for your interest in contributing to **AEGIS** — open AI safety infrastructure for the internet. Contributions of all kinds are welcome: code, detection modules, documentation, research, design, and well-reported bugs.

This guide explains how to get involved and what we expect from contributions. By participating, you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md). <!-- TODO: add CODE_OF_CONDUCT.md -->

> **Pre-release status.** AEGIS is under active development and most repositories are currently **private**. Some processes below (public issue templates, package publishing, CLA tooling) are not fully in place yet and are marked **TODO**. If you have access and a question this guide doesn't answer, open a discussion or reach a maintainer.

---

## Table of Contents

1. [Ways to Contribute](#ways-to-contribute)
2. [Where Things Live](#where-things-live)
3. [Before You Start](#before-you-start)
4. [Development Setup](#development-setup)
5. [Development Workflow](#development-workflow)
6. [Coding Standards](#coding-standards)
7. [Testing Requirements](#testing-requirements)
8. [Documentation](#documentation)
9. [Contributing a Detection Module](#contributing-a-detection-module)
10. [Privacy & Safety Requirements](#privacy--safety-requirements)
11. [Reporting Security Vulnerabilities](#reporting-security-vulnerabilities)
12. [Reporting Bugs & Requesting Features](#reporting-bugs--requesting-features)
13. [Pull Request Process](#pull-request-process)
14. [Licensing of Contributions](#licensing-of-contributions)

---

## Ways to Contribute

- **Code** — bug fixes, features, performance, and platform work across the SDK, engine, and extension.
- **Detection modules** — new or improved harm-category modules (scam, phishing, adult content, malware, …). See [Contributing a Detection Module](#contributing-a-detection-module).
- **Documentation** — improvements to the [wiki](https://github.com/AEGIS-INFRA/.github/wiki), code comments, and examples.
- **Research** — detection techniques, evaluation methodology, bias analysis, adversarial robustness.
- **Bug reports & feedback** — clear, reproducible issues and real-world integration feedback.

If you're new, look for issues labeled `good first issue` or `help wanted`. **TODO: confirm label taxonomy.**

## Where Things Live

| Repository | Contribute here for… |
|------------|----------------------|
| [`aegis-core`](https://github.com/AEGIS-INFRA/aegis-core) | The detection + policy **engine** (extraction, scoring, fusion, policy). |
| [`aegis-js-sdk`](https://github.com/AEGIS-INFRA/aegis-js-sdk) | The **Web/JavaScript SDK** that embeds the engine. |
| [`aegis-browser-extension`](https://github.com/AEGIS-INFRA/aegis-browser-extension) | The **Family Protection** browser extension. |
| [`aegis-website`](https://github.com/AEGIS-INFRA/aegis-website) | The product website, developer resources, and trust pages. |
| [`.github`](https://github.com/AEGIS-INFRA/.github) | The org profile and the **documentation wiki**. |

Before opening a PR, make sure you're in the right repository — engine logic belongs in `aegis-core`, not in the SDK or extension wrappers.

## Before You Start

- **Search first.** Check existing [issues](https://github.com/AEGIS-INFRA/aegis-core/issues) and discussions to avoid duplicates.
- **Discuss large changes first.** For anything beyond a small fix — new features, API changes, new modules, architectural changes — open an issue or discussion describing the problem and your proposed approach *before* writing code. This avoids wasted work on something that doesn't fit the design.
- **Respect the architecture boundaries.** AEGIS keeps detection, scoring, and policy separate (see [How It Works](https://github.com/AEGIS-INFRA/.github/wiki/How-It-Works)). Scoring must not enforce; policy must not score. Keep PRs within one layer where possible.
- **Keep PRs focused.** One logical change per PR. Unrelated refactors should be separate.

## Development Setup

> Exact commands depend on the repo; these are the expected basics. **TODO: confirm toolchain versions and scripts per repo.**

**Web SDK / engine (`aegis-js-sdk`, `aegis-core`)**
```bash
git clone https://github.com/AEGIS-INFRA/aegis-js-sdk
cd aegis-js-sdk
npm install
npm run build
npm test
```

**Browser extension (`aegis-browser-extension`)**
```bash
git clone https://github.com/AEGIS-INFRA/aegis-browser-extension
cd aegis-browser-extension
npm install
npm run build           # then load the unpacked build in your browser
```

Requirements (TODO: confirm): a current Node.js LTS, npm or yarn, and a Chromium-based browser for extension and ORT-Web testing.

## Development Workflow

1. **Fork** the repository (or branch, if you're a maintainer).
2. **Branch** from `main` with a descriptive name: `fix/extraction-shadow-dom`, `feat/phishing-module`, `docs/configuration-thresholds`.
3. **Commit** using [Conventional Commits](https://www.conventionalcommits.org/): `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`, `perf:`. Example: `fix(policy): clamp escalate correctly under monitor level`.
4. **Keep history clean** — rebase on `main` before opening the PR; squash noise.
5. **Open a PR** against `main` using the PR template, linking the issue it resolves.

## Coding Standards

- **Language & types.** TypeScript with `strict` mode; no `any` without justification. Public APIs must be fully typed.
- **Style.** Formatting and linting are enforced (Prettier + ESLint). Run `npm run lint` and `npm run format` before pushing. **TODO: confirm config.**
- **Naming.** Follow the canonical vocabulary from the wiki — actions are `allow`/`label`/`blur`/`block`/`escalate`/`report`; profiles are `general`/`child_safe`/`student`/`enterprise`/`custom`. Don't reintroduce legacy names (`mask`, `warn`, etc.).
- **Comments.** Explain *why*, not *what*. Document non-obvious constraints (browser quirks, budgets).
- **No breaking changes without discussion.** Public API or config-schema changes require an issue and a migration note.

## Testing Requirements

Tests are required for all behavioral changes. A PR that changes logic without tests will be asked for them.

- **Unit tests** for engine logic (extraction, scoring, fusion, thresholding, policy resolution). Policy and scoring are pure functions — test them deterministically.
- **Browser tests** for anything touching ML inference or the DOM. ORT-Web WASM/WebGPU and Node behave differently — **validate in a real (headless) browser**, not just Node.
- **No network in tests for local-first paths.** Local-only behavior must be testable offline.
- **Coverage** should not regress. **TODO: confirm coverage threshold.**
- Run the full suite (`npm test`) and confirm it passes before opening the PR.

## Documentation

- Update docs **in the same PR** as the code change. New config properties, events, actions, or module fields must be reflected in the [wiki](https://github.com/AEGIS-INFRA/.github/wiki) (Configuration, How It Works, Metrics, etc.).
- Follow the existing wiki conventions: clear headings, short paragraphs, configuration tables with `Property / Type / Default / Description`, and placeholders/`TODO` markers for genuinely unknown details.
- Keep examples consistent with the canonical decision vocabulary and the running scam example used across the docs.

## Contributing a Detection Module

Modules are how AEGIS detects harm categories. Building one is a full lifecycle, documented in [Preparing & Registering a Module](https://github.com/AEGIS-INFRA/.github/wiki/Preparing-and-Registering-a-Module). A module contribution **must** include:

- A valid **`ModuleManifest`** (unique `moduleId`, correct `category`, semver, declared modalities, per-profile thresholds, performance budget, integrity hashes).
- A signed **bundle** whose artifacts match their declared **SHA-256** hashes (a mismatch quarantines the module).
- A **`model_card.md`** documenting training data, sub-group false-positive rates, and known limitations.
- A **`benchmark_report.json`** with P50/P90/P99 latency and peak memory on reference hardware.
- Evidence the module **passes its evaluation gates** (accuracy, runtime budget, robustness, bias) — a module that meets accuracy targets but exceeds its latency budget is **not** conforming.
- An `execute()` implementation that obeys the contract: no network access, no DOM access, no out-of-Worker state, scores in `[0.0, 1.0]`, within the declared `maxLatencyMs`.

> ## ⚠ Child-safety requirement (non-negotiable)
> For any module whose training touches sexual or adult content: **no sexual imagery involving minors (CSAM) may appear in any dataset — training, validation, test, or calibration.** Any such material encountered during dataset construction must be excluded immediately and reported to **NCMEC** (USA) or the appropriate national authority. This policy is non-overridable. PRs that cannot demonstrate compliance will not be merged.

Module PRs receive extra review for safety, privacy, calibration, and budget compliance. Use the [pre-registration checklist](https://github.com/AEGIS-INFRA/.github/wiki/Preparing-and-Registering-a-Module#14-pre-registration-checklist) before submitting.

## Privacy & Safety Requirements

Every contribution must uphold AEGIS's core guarantees:

- **Local-first.** Don't add code paths that send creative content, browsing data, or screenshots off-device by default. New cloud features must be opt-in, key-gated, and documented.
- **No raw content in telemetry.** Metrics events carry references and metadata — never ad copy, images, or personal data. Use ephemeral IDs; never introduce stable cross-site identifiers.
- **Child-safety first.** Changes affecting `child_safe`/`student` profiles must fail closed and must never emit child-identifying information.
- **Explainability.** Decisions must remain explainable — preserve `riskScore`, `categories`, `confidence`, `source`, and the explanation.

PRs that weaken these guarantees will be declined regardless of other merit.

## Reporting Security Vulnerabilities

**Do not open a public issue for security vulnerabilities.** This includes module signature/verification bypasses, sandbox escapes, privacy leaks, and anything that could be exploited. Follow the process in [`SECURITY.md`](SECURITY.md) and report privately to the maintainers. **TODO: add SECURITY.md and a private contact.**

## Reporting Bugs & Requesting Features

Open an issue with:

- **Bugs:** what you expected, what happened, a minimal reproduction, platform/browser/version, and any relevant `aegis.error` / decision output (with content redacted).
- **Features:** the problem you're trying to solve and why; proposed behavior; alternatives considered.

Please redact any sensitive data from logs and screenshots before attaching them.

## Pull Request Process

A PR is ready for review when:

- [ ] It addresses one logical change and links its issue.
- [ ] Tests are added/updated and the full suite passes.
- [ ] Lint and formatting pass.
- [ ] Documentation is updated in the same PR.
- [ ] Public API / config-schema changes were discussed and include migration notes.
- [ ] Privacy & safety requirements are upheld (and the child-safety requirement, for modules).
- [ ] Commits follow Conventional Commits.

Maintainers review for correctness, architecture fit, safety/privacy, performance budgets, and test coverage. At least one maintainer approval is required to merge; module and security-sensitive PRs require additional review. Expect iteration — review comments are about the change, not about you.

## Licensing of Contributions

By contributing, you agree that your contributions are licensed under the project's license. **TODO: confirm the license and whether a CLA or DCO sign-off (`Signed-off-by:` via `git commit -s`) is required.** Do not submit code, data, or model weights you don't have the rights to contribute — this applies especially to module training data.

---

Thank you for helping build a safer, more transparent open internet. Questions that this guide doesn't answer belong in [Discussions](https://github.com/AEGIS-INFRA/aegis-core/discussions) or the [FAQ](https://github.com/AEGIS-INFRA/.github/wiki/FAQ-General).
