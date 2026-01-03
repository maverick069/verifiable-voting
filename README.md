# Verifiable Voting Framework: India-Scale Empathy-Constant

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

A low-cost, end-to-end verifiable voting system designed for massive-scale elections (e.g., India's 900M+ voters) with a focus on empathy in design—prioritizing accessibility, privacy, and trust without complexity. It ensures ballot secrecy, voter-verifiable inclusion, and resistance to manipulation through cryptographic proofs, modular hardware, and transparent processes. "Empathy-Constant" emphasizes constant-time operations, eco-minimal paper trails, and user-friendly interfaces to reduce voter stress and errors, while making large-scale fraud logistically hard and statistically obvious.

## Overview

This repository hosts the master specification for a verifiable voting framework that scales to national levels while remaining "paper-quiet by default." Voters get inclusion receipts without any ability to prove how they voted. The public verifies tallies and batch integrity without trusting operators.

Key features:
- **End-to-End Verifiability**: Voters confirm their vote is included via hashes; public ledger allows anyone to audit counts.
- **Privacy-Preserving**: Receipt-freeness (no proof of choice possible); no linkable timestamps or side-channels.
- **Scalable & Low-Cost**: Reusable modules (~$10–13 BOM), offline-tolerant, power-frugal for low/middle-income countries.
- **Security**: Face-embedded, election-local voterhashes, VRF-governed timing, Merkle chaining, and threshold signatures from a multi-institution consortium.
- **Accessibility**: Multiple SKUs (audio, visual-plus, haptic, off-grid) with identical exteriors; constant 60s cast time.
- **Eco-Minimal Paper Trail**: Only designated booths print batch anchor slips (Merkle root QR/short ID, prev root, counts).
- **Cacophony Defense**: Randomized haptic call-up (HMC) on a limited pool of modules collapses efficient booth capture into one-human-one-module impersonation, making wholesale fraud a visible, high-cost operation.

The system resists powerful incumbents via statutory tripwires, real-time transparency, and multi-party oversight. Excludes remote/internet voting; postal logistics can be verified under the same rules but are out of scope here.

For the full spec, download  
**[Master_Spec_Verifiable_Voting_v1.4.1_2026-01-03_CC-BY-4.0.pdf](https://github.com/maverick069/verifiable-voting/blob/main/Master_Spec_Verifiable_Voting_v1.4.1_2025-01-03_CC-BY-4.0.pdf)**

*(Update the filename/path above to match the actual PDF in your repo.)*

## Key Ideas

- **Hardware**: Voter Machine (wheel + one hooded slot), booth-bound reusable modules (with sealed haptic for HMC), charging/UV rack with N = 3 × M_v modules.
- **Eligibility & One-Human-One-Vote**: At check-in, a one-time facial capture is converted to a quantized embedding and hashed with an election-secret key and electionId to form an election-local voterhash. A duplicate index prevents the same human body from voting multiple times across booths in the same election, without storing raw biometrics on the public ledger.
- **Receipts**: Voterhash + votehash (printed or digital), used for inclusion check—no choice revealed; receipt-freeness is preserved.
- **Ledger**: Public Bulletin Board with chained Merkle roots, threshold-signed by a consortium. Prevents operator discretion via **VRF-Governed Sliding-Window Cadence (SWC)**.
- **Flow Control & Cacophony Defense**: Haptic-Module Call-Up (HMC) uses VRF-jittered windows to make each module buzz at randomized times; attempts to batch-process many votes with a small colluding team degrade into logistical “cacophony” (only one human can practically service one buzzing module at a time).
- **Process**: Voter authorization at check-in; eligibility (face capture + voterhash check); pick module; HMC call-up to booth; vote via wheel; confirm (2s hold); receipt issued; module resets.
- **Audits**: Risk-Limiting Audits (RLA), attestation, telemetry tripwires (e.g., wait times, outliers, anomalously “perfect” flow).

## System Components

### Hardware
- **Reusable Voter Module**: Core unit with LOCK→VOTE key, secure element (SE), sealed low-amplitude LRA haptic (for HMC), pogo/USB-C charging. Booth-bound; rebind requires jig and triggers 7-day lockout + public event.
- **Voting Interface**: Knurled wheel rotates candidates into shrouded bracket; 2s confirm hold. Fixed 60s cast time (constant-time).
- **Charging/Disinfection Rack**: Holds modules; UV-C cleaning; health telemetry only (no data path, no casting mode).
- **Accessibility Variants**: Audio (beeps), Visual-Plus (high contrast), Haptic, Off-Grid (PV/hand-crank). Identical enclosures for privacy; timing and cues standardized to preserve anonymity.
- **Test Mode**: Voter-facing, constant-time (60s), no hashes generated; public counter only; used to spot-check hardware without affecting the ledger.

### Cryptographic Elements
- **Voterhash**:  
  `voterhash = H(election_secret_key || quantized_face_embedding || electionId)`  
  – Computed inside a secure element from a one-time facial capture at eligibility.  
  – Unique per human body *per election*; not reusable across elections; not linkable to civil identity from the public ledger alone.  
  – Raw images and embeddings are not written to the public ledger.
- **Votehash**:  
  `votehash = H(voterhash, selection, device-secret, window)`  
  – Computed in the module during CAST; does not reveal choice without internal secrets.
- **Batch Record**:  
  `{counts per candidate, voters:{voterhash...}, votes:{votehash...}}`, randomized order, VRF proof, Merkle root chaining.
- **VRF-SWC**: Selects batch interval from a ±2 window over 5-minute centers using `prevRoot + public beacon`; zero operator choice; optional publication jitter.

### Security Properties

|                Property               |                   Description                   |
|---------------------------------------|-------------------------------------------------|
| SP-1 Secrecy/Receipt-Freeness         | No voter can prove choice, even to themselves.  |
| SP-2 Inclusion                        | Voter verifies receipt hashes in public ledger. |
| SP-3 Uniqueness                       | One counted vote per authorization/voterhash.   |
| SP-4 Eligibility Separation           | Check-in handles ID/face; machine only casts.   |
| SP-5 Append-Only Record               | Public ledger with chaining and signatures.     |
| SP-6 Constant-Time/Anti-Side-Channels | Fixed 60s cast; shrouded, uniform inputs.       |
| SP-7 Accessibility Parity             | All SKUs equivalent in function and time.       |
| SP-8 VRF Verifiability                | Public proof of timing fairness (SWC).          |
| SP-9 Booth-Binding Auditability       | Tamper-evident rebinds on public log.           |
| SP-10 Cacophony Defense               | Randomized haptic call-up destroys parallel booth capture; one human can practically service only one buzzing module at a time. |
| SP-11 One-Human-One-Vote Integrity    | Election-local voterhash from facial embedding + duplicate index makes multi-booth impersonation statistically and logistically hard at scale. |

### Spec delta — Paper Trail (Designated Booths Only)
- **Scope**: Only booths pre-earmarked for paper trail produce physical artifacts. All others remain paper-quiet.
- **When to print**: At each batch close (per SWC rules) in an earmarked booth.
- **What to print** (on one small slip):
  - Batch Merkle Root (full) + short ID + optional QR.
  - Prev Root (short) to show chaining.
  - Counts per candidate for this batch (no vote/voter hashes).
  - Header fields: Election ID, Constituency ID, Booth ID, Voter Machine ID, Batch ID, Window Label (SWC).
  - Two officer signatures + one observer signature.
  - **What never appears on paper**: No votehash list, no voterhash list, no per-ballot timestamps, no mappings of votehash → candidate.
- **Chain-of-custody**: Affix slips to a single Daily Audit Sheet or store sequentially in a sealed envelope; record slip count in the booth log.
- **Verification**: Anyone can scan the root QR or type the short ID to check it matches the public ledger; counts must match the batch’s posted proofs.

### Operational Aspects
- **Capacity**: 1 vote/min per module; provision 3× max voters/hour; seating sized to absorb HMC jitter.
- **Telemetry**: Privacy-preserving metrics (uptime, queues, attestation, HMC expiries, gate passes); tripwires for outliers trigger fallback or surge deployment.
- **Paper Trail**: Eco-minimal—batch-close slips in designated booths: root (QR/full + short ID), prev short, counts, signatures. Sealed; verifiable against ledger.
- **Governance**: Consortium (EC, opposition, courts, universities); statutory requirements for transparency, RLAs, VRF beacon, threshold-held election_secret_key.
- **Audits/Acceptance**: Pre-election red-teaming; day-of metrics (99.95% attestation, <20–30m waits); post-RLA + crypto checks.
- **Cost**: ~$10–13/module core; ~$3 off-grid add; reusable 5–7 years.
- **Rollout**: Phased pilot → scale-up → nationwide with bug bounties and open verifiers.

## Diagrams

The spec includes stroke-only B&W diagrams for compatibility:

- **Reusable Voter Module**: Circular key with arrow (LOCK to VOTE), battery LEDs (dots), beep indicator, sealed haptic.
- **Voter Machine**: Rectangular slot + hood; circular wheel labeled; bracket with "Confirm" above.
- **Charging/UV Rack**: Slanted slots for modules, labeled Machine A/B; cast-return flow.
- **Booth Layout**: Pooled modules; voter path: eligibility → pick → call-up → cast → return; rack shows health.

For visuals, see the diagram pages in the PDF.

## Installation/Setup

(Coming soon—prototype code for simulations in Python/Rust. For now, review the spec PDF and test vectors.)

## Usage

1. **Voter Flow**: Check-in → eligibility (ID + facial capture) → authorization → pick module from pool → wait for haptic call-up → insert/rotate/confirm → receipt → return module.
2. **Verification**: Use any open verifier or public ledger API to search for `voterhash`/`votehash` from the receipt and confirm inclusion.
3. **Audit Example**: Compare printed slip root (designated booths) to ledger; recompute tallies from published batch records; run RLAs on samples.

## Contributing

Welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines. Focus on prototypes, simulations, verifiers, or spec refinements. Follow code style: Python (PEP8), Rust (clippy), and keep changes spec-aligned (v1.4.x).

## License

Creative Commons CC BY 4.0 — reuse with attribution:  
**"Gopanna, M. (primary), & GPT-5 Thinking (co-author). Master Specification — India-Scale End-to-End Verifiable Voting (Wheel + One-Slot + Reusable Module), v1.4.1, 2026-01-03."**

## Citation

Gopanna, M. (primary), & GPT-5 Thinking (co-author).  
*"Master Specification — India-Scale End-to-End Verifiable Voting (Wheel + One-Slot + Reusable Module)", v1.4.1, 2026-01-03.*  
License: CC BY 4.0.

## Notes

- Diagrams are vector stroke-only for mobile compatibility; avoid fills.
- Optional PV/hand-crank SKUs for power-scarce areas; default omits for cost/simplicity.
- Change Log: See Appendix K in PDF for version history.

## Roles

System architecture and core ideas by **Madhusudan Gopanna**.  
Drafting and flow/logic assistance by **GPT-5 Thinking** (collaborative AI co-author).  
Adversarial review / Red Team by **Gemini 3 Pro**.
