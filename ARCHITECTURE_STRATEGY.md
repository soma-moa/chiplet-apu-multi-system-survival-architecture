> Multilingual Notice: This document is a dual-published document in Korean and English with identical contents. (Korean: ARCHITECTURE_STRATEGY.ko.md)
> Correction & Retraction Notice (v3.5 Revision Date: 2026-10-04): This document is a literature survey document classifying and organizing published prior research, standards, and practical literature from the perspective of modular survival architectures, rather than claiming original designs or prior art preemption. Prior claims of originality, designer decision-making authority, and prior art preemption declarations in earlier versions are fully retracted. The engineering credit for this technical synthesis belongs to the authors and practitioners of the cited primary literature; the author functions solely as a compiler of materials. In case of any discrepancy between this document and the primary literature, the primary literature shall take precedence. Previously published public versions (Zenodo DOI, etc.) are maintained as historical records, but this revised version supersedes them.
> Copyright Notice: A single CC BY 4.0 license is applied to this document, its derivative expressions, and compiled materials.

ARCHITECTURE_STRATEGY.md — Literature Survey and Structural Classification of Modular Survival Architectures (v3.5)

> This document excludes specific companies or brands and classifies and synthesizes structural characteristics and standardization logic of universal modular architectures aimed at securing system survivability and economic viability simultaneously, regardless of implementation means or control entities, based on prior research and standard literature.

1. Comparison of Two Module and Compute Block Assembly Approaches
 * Monolithic Integration (Single-Die / Monolithic Integration)
   * Structure — An approach where all compute, control, and physical blocks are integrated into a single region (Die/Monolith).
   * Characteristics — Initial manufacturing and operation costs are relatively low, and internal wiring and interface structures between blocks are simple.
   * Limitations — In the event of a single defect in a specific region, a risk of overall system load and total paralysis may exist, and limitations may exist in per-region power and security isolation.
 * Modular Integration (Chiplet / Modular Integration)
   * Structure — An approach where individual chiplets, software modules, and physical blocks separated by function are interconnected via fabric interconnects and standard interfaces. (UCIe Specification; Khairullah & Elks, 2020)
   * Characteristics — Aims for self-healing configurations where even if a failure occurs in a specific block, that region is independently isolated, and control is handed over to adjacent, individual, or spare blocks. (FPGA-based chiplet-style partitioned prototype implementation example: Swift-Healer; arXiv:2603.14411) Power, clock, security, and physical control regions can be individualized on a block unit basis.
   * Limitations — Requires bridge interfaces [TL Bridge (author's terminology / Die-to-Die Interconnect)] connecting blocks and complex physical/software control design.

2. Purpose of Dual-Track Operation
The two approaches are not a matter of superiority, but are operated in parallel depending on the operational purpose and application environment.
 * Mass-Market and Cost Validation Track (Monolithic Integration) — Utilized for cost reduction and batch manufacturing/operation yield validation.
 * Survival and Field Security Track (Modular Integration) — Applied to environments where uninterrupted operation and data/physical isolation are required, such as data centers, autonomous driving, industrial lines, EV battery swap systems, and disaster evacuation infrastructure (author's application concept). Approaches have been studied to mitigate unauthorized access to unverified external blocks and secure safety by independently isolating internal blocks. (Swift-Healer; Control Engineering "Architecture for Mitigating Effects of External Faults")

3. Core Interoperability Logic of Modular Architecture
 * High-Performance Configuration — Core control main block + high-performance external compute acceleration/drive block [connected via TL Bridge (author's terminology / Die-to-Die Interconnect)] → Securing maximum performance (UCIe Specification)
 * Universal / Bypass Configuration — In the event of disconnection/failure of external acceleration/drive blocks, compute and control states are transferred to internal backup compute/drive blocks → Maintaining uninterrupted system operation and optimizing costs (arXiv:2603.14411)
The core of interoperability lies in the standardization of the TL Bridge (author's terminology / Die-to-Die Interconnect) and the control OS layer. Because the bridge interface independently separates power/clock/control and assesses block status through standard status verification systems, structural options are provided to selectively attach, detach, and replace compute and functional blocks according to requirements without modifying the motherboard or base mechanical structure. (UCIe Specification; arXiv:2603.14411)

4. Progressive Accumulation of Internal Technical Know-how
 * Stage 1 — Connecting external high-performance blocks to secure initial system performance capabilities.
 * Stage 2 — Operating internal built-in blocks in parallel in entry-level and field verification environments to validate self-healing and control methods.
 * Stage 3 — Establishing autonomous isolation structures that maintain minimum functionality (failsafe) even under extreme failure conditions where external blocks are excluded. (Control Engineering; AAAI FS-07-02)

5. Academic and Industrial Trends in Semiconductor and Modular Systems
Major literature and industry standards (UCIe Specification, IEEE EPS) address self-healing and redundancy configurations as primary architectural challenges, enhancing connectivity with external high-performance compute modules while horizontally arranging and verifying spare compute/control blocks to prepare for potential system failures.

6. Literature Classification of Universal Structures and Multi-Tier Control Topologies
This section classifies and organizes module separation, status verification, fault isolation, and self-healing mechanisms addressed in literature and standards by layer and topology.

 * Author's Prior Artifact References
   * Technical whitepapers and system controller literature previously authored and published by the author regarding modular architectures (CWP series, chiplet-apu-multi-system-survival-architecture) are individually referenced as the author's prior background work for this literature survey.

 * Layer-Specific Isolation & Healing Methods
   * Hardware Layer — Physical and electrical isolation based on microcode, firmware (FW), memory controllers, IOMMU/MMU, and bridge logic (IEEE EPS; AAAI FS-07-02), as well as physical mechanical clutch coupling control according to the author's CWP concept.
   * System & Resource Control Layer — Fault containment utilizing containment barriers (firewalls, MMUs, etc.) at the hardware and memory level. (Control Engineering)
   * Higher Software / AI Layer — Software agents, AI accelerators, and artificial neural network (ANN) diagnosis-based TMR redundancy control and status verification. (arXiv:2603.14411)

 * Control Topology Taxonomies in Literature
   * Autonomous Individual Control — Structures where individual chiplets/modules independently perform self-isolation, power attenuation, or transition to minimum functionality mode upon internal anomaly judgment without external controller intervention. (UCIe Specification; Swift-Healer)
   * Horizontal Peer-to-Peer Control — Locally distributed control structures where adjacent agents/blocks on the same layer communicate horizontally to mutually exclude anomalous nodes or perform surrogate computation. (arXiv:1505.05537)
   * Multi-Tier Hybrid Control — Vertical and horizontal multi-stage linked control and multi-tier self-repair structures leading from individual nodes ↔ separate healing layers (1st-3rd line defense) ↔ global orchestration. (Khairullah & Elks, 2020; arXiv:2603.14411)

 * Localized Proximity Action & Latency Mitigation
   * Structures where, to mitigate central control latency upon anomaly detection, the physically/logically closest adjacent node or lower control tier executes primary local containment prior to central control, followed by reporting upward to intermediate managers or the central system. (Control Engineering; AAAI FS-07-02)

 * Predictive Preemptive Action
   * Control configurations that detect anomaly signs in advance through telemetry accumulated data, time-series dynamic pattern learning, subtle voltage/temperature perturbation detection, or predictive algorithms before actual hardware failure and data errors occur, preemptively bypassing or isolating to idle lanes/blocks. (UCIe Specification; Swift-Healer) In a specific prototype measurement case (Swift-Healer), anomalies were predicted two real-time loop cycles (~0.06 ms) in advance, and recovery latency (MTTR) was reported as ~0.005 ms for clock adjustment and ~1.1 ms for partial reconfiguration.

7. Practical Protection
 * Translation Reference Principle: In the event of contextual differences between the Korean text and English translation of this summary document, the Korean text shall serve as the reference baseline; however, the substantive content and engineering interpretation of specifications shall give highest priority to the primary literature.
 * Primary Literature Precedence: The legal and engineering interpretation and rights of the content included in this summary document belong to the cited primary literature researchers and standards bodies. In case of interpretation discrepancies between this document and primary literature, primary literature shall take precedence.
 * Nature of Public Information: This document is a compiled output of publicly available prior technology research and standard specifications in academia and industry, and does not claim new patent rights or proprietary technology preemption.
 * Single License Application: A single CC BY 4.0 license is applied to the documents and compiled materials in this repository. Detailed legal conditions follow the LICENSE file at the root of the repository.
 * Separation of Commercialization Details: The original whitepaper text includes only pure open-source and prior art survey content, while proprietary revenue models and detailed commercialization plans are managed separately in distinct technical documents.

8. Sources & Records

 * Primary Literature & Standards
   * UCIe Consortium, "Universal Chiplet Interconnect Express (UCIe) Specification (v1.0 / v1.1)", 2022–2023. — Specifications for per-lane error tracking, periodic parity Flit, threshold-based retraining, and predictive failure analysis.
   * Suvizi, Iwu, Amberiadis, Venkataramani, "Swift-Healer: Firmware-Reconfigurable Self-Healing for Remote Glitch-Injection on Autonomous Navigation Systems", Great Lakes Symposium on VLSI (GLSVLSI '26), NIST Pub ID 961103, 2026. DOI: 10.1145/3787109.3815273 — Firmware-reconfigurable self-healing and glitch isolation demonstration on a Zynq FPGA-based chiplet-style partitioned prototype.
   * Lukas Flad, Mark Leyer, Felix Sebastian Nitz, Tobias Krawutschke, "A Comprehensive Survey of Redundancy Systems with a Focus on Triple Modular Redundancy (TMR)", arXiv preprint arXiv:2603.14411, 2026. DOI: 10.48550/arXiv.2603.14411 — Comprehensive survey of redundancy systems and TMR/ANN repair structures.
   * Shawkat Sabah Khairullah, Carl R. Elks, "Self-Repairing Hardware Architecture for Safety-Critical Cyber-Physical-Systems", IET Cyber-Physical Systems: Theory & Applications, Vol. 5, No. 1, pp. 92–99, 2020 (online 2019-11) / arXiv:1910.14127. DOI: 10.1049/iet-cps.2019.0022 — Tiered (1st-3rd line defense) self-repairing hardware architecture for safety-critical cyber-physical systems.
   * Mohsen Khalili, Xiaodong Zhang, Marios M. Polycarpou, Thomas Parisini, Yongcan Cao, "Distributed Adaptive Fault-Tolerant Control of Uncertain Multi-Agent Systems", arXiv preprint arXiv:1505.05537, 2015. DOI: 10.48550/arXiv.1505.05537 — Distributed adaptive fault-tolerant control of uncertain multi-agent systems.
   * AAAI, "FPGA-Based Fault Detection, Isolation, and Recovery (FDIR)", AAAI Fall Symposium Series (FS-07-02), 2007. — Vehicle/system health management and FPGA fault processing.
   * IEEE Electronics Packaging Society (EPS), "Architecting Chiplets for Product Manufacturing Test Resiliency", IEEE EPS Publications. — Best practices for chiplet isolation and lane redundancy/manufacturing test resiliency.
   * Tessolve, "Challenges and Solutions for Building Systems of Chiplets", Tessolve Engineering Whitepaper. — Spare chiplets, telemetry-based repair, and system building challenges.
   * Dave Harrold, "Architecture for Mitigating Effects of External Faults", Control Engineering Magazine, 2011. — Magazine article addressing the conceptual distinction between fault recovery and fault containment.

 * Author's Prior Artifact References
   * soma-moa / chiplet-apu-multi-system-survival-architecture (CERN Zenodo DOI: 10.5281/zenodo.22374987) — Author's prior whitepaper material regarding APU compute control and modular architectures.
   * soma-moa / LAST-LIGHT (CERN Zenodo DOI: 10.5281/zenodo.22373189) — Author's prior whitepaper material regarding disaster evacuation guidance auxiliary infrastructure.
   * soma-moa / CWP Series (CWP-Entry, CWP-Battery-Swap, CWP-Clamping-Battery-Swap-System, CWP-Rolling-Self-Align-Battery-Swap-System) (CERN Zenodo DOIs: 10.5281/zenodo.22373538, 10.5281/zenodo.22373722, 10.5281/zenodo.22373704) — Author's prior whitepaper materials regarding mechanisms and docking.
   * soma-moa Main Repository (soma-moa / soma-moa | somamoa.ai.kr) — Integrated documentation and prior whitepaper version repository.
