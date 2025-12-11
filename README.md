# Verifiable Voting Framework: India-Scale Empathy-Constant

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

A low-cost, end-to-end verifiable voting system designed for massive-scale elections (e.g., India's 900M+ voters) with a focus on empathy in design—prioritizing accessibility, privacy, and trust without complexity. It ensures ballot secrecy, voter-verifiable inclusion, and resistance to manipulation through cryptographic proofs, modular hardware, and transparent processes. "Empathy-Constant" emphasizes constant-time operations, eco-minimal paper trails, and user-friendly interfaces to reduce voter stress and errors.

## Overview

This repository hosts the master specification for a verifiable voting framework that scales to national levels while remaining "paper-quiet by default." Voters get inclusion receipts without any ability to prove how they voted. The public verifies tallies and batch integrity without trusting operators.

Key features:
- **End-to-End Verifiability**: Voters confirm their vote is included via hashes; public ledger allows anyone to audit counts.
- **Privacy-Preserving**: Receipt-freeness (no proof of choice possible); no linkable timestamps or side-channels.
- **Scalable & Low-Cost**: Reusable modules (~$10-13 BOM), offline-tolerant, power-frugal for low/middle-income countries.
- **Security**: VRF-governed timing, Merkle chaining, threshold signatures from a multi-institution consortium.
- **Accessibility**: Multiple SKUs (audio, visual-plus, haptic, off-grid) with identical exteriors; constant 60s cast time.
- **Eco-Minimal Paper Trail**: Only designated booths print batch anchor slips (Merkle root QR/short ID, prev root, counts).

The system resists powerful incumbents via statutory tripwires, real-time transparency, and multi-party oversight. Excludes remote/internet voting or postal logistics (though verifiable under same rules).

For the full spec, download [Master_Spec_Wheel_OneSlot_ReusableModules_v1.3.1_2025-11-12_Letter_CC-BY_full-with-diagrams.pdf](releases/tag/v1.3.1).

## Key Ideas

- **Hardware**: Voter Machine (wheel + one hooded slot), booth-bound reusable modules, charging/UV rack with N = 3 × M_v modules.
- **Receipts**: Voterhash + votehash (printed or digital), used for inclusion check—no choice revealed.
- **Ledger**: Public Bulletin Board with chained Merkle roots, threshold-signed by a consortium. Prevents operator discretion via VRF-Governed Sliding-Window Cadence (SWC).
- **Process**: Voter authorization at check-in; pick module, vote via wheel, confirm (2s hold); receipt issued; module resets.
- **Audits**: Risk-Limiting Audits (RLA), attestation, telemetry tripwires (e.g., wait times, outliers).

## System Components

### Hardware
- **Reusable Voter Module**: Core unit with LOCK→VOTE key, secure element (SE), pogo/USB-C charging. Booth-bound; rebind requires jig and triggers 7-day lockout + public event.
- **Voting Interface**: Knurled wheel rotates candidates into shrouded bracket; 2s confirm hold. Fixed 60s cast time.
- **Charging/Disinfection Rack**: Holds modules; UV-C cleaning; health telemetry only (no data path).
- **Accessibility Variants**: Audio (beeps), Visual-Plus (high contrast), Haptic, Off-Grid (PV/hand-crank). Identical enclosures for privacy.
- **Test Mode**: Voter-facing, constant-time (60s), no hashes generated; public counter.

### Cryptographic Elements
- **Voterhash**: H(minute-key, device-secret, window nonce)—not linkable to identity.
- **Votehash**: H(voterhash, selection, device-secret, window).
- **Batch Record**: {counts per candidate, voters:{voterhash...}, votes:{votehash...}}, randomized order, VRF proof, Merkle root chaining.
- **VRF-SWC**: Selects window over 5-min bins (±2 min) based on prevRoot + beacon; zero operator choice; optional publication jitter.

### Security Properties
|                Property               |                   Description                   |
|---------------------------------------|-------------------------------------------------|
| SP-1 Secrecy/Receipt-Freeness         | No voter can prove choice, even to themselves.  |
| SP-2 Inclusion                        | Voter verifies receipt hashes in public ledger. |
| SP-3 Uniqueness                       | One counted vote per authorization.             |
| SP-4 Eligibility Separation           | Check-in handles ID; machine only casts.        |
| SP-5 Append-Only Record               | Public ledger with chaining and signatures.     |
| SP-6 Constant-Time/Anti-Side-Channels | Fixed 60s cast; shrouded inputs.                |
| SP-7 Accessibility Parity             | All SKUs equivalent in function and time.       |
| SP-8 VRF Verifiability                | Public proof of timing fairness.                |
| SP-9 Booth-Binding Auditability       | Tamper-evident rebinds.                         |

### Operational Aspects
- **Capacity**: 1 vote/min per module; provision 3x max voters/hour.
- **Telemetry**: Privacy-preserving metrics (uptime, queues, attestation); tripwires for outliers trigger fallback.
- **Paper Trail**: Eco-minimal—batch-close slips in designated booths: root (QR/full + short ID), prev short, counts, signatures. Sealed; verifiable against ledger.
- **Governance**: Consortium (EC, opposition, courts, unis); statutory requirements for transparency, RLAs, VRF beacon.
- **Audits/Acceptance**: Pre-election red-teaming; day-of metrics (99.95% attestation, <20-30m waits); post-RLA + crypto checks.
- **Cost**: ~$10-13/module core; $3 off-grid add; reusable 5-7 years.
- **Rollout**: Phased pilot → scale-up → nationwide with bug bounties.

## Diagrams
The spec includes stroke-only B&W diagrams for compatibility:

- **Reusable Voter Module**: Circular key with arrow (LOCK to VOTE), battery LEDs (dots), beep indicator.
- **Voter Machine**: Rectangular slot + hood; circular wheel labeled; bracket with "Confirm" above.
- **Charging/UV Rack**: Slanted slots for modules, labeled Machine A/B; cast-return flow.
- **Booth Layout**: Pooled modules; voter path: pick → cast → return; rack shows health (no data).

For visuals, see page 5 of the PDF.

## Installation/Setup
(Coming soon—prototype code for simulations in Python/Rust. For now, review the spec PDF.)

## Usage
1. **Voter Flow**: Check-in → authorization → pick module → insert/rotate/confirm → receipt → return module.
2. **Verification**: Use public ledger API to search for voterhash/votehash.
3. **Audit Example**: Compare printed slip root to ledger; run RLA on samples.

## Contributing
Welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines. Focus on prototypes, simulations, or spec refinements. Follow code style: Python PEP8, Rust clippy.

## License
Creative Commons CC BY 4.0 — reuse with attribution: "Gopanna, M. (primary), & GPT-5 Thinking (co-author). Master Specification — India-Scale End-to-End Verifiable Voting (Wheel + One-Slot + Reusable Module), v1.3.1, 2025-11-12."

## Citation
Gopanna, M. (primary), & GPT-5 Thinking (co-author). "Master Specification — India-Scale End-to-End Verifiable Voting (Wheel + One-Slot + Reusable Module)", v1.3.1, 2025-11-12. License: CC BY 4.0.

## Notes
- Diagrams are vector stroke-only for mobile compatibility; avoid fills.
- Optional PV/hand-crank SKUs for power-scarce areas; default omits for cost/simplicity.
- Change Log: See Appendix AB in PDF for version history.
