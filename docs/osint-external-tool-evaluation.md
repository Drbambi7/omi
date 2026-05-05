# OSINT External Tool Evaluation Guide

> **Scope:** This guide covers the safe, defensive-only evaluation of external OSINT (Open-Source Intelligence) repositories. It does **not** authorize or describe offensive scraping, credential harvesting, evasion techniques, or any action that targets real systems or individuals without explicit authorization.

---

## Purpose

Omi developers and security researchers may occasionally need to assess whether an external OSINT tool is suitable for inclusion as a reference, integration, or educational resource. This document establishes a repeatable, security-compliant process for performing that assessment without running harmful code or violating applicable laws.

This guide is intentionally **read-only and advisory**. The output of the evaluation process is a documented decision (reject / further review / allow as isolated reference) — not a deployment action.

---

## Safety Boundaries

All evaluations must stay within the following hard limits:

| Boundary | Rule |
|---|---|
| **Target systems** | Never run scraping or data-collection code against real websites, services, or users without explicit written permission. |
| **Credential handling** | Never execute code that harvests, stores, or transmits authentication tokens, passwords, or session cookies. |
| **Dark-web / Tor access** | Do not route evaluation traffic through anonymisation networks. |
| **Evasion** | Do not test or enable rate-limit bypasses, CAPTCHA solvers, or fingerprint-spoofing features. |
| **Automation against live targets** | Do not connect automated scrapers to production endpoints, even for testing purposes. |
| **Legal jurisdiction** | Abide by the Computer Fraud and Abuse Act (US), the Computer Misuse Act (UK), GDPR (EU), and equivalent local legislation. |
| **Data retention** | Any sample data gathered during a sandboxed review must be deleted immediately after the evaluation. |

---

## How to Review an External Repository Safely

Follow these steps in order. Do not skip steps.

### Step 1 — Initial triage (no code execution)

1. Open the repository in a browser. Do not clone yet.
2. Read the README, licence file, and any `SECURITY.md` or `CONTRIBUTING.md`.
3. Answer the questions in the **Legal/Ethical Checklist** below. If any answer is "No" or "Unclear", stop and escalate to a senior maintainer before proceeding.

### Step 2 — Static code review (no code execution)

1. Clone the repository into an isolated, offline-capable directory:
   ```
   git clone --depth 1 <repo-url> /tmp/osint-review/<repo-name>
   ```
2. Review the dependency manifest (`requirements.txt`, `package.json`, `go.mod`, etc.) using the **Dependency Review Checklist**.
3. Search for patterns that indicate harmful behaviour:
   - Hardcoded credentials or API keys
   - `subprocess`, `exec`, `eval` calls with un-sanitised input
   - Network calls to `.onion` addresses
   - Code that reads or writes the system keychain / credential store
   - Obfuscated strings (`base64`, `rot13`, hex blobs)
4. Document every finding in a review note (plain text or internal ticket). Do not commit findings to a public branch.

### Step 3 — Sandboxed dynamic evaluation (optional, gated)

Only proceed if Step 1 and Step 2 produce no blocking findings **and** a senior maintainer has approved in writing.

Use the **Sandbox Checklist** below to verify the environment before running any code.

---

## Sandbox Checklist

Before executing any code from an external OSINT tool, confirm every item:

- [ ] The evaluation machine is a **dedicated VM or container** with no access to production credentials, Firestore, Firebase, or any Omi backend service.
- [ ] All outbound network access is **blocked by default**; only explicitly whitelisted local mock endpoints are reachable.
- [ ] The host OS has **no saved browser sessions, SSH keys, or API tokens** in the environment.
- [ ] A **snapshot / checkpoint** of the VM has been taken before execution so the environment can be rolled back.
- [ ] Execution is logged to a local file and **not** streamed to an external service.
- [ ] The sandbox is **not** connected to a corporate VPN or any internal network segment.
- [ ] A second reviewer is present or has been notified and can observe the session.
- [ ] A time-boxed session limit (e.g., 60 minutes) has been set; the VM will be destroyed afterwards.

---

## Dependency Review Checklist

For each dependency declared by the external tool:

- [ ] The package exists on an official registry (PyPI, npm, crates.io, etc.) and is not a typosquat.
- [ ] The declared version has no known critical CVEs (check [osv.dev](https://osv.dev) or `pip-audit` / `npm audit`).
- [ ] The package does not pull in sub-dependencies that are flagged by the registry's security advisories.
- [ ] No dependency overrides pinned versions with `--trusted-host`, `--extra-index-url`, or equivalent bypass flags.
- [ ] Post-install scripts (`postinstall`, `setup.py` hooks) have been reviewed and do not make network calls.



---

## Legal / Ethical Checklist

- [ ] The tool's license is compatible with Omi's MIT license (if integration is being considered).
- [ ] The tool does not instruct users to violate terms of service of third-party platforms.
- [ ] The tool does not facilitate scraping of personal data without consent (GDPR Article 6 / CCPA).
- [ ] The tool has a clear, documented, defensive-research use case stated by its maintainers.
- [ ] There is no evidence in issues, PRs, or commit history that the tool has been used for malicious purposes by its authors.
- [ ] Using or referencing the tool will not expose Omi or its contributors to reputational, legal, or regulatory risk.

---

## Decision Matrix

After completing all applicable checklists, record a decision using the following matrix:

| Outcome | Criteria | Next Action |
|---|---|---|
| **Reject** | Any Legal/Ethical item is "No"; or a blocking dependency CVE is found; or harmful code patterns are identified in Step 2. | Document findings. Do not integrate or reference the tool. Archive the review note internally. |
| **Further Review** | All Legal/Ethical items pass, but Step 2 reveals ambiguous patterns that require expert analysis (e.g., obfuscated code, unclear network behaviour). | Escalate to a security reviewer. Do not proceed to Step 3 until cleared. |
| **Allow as isolated external reference** | All checklists pass; the tool demonstrates clear defensive value (e.g., educational example, dependency audit helper); no integration into Omi code is required. | Document the approval. Link to the repository from relevant internal documentation **without** embedding or executing any of its code in the Omi codebase. |

> **Integration is never an automatic outcome of this process.** Even a tool that passes all checklists requires a separate architectural review and maintainer approval before any code is added to the Omi repository.

---

## Example: Evaluating `robin_scrapper`

The repository `https://github.com/tekcin/robin_scrapper` has been cited as an example external OSINT tool. The evaluation process above was followed as a read-only exercise. The outcome is:

**Reject / No integration.**

Rationale: Omi is a personal AI assistant focused on transcription, memory, and productivity. Web-scraping infrastructure is outside Omi's threat model and product scope. Even if the tool passed all checklists, there is no architectural justification for integrating it. The repository may serve as a *conceptual reference* for developers studying OSINT methodologies in their own sandboxed environments, but it must not be cloned into, imported by, or executed as part of any Omi service.

---

## References

- [OWASP Testing Guide — Information Gathering](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/01-Information_Gathering/)
- [osv.dev — Open Source Vulnerability Database](https://osv.dev)
- [SANS Institute — Legal Issues in Penetration Testing](https://www.sans.org/white-papers/legal-issues-pen-testing/)
- [Omi Docs Index](INDEX.md)
