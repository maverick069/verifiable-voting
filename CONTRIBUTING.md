# Contributing to Verifiable Voting (Wheel + One-Slot + Reusable Module)

First off—thank you. This project exists to help any lawful election authority deploy **end-to-end verifiable, privacy-preserving** voting that’s practical for low- and middle-income countries. Contributions must never weaken security, auditability, or voter privacy.

This document explains **how** to propose changes, the **standards** we enforce, and the **process** we follow.

---

## 0) Fast Start

- **Issues:** Use the templates under `.github/ISSUE_TEMPLATE`.
- **Small fixes/docs:** Open a PR to `main` with a clear title + checklist below.
- **Specs/crypto/hardware changes:** Open an issue first for discussion; PR only after a **maintainer green-light**.
- **Security bugs:** Do **not** open a public issue. Follow **Responsible Disclosure** below.

---

## 1) Project Scope (what belongs here)

- **In-scope:** master spec, reference diagrams, verifier CLI prototypes, test vectors, ops guidance, audit & RLA procedures, threat modeling, accessibility flows, cost models, i18n scaffolds, and reproducibility tooling.
- **Out-of-scope:** partisan content, country-specific political advocacy, remote/Internet voting, features that enable **receipt-proving** or weaken **ballot secrecy**.

---

## 2) Ground Rules (must not break)

- **Secrecy / Receipt-freeness:** No contribution may let a voter prove how they voted.
- **Inclusion verifiability:** Every voter must be able to confirm their ballot is included **without** revealing their choice.
- **Append-only / Auditability:** Public bulletin board semantics and tamper evidence must remain verifiable by third parties.
- **Operator non-discretion:** No change may re-introduce manual timing/control where the spec mandates VRF-governed behavior.
- **Accessibility parity:** Assistive SKUs must not create signature-able differences that deanonymize voters.
- **Jurisdiction neutrality:** Keep designs portable; country-specific notes go to `/docs/regional-notes/`.

---

## 3) Security & Cryptography Contributions

> Crypto changes require explicit sign-off from the Lead Architect or a designated Cryptography Reviewer.

- **No home-rolled crypto.** Use well-vetted primitives; justify parameters and cite references.
- **VRF & Merkle invariants:** Must match the spec (seed sources, linkability rules, batch header proofs).
- **Threat model delta:** PR must include a short “Threat Model Impact” note: new assumptions, new attacker capabilities, mitigations.
- **Test vectors:** Add cross-checked vectors under `tests/vectors/` (deterministic, reproducible).
- **Constant-time claims:** Back with micro-bench notes or a pointer to a reproducible check.
- Red Teaming: We actively invite cryptographers and security researchers to attempt to break the 'VRF-Governed Sliding Window' or the 'Receipt-Freeness' properties. Please file these findings under Responsible Disclosure.

---

## 4) Hardware & UX Contributions

- Keep **stroke-only** diagrams (no fills) for legibility and mobile-friendly PDFs.
- Label figures clearly; avoid per-candidate cues or UI asymmetries.
- When proposing SKU changes, include **BOM deltas**, **power envelope**, **attestation path**, and **tamper responses**.
- Provide **manufacturability notes** and, where possible, a cost band at national scale.

---

## 5) Documentation Standards

- **Spec edits** follow the section numbering and headings already in use.
- Update **Appendix AB — Change Log** with a succinct entry (version bump + dated).
- Add citations and keep normative language clear: **MUST / MUST NOT / SHOULD / MAY**.
- PDFs: A4 and US-Letter variants should be produced together when the spec changes.

---

## 6) Code Style & Tooling

- **Languages:** Prefer Python for verifier prototypes and tooling. Shell scripts should be POSIX-compatible.
- **Formatting:** `black` + `ruff` for Python; `shfmt` for shell.
- **Typing:** Python code uses `mypy` (strict where feasible).
- **Commits:** Conventional Commits (e.g., `feat: …`, `fix: …`, `docs: …`, `spec: …`, `crypto: …`, `hw: …`).
- **Branching:** `feature/<topic>`; keep PRs focused and < ~600 LOC if possible.

---

## 7) Tests & Reproducibility

- **Deterministic tests** only; pin seeds where randomness is used for tests.
- Include **end-to-end checks** for:
  - dual-hash receipt (`voterhash`, `votehash`) set semantics,
  - batch record shape & Merkle chaining,
  - VRF proof verification and sliding-window cadence,
  - inclusion verification without choice revelation.
- No PII in tests. Use generated synthetic rolls and fixtures.

---

## 8) Internationalization & Accessibility

- English is the reference language. Add localizations under `i18n/<locale>/`.
- Provide text-only alternatives for figures when applicable.
- Any change affecting assisted voting must document **parity analysis** (why it does not leak identity/choice).

---

## 9) Legal & Licensing

- **Docs/specs/diagrams:** **CC BY 4.0** (attribution: *Gopanna, M. (primary), & GPT-5 Thinking (co-author). Master Specification — India-Scale End-to-End Verifiable Voting (Wheel + One-Slot + Reusable Module), v1.3.1, 2025-11-12.*).
- **Code:** If present, prefer **Apache-2.0** (state clearly in file headers). If you contribute code under a different permissive license (MIT/BSD-3), note it in the PR.
- Ensure third-party assets are compatible; include LICENSE notices as needed.

---

## 10) PR Checklist (paste into your PR)

- [ ] Scope: This change is in-scope for the project.
- [ ] Security: Does **not** weaken secrecy, receipt-freeness, or append-only guarantees.
- [ ] Threat model impact documented.
- [ ] Tests: New/updated deterministic tests added; vectors under `tests/vectors/` where applicable.
- [ ] Docs: Spec updated and **Appendix AB** change log entry added.
- [ ] PDFs: A4 + Letter rebuilt (if spec text/figures changed).
- [ ] Licensing headers present; third-party attributions included.
- [ ] I have read and comply with Responsible Disclosure below.

---

## 11) Responsible Disclosure (Security)

If you believe you have found a vulnerability or a way to undermine ballot secrecy, inclusion, or append-only guarantees:

1. **Do not file a public issue.**
2. Use Github's Private Vulnerability Reporting. Learn more at https://docs.github.com/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability
3. We will acknowledge within **72 hours**, provide a remediation timeline, and credit you upon coordinated disclosure (or keep you anonymous upon request).

---

## 12) Governance & Decision Process

- Maintainers act by **rough consensus and running code**.
- **Spec-level changes** require: open design issue → discussion → maintainer approval → PR with tests/doc rebuild → version bump and change log entry.
- **Breaking changes** (affecting invariants or interfaces) require two maintainers and a 5-day comment window unless an urgent security fix.

---

## 13) Versioning

- The spec uses **calendar-anchored semantic tags** (e.g., `v1.3.1_2025-11-12`).
- Any change altering security properties, batch format, or verification logic requires a **minor** bump at minimum.

---

## 14) Community Conduct

- Be rigorous and kind. No harassment, political campaigning, or ad hominems.
- Debate ideas, not people. Be explicit about assumptions and trade-offs.

---

## 15) Attribution

Cite the work as:

> Gopanna, M. (primary), & GPT-5 Thinking (co-author). *Master Specification — India-Scale End-to-End Verifiable Voting (Wheel + One-Slot + Reusable Module)*, v1.x.y, 2025-11-09+. CC BY 4.0.

---

*Thank you for helping build auditable democracy technology without compromising privacy or practicality.*
