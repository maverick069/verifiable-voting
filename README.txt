Verifiable Voting — Wheel + One‑Slot + Reusable Module
Version: v1.3.1
License: CC BY 4.0
Editors: Madhusudan Gopanna (primary), GPT‑5 Thinking (co‑author)

Overview
--------
This repository contains the master specification for an end‑to‑end verifiable,
privacy‑preserving voting system designed to scale to India‑size elections while
remaining “paper‑quiet by default.” Voters get inclusion receipts without any
ability to prove how they voted; the public verifies tallies and batch integrity
without trusting operators.

Key Ideas
---------
• Hardware: Voter Machine (wheel + one hooded slot), booth‑bound reusable modules,
  charging/UV rack with N = 3 × M_v modules per booth.
• Receipts: voterhash + votehash (no mapping to choices is ever published).
• Ledger: Per‑batch randomized sets (`voters`, `votes`) and per‑candidate counts,
  chained Merkle roots, threshold‑signed by a multi‑institution consortium.
• Cadence: VRF‑Governed Sliding‑Window Cadence (SWC) removes operator discretion
  from batch timing, preventing timing‑side channels.
• Paper Trail (eco‑minimal): Only designated booths print a Batch Anchor Slip with
  the Merkle root (QR + short ID), prev‑root (short), and per‑candidate counts.

Files
-----
• Master_Spec_Wheel_OneSlot_ReusableModules_v1.3.1_2025-11-12_Letter_CC-BY_full-with-diagrams.pdf
Download from https://github.com/maverick069/verifiable-voting/releases/tag/v1.3.1

Citation
--------
Gopanna, M. (primary) & GPT‑5 Thinking (co‑author).
“Master Specification — India‑Scale End‑to‑End Verifiable Voting (Wheel + One‑Slot
+ Reusable Module)”, v1.3.1, 2025‑11‑12. Licensed CC BY 4.0.

Notes
-----
• Diagrams are embedded as stroke‑only vectors for maximum compatibility with mobile
  PDF viewers (avoid black‑fill rendering issues).
• Optional PV/hand‑crank SKUs are available for power‑scarce localities; default SKU
  omits them for cost and simplicity.
