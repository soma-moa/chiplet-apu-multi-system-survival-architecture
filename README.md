# soma-moa: Chiplet-APU Multi-System Survival Architecture

> **Correction & Retraction Notice**
> This repository and notice retract statements of independent technical invention, defensive prior art, original IP ownership, inventorship, and unsubstantiated figures presented in previously published versions (such as v2.8.1, Zenodo DOI 10.5281/zenodo.22374987), serving as a revised overview for a literature survey project. This correction applies to documents within this repository that have completed revision (such as README.md, ARCHITECTURE_STRATEGY.md). Statements of original invention, prior art scope, IP ownership, inventorship, and unsubstantiated figures remaining in unrevised documents are also subject to retraction under this notice, and reliance on such statements is discouraged prior to revision completion. Previously published versions (v2.8.1) are retained solely as historical records, and this revised version supersedes all technical and literature interpretations. No claim of original invention or prior art scope is made; engineering credit belongs entirely to the researchers and standards bodies of the cited works.
> 
> Compiler: deundeuni
> Final Revision Date: 2026-10-04
> Official Repository: https://github.com/soma-moa/chiplet-apu-multi-system-survival-architecture | Official Domain: somamoa.ai.kr
> License: CC BY 4.0
> Language Authority Notice: In case of discrepancies between Korean and English translations, the Korean original serves as the reference, but the primary literature remains authoritative for technical interpretation.

---

This repository serves as a guide for a literature review project investigating prior research and engineering standards related to fault tolerance, fabric resiliency, and self-healing in multi-chiplet and APU-based computing systems.

📂 Project File Structure

* [README.md] : Project overview and revision notice (this document)
* [ARCHITECTURE_STRATEGY.md] : Modular survival architecture literature survey and structural synthesis (Revision Completed)
* [WHITEPAPER.md] : Technical whitepaper text (Revision Under Review)
* [LICENSE] : CC BY 4.0 single license specification
* [PHILOSOPHY.md] : Reference material (Revision Under Review)

🏗️ Full-Stack Layer Overview (Compiler's Analytical Taxonomy)

┌────────────────────────────────────────────────────────────────────────┐
│ [L3] Escalation & Notification Layer                                   │
│ - User and external administrator escalation interface (Compiler concept)│
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │ (Telemetry & Backpressure Signals)
┌──────────────────────────────────▼─────────────────────────────────────┐
│ [L2] Deterministic Governance & Control Layer                          │
│ - FSM state transition control and physical override latches (Bus Guardian)│
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │ (State & Control Signals)
┌──────────────────────────────────▼─────────────────────────────────────┐
│ [L1] Compute & Fabric Layer                                            │
│ - CPU / Bridge Chiplet (Compiler term: TL) / Compute Units / CXL Pools │
│ - Mesh fabric + Bypass paths (Compiler term: Shoulder) + Resource Governor│
│ - Random sampling anomaly detection (Compiler term: Leukocyte Scan) + Isolation│
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │ (High-Speed Signal & Power Bus)
┌──────────────────────────────────▼─────────────────────────────────────┐
│ [L0] Physical & Material Layer                                         │
│ - PMIC power-off control, high-speed retimers                          │
│ - Differential reduction docking & align (Compiler's prior work, CWP)   │
└────────────────────────────────────────────────────────────────────────┘

⚖️ License Policy

This repository and its curated materials are licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0). For details, refer to the LICENSE file in the repository root.

---

Sources & References

* Repository Document Status
  * Multi-Chiplet System Fault Tolerance Literature Survey (chiplet-apu-multi-system-survival-architecture) — GitHub: soma-moa | CERN Zenodo DOI: 10.5281/zenodo.22374987 (Revised into literature survey; earlier public version v2.8.1 preserved as historical record)
  * Modular Survival Architecture Literature Survey (ARCHITECTURE_STRATEGY) — Revised into literature survey
  * Emergency Evacuation Infrastructure (LAST-LIGHT) — GitHub: soma-moa / LAST-LIGHT | CERN Zenodo DOI: 10.5281/zenodo.22373189 (Revision Under Review)
  * Polar Marine Sacrificial Armor (MAX-LIFE-ICE-BELT) — GitHub: soma-moa / MAX-LIFE-ICE-BELT | CERN Zenodo DOI: 10.5281/zenodo.22373686 (Revision Under Review)
  * CWP Alignment & Docking Series (CWP-Entry, CWP-Battery-Swap, CWP-Clamping, CWP-Rolling-Self-Align) — GitHub: soma-moa (Revision Under Review)
  * Main Gateway Repository (soma-moa) — GitHub: soma-moa / soma-moa (Revision Under Review)

* Reference Engineering Standards
  * CXL 3.0 / UCIe Rev 1.0/1.1 / JEDEC HBM3 / HSA Foundation AQL Specification [Reference literature: memory-based titles, unverified]
  * CAN Bus (ISO 11898) / SPI / PCIe Gen6/7 Retimer Specification [Reference literature: memory-based titles, unverified]
  * ISO 13849-1 / IEC 61508 / ISO 26262 (These standards serve as references; compliance or certification is not claimed)
