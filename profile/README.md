<div align="center">

# AEGIS

**Ads Enforcement, Governance & Integrity System** |
***Open AI Safety Infrastructure for the Internet***

AEGIS is an open, privacy-first safety platform for detecting and moderating harmful, deceptive, and illegal advertising across the open web and mobile apps — built for developers, families, schools, and enterprises.

![status](https://img.shields.io/badge/status-pre--release-orange)
![model](https://img.shields.io/badge/AI-local--first-success)
![core](https://img.shields.io/badge/core-open--source-blue)
![platforms](https://img.shields.io/badge/platforms-Web%20%7C%20Android%20%7C%20iOS-blueviolet)
![license](https://img.shields.io/badge/license-TBD-lightgrey)

[Documentation Wiki](https://github.com/AEGIS-INFRA/.github/wiki) ·
[How It Works](https://github.com/AEGIS-INFRA/.github/wiki/How-It-Works) ·
[FAQ](https://github.com/AEGIS-INFRA/.github/wiki/FAQ-General)

</div>

---

## The problem

Illegal, deceptive, and harmful advertising is growing across the web and mobile ecosystem — scam investments, phishing, malware, exploitative content, fake health cures, counterfeit promotions, and AI-generated deceptive ads engineered to look legitimate.

Major platforms moderate aggressively, but only **inside their own walled gardens**. The moment a user steps onto an independent website, a mobile app, a gaming app, an embedded ad network, or a sponsored result, that protection disappears.

> **There is no universal, user-controlled AI safety layer for the open internet** that developers, platforms, schools, and families can directly integrate and manage.

AEGIS exists to build that layer — with transparency, privacy, and user control.

> **AEGIS is not an ad blocker.** Ad blockers hide ads from a blocklist. AEGIS *evaluates* advertising content and acts only on the genuinely harmful subset, leaving legitimate ads untouched.

## The AEGIS platform

AEGIS follows an **open-core** model: a transparent, auditable open-source foundation, with commercial and consumer products built on top.

| Product | What it is | Audience | Model |
|---------|------------|----------|-------|
| **AEGIS CORE** | The privacy-first AI safety **SDK + policy engine** developers embed in web, Android, and iOS apps. | Developers, platform operators | Open source |
| **AEGIS Family Protection** | A **browser extension** that brings child-safe ad protection and parental controls to families. | Parents, families | Free + premium |
| **AEGIS Enterprise** | Managed cloud, centralized monitoring dashboards, analytics, threat intelligence, and compliance tooling. | Enterprises, schools, platforms | Commercial |
| **AEGIS Modules** | The ecosystem of signed, versioned detection modules (scam, phishing, adult content, malware, …) the engine runs. | Module authors, researchers | Open + curated |

All four share one foundation: the same on-device detection → scoring → policy pipeline, the same explainable decisions, and the same privacy guarantees.

## How it works

Every product runs the same three-layer pipeline. A piece of ad content is detected and normalized, scored against harm-category modules, and resolved to a policy decision — all on-device by default.

```
CreativeExtractor  →  RiskScoringEngine  →  PolicyEngine  →  EnforcementAdapter
   (detect +            (modules + fusion     (thresholds +     (apply to the
    extract)             → risk score)         overrides →       rendered surface)
                                               binding action)
```

Each evaluation yields one of six explainable decisions: `allow`, `label`, `blur`, `block`, `escalate`, or `report` — each with a risk score, the categories detected, a confidence level, and a human-readable reason.

Read the full pipeline in [How It Works](https://github.com/AEGIS-INFRA/.github/wiki/How-It-Works), with deep-dives on [Ad Creative Extraction](https://github.com/AEGIS-INFRA/.github/wiki/Ad-Creative-Extraction), the [Risk Scoring Framework](https://github.com/AEGIS-INFRA/.github/wiki/Risk-Scoring-Framework), and the [Policy-Aware Engine](https://github.com/AEGIS-INFRA/.github/wiki/Policy-Aware-Engine).

## Who AEGIS is for

- **Developers & platform operators** — embed AEGIS CORE to protect users from harmful third-party ads, improve trust and safety, and meet regulatory expectations.
- **Parents & families** — install AEGIS Family Protection to keep scams, gambling-style "rewards," adult content, and malware away from children.
- **Schools & educational institutions** — safer browsing and research for students via the `student` and `child_safe` profiles, with centrally managed policy.
- **NGOs & regulators** — an auditable, open safety layer focused on digital safety, child protection, and AI accountability.
- **Enterprises** — protect employees from phishing and malicious sponsored links, with managed monitoring and compliance tooling.

## Getting started

**Developers — integrate AEGIS CORE**

```bash
npm install @aegis/core        # Web / JavaScript
```
```js
import { AegisClient } from "@aegis/core";
const aegis = AegisClient.init({ targetProfile: "general", protectionLevel: "block", localOnly: true });
const decision = await aegis.scanCreative({ title: "Win a free phone now", body: "Click to claim", landingUrl: "https://example.test/prize" });
// → { action: "block", riskScore: 0.82, categories: ["scam","deceptive_claim"], ... }
```
Android (`com.aegis:aegis-core:x.y.z`) and iOS (`https://github.com/AEGIS-INFRA/aegis-ios`) quick starts are in [Getting Started](https://github.com/AEGIS-INFRA/.github/wiki/Getting-Started). *Package coordinates are placeholders pending the first release.*

**Parents — install Family Protection**

Available as a Chrome-based extension (Phase 3). **TODO: add store link when published.**

**Enterprises & schools — managed deployment**

Centralized policy, monitoring, and compliance via AEGIS Enterprise. **TODO: add contact / sign-up link.**

## Repositories

> Repo names are placeholders under the `AEGIS-INFRA` org pending the first release.

| Repository | Contents | Visibility |
|------------|----------|-----------|
| `aegis-core` | Web/JavaScript SDK + policy engine | Open source |
| `aegis-android` | Android (Kotlin) SDK | Open source |
| `aegis-ios` | iOS (Swift) SDK | Open source |
| `aegis-family-extension` | Family Protection browser extension | Open source (free tier) |
| `aegis-modules` | Detection module registry + reference modules | Open + curated |
| `aegis-enterprise` | Managed cloud / monitoring services | Commercial |
| `.github` | Org profile + the documentation wiki | Public |

## Privacy & security

Privacy and security are platform-wide design constraints, not features:

- **Local-first.** Detection and scoring run on-device by default; no browsing data or creative content leaves the device unless you opt in to a cloud feature.
- **Opt-in, consent-gated cloud.** Remote reputation and monitoring require explicit configuration; raw content, screenshots, and personal data are never transmitted by default — and content inclusion is discouraged entirely for minors.
- **Signed, verified modules.** Detection modules are cryptographically signed and verified by SHA-256 before any code runs; a mismatch quarantines the module. Modules are sandboxed and cannot access the network or DOM.
- **Explainable by default.** Every decision is auditable — score, categories, confidence, and reason.

Details: [Metrics & Monitoring → Privacy](https://github.com/AEGIS-INFRA/.github/wiki/Metrics-and-Monitoring). To report a vulnerability, see [`SECURITY.md`](SECURITY.md). **TODO: add security policy and contact.**

## Documentation

The full documentation lives in the [AEGIS Wiki](https://github.com/AEGIS-INFRA/.github/wiki):

- [Home](https://github.com/AEGIS-INFRA/.github/wiki/Home) · [Getting Started](https://github.com/AEGIS-INFRA/.github/wiki/Getting-Started) · [How To Use](https://github.com/AEGIS-INFRA/.github/wiki/How-To-Use) · [Configuration](https://github.com/AEGIS-INFRA/.github/wiki/Configuration)
- [How It Works](https://github.com/AEGIS-INFRA/.github/wiki/How-It-Works) · [Ad Creative Extraction](https://github.com/AEGIS-INFRA/.github/wiki/Ad-Creative-Extraction) · [Risk Scoring Framework](https://github.com/AEGIS-INFRA/.github/wiki/Risk-Scoring-Framework) · [Policy-Aware Engine](https://github.com/AEGIS-INFRA/.github/wiki/Policy-Aware-Engine)
- [Preparing & Registering a Module](https://github.com/AEGIS-INFRA/.github/wiki/Preparing-and-Registering-a-Module) · [Metrics & Monitoring](https://github.com/AEGIS-INFRA/.github/wiki/Metrics-and-Monitoring) · [End-to-End Examples](https://github.com/AEGIS-INFRA/.github/wiki/End-to-End-Examples) · [FAQ](https://github.com/AEGIS-INFRA/.github/wiki/FAQ-General)

## Roadmap

- **Phase 1 — SDK foundation:** JavaScript web SDK, rule engine, lightweight text/image classifiers, local-first processing.
- **Phase 2 — Mobile SDKs:** Android and iOS, shared moderation engine, centralized configuration.
- **Phase 3 — Family Protection:** Chrome-based extension, child-safe mode, parental controls.
- **Future:** video moderation, federated feedback learning, telecom/browser integrations, enterprise infrastructure, AI trust & compliance tooling.

## Contributing

AEGIS is built in the open, and contributions, audits, and issue reports are welcome — code, detection modules, documentation, and research alike. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the workflow and [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) for community expectations.

## Governance & license

AEGIS follows an open-core model: the core SDK, policy engine, and reference modules are intended for release under an OSI-approved open-source license, with commercial layers built on top. **TODO: confirm license and governance model.**

---

<div align="center">

### Our vision

To build the open and trusted **safety infrastructure layer for the internet** — enabling developers, platforms, schools, and families to protect users from harmful, deceptive, and illegal content across web and mobile environments, with transparency, privacy, and user control.

</div>

