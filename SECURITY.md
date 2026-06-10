# Security Policy

The security of AEGIS matters because AEGIS is a safety system: people rely on it to protect them — and especially children — from harmful, deceptive, and malicious advertising. A vulnerability here can undermine that protection or compromise the privacy AEGIS is built to preserve. We take reports seriously and appreciate responsible disclosure.

> **Pre-release status.** AEGIS is under active development and most repositories are currently private. Contacts and tooling below are being established and are marked **TODO**.

---

## Reporting a Vulnerability

**Please do not open a public issue, pull request, or discussion for security vulnerabilities.** Public disclosure before a fix puts users at risk.

Report privately through one of:

- **GitHub Private Vulnerability Reporting** — use the **"Report a vulnerability"** button under the *Security* tab of the affected repository (e.g. [`aegis-core`](https://github.com/AEGIS-INFRA/aegis-core/security)). This is the preferred channel.
- **Email** — `security@<aegisinfra.com>`.

You should receive an acknowledgment within **3 business days** *(target; TODO confirm)*. If you don't, please follow up.

## What to Include

A good report helps us triage fast. Where possible, include:

- A clear description of the vulnerability and its **impact**.
- The affected **repository, component, and version/commit**.
- **Steps to reproduce** or a minimal proof-of-concept.
- The **environment** (platform, browser/OS, device tier).
- Any logs or output — **with personal data and creative content redacted**.

Please do not include real personal data, real harmful creatives, or any content that could constitute illegal material (see [Important: Illegal Content](#important-illegal-content)).

## Coordinated Disclosure

We follow coordinated disclosure:

1. **Acknowledge** your report and begin triage.
2. **Assess** severity and scope; reproduce the issue.
3. **Develop and test** a fix (and a module/key rotation if integrity is affected).
4. **Release** the fix and, where relevant, a security advisory and CVE.
5. **Publicly credit** you, if you wish, after the fix ships.

Please give us a reasonable window — our target is up to **90 days** from acknowledgment to public disclosure — and coordinate timing with us before going public. We're happy to keep you updated throughout.

## Scope

AEGIS is a privacy-first, local-first safety SDK with signed, sandboxed detection modules and a separated scoring/policy pipeline. Vulnerabilities that matter most are those that defeat those guarantees.

### In scope

- **Module integrity bypass** — defeating bundle signature or SHA-256 verification; getting a tampered or unsigned module to execute; escaping the `QUARANTINED` state.
- **Module sandbox escape** — a module gaining network access, DOM access, or shared state outside its Web Worker, contrary to the `execute()` contract.
- **Privacy leaks** — exfiltration of raw creative content, screenshots, browsing data, or personal data; telemetry that includes content despite `includeContent: false`; introduction of stable cross-site identifiers; leakage of child-identifying data under `child_safe`.
- **Policy / enforcement bypass** — causing harmful content to be `allow`ed when policy dictates otherwise; locally overriding remote enterprise policy or a blocked category; allowlist abuse to defeat an org-mandated block.
- **Supply-chain issues** — malicious or compromised dependencies, build artifacts, or distributed module bundles.
- **Memory safety / DoS / crashes** in the engine, SDK, or extension, including crafted creatives that crash or hang the host or starve other modules.
- **Credential/key handling** flaws in the optional cloud layer (API key exposure, auth bypass, insecure transport).

### Generally out of scope

- **Detection quality** — ordinary false positives or false negatives, and non-crashing adversarial evasion of a model, are **quality issues**: please file them as normal issues, not security reports. (Adversarial input that causes a crash, RCE, or data leak **is** in scope.)
- Vulnerabilities only reachable in private development infrastructure not shipped to users.
- Missing security headers or best practices **without** a demonstrated impact.
- Self-XSS, social engineering, or physical attacks.
- Reports generated solely by automated scanners without a demonstrated, reproducible impact.

If you're unsure whether something is in scope, report it privately and let us decide.

## Supported Versions

AEGIS is pre-release; there is no supported stable release yet. Once releases begin, this section will list the versions that receive security updates. **TODO: populate on first release.**

| Version | Supported |
|---------|-----------|
| pre-release (`main`) | Best-effort |

## Safe Harbor

We support good-faith security research. If you make a genuine effort to comply with this policy, we will:

- Consider your research authorized and will not pursue or support legal action against you for it.
- Work with you to understand and resolve the issue promptly.

To stay within safe harbor, please: only test against systems/accounts you own or are explicitly permitted to test; **never access, modify, or exfiltrate data that isn't yours**; avoid privacy violations, service degradation, and data destruction; and never, under any circumstances, collect, generate, or transmit illegal content while testing (see below). Stop and report once you've confirmed a vulnerability.

## Important: Illegal Content

AEGIS includes detectors for adult and exploitative content. **Never** use real child sexual abuse material (CSAM) or other illegal content to test or demonstrate a vulnerability. If you encounter such material in any AEGIS-related context, do not download, copy, or attach it — report it immediately to **NCMEC** (USA, `report.cybertip.org`) or the appropriate national authority, and notify us through the private channel without including the material. This obligation overrides everything else in this policy.

## Recognition

With your permission, we're glad to credit reporters in our advisories and a security acknowledgments list once established. **TODO: acknowledgments page.**

---

Thank you for helping keep AEGIS and the people it protects  safe.
