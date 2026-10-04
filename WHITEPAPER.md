# Chiplet-APU Multi-System Survival Architecture: Literature Survey and Structural Synthesis

> **Correction & Retraction Notice**
> This document retracts statements of independent technical invention, defensive prior art, original IP ownership, inventorship, and unsubstantiated figures presented in previously published versions (such as v2.8.1, Zenodo DOI 10.5281/zenodo.22374987), converting the work into a comprehensive literature survey and engineering review. Previously published versions (v2.8.1) are retained solely as historical records, and this revised version supersedes all technical and literature interpretations. No claim of independent invention is made for this architectural configuration; engineering credit belongs entirely to the authors and standards bodies of the cited works. In the event of any conflict between this document and the primary literature, the technical descriptions and interpretations of the primary literature shall prevail.
> 
> Compiler: deundeuni
> First Published Version Date: 2026-08-22 / Final Revision Date: 2026-10-04
> Official Repository: https://github.com/soma-moa/chiplet-apu-multi-system-survival-architecture
> License: CC BY 4.0
> Language Authority Notice: In case of discrepancies between Korean and English translations, the Korean original serves as the reference, but the primary literature remains authoritative for technical interpretation.

---

1. Revision History

* 2026-10-04 — Added correction and retraction notice, retracted independent invention claims, fully converted to literature survey and structural synthesis, incorporated review feedback.
* Prior to v2.8.1 (2026-08-22 ~ 2026-09-27) — Draft and defensive publication attempt versions (retained solely as historical records).

---

2. Full-Stack Integrated Layer Structure (4-Tier Taxonomy)

The 4-Tier hierarchical structure (L0–L3) presented in this document is an analytical taxonomy defined by the compiler to systematically categorize prior research and engineering standards related to self-healing and fault tolerance in multi-system computing.

* [L3] Escalation & Notification Layer — Transmits abnormal states to users and external administrators, safely delegating control. (Compiler's application concept)
* [L2] Deterministic Governance & Control Layer — Controls hardware finite state machines (FSM) and utilizes high-speed hardware latches to physically override invalid control commands from upper software or inference engines during abnormal system operation. (Corresponding Literature: Ding et al., 2017 Hierarchical Fault-Tolerant Design Patterns / Kopetz & Bauer, 2003 Bus Guardian concept)
* [L1] Compute & Fabric Layer — Composed of compute units (CPU, GPU, NPU), fabric interconnects connecting them, bridge chiplets (compiler term: Team Leader / TL Chiplet), and CXL memory pools. (Corresponding Literature: UCIe / CXL Specifications, Flad et al., 2026 TMR Survey, Khairullah & Elks, 2020 Hierarchical Self-Repair, Suvizi et al., 2026)
* [L0] Physical & Material Layer — Handles power management ICs (PMIC), high-speed retimers, differential reduction docking, and physical interface couplings. (Corresponding Literature: CXL/PCIe Retimer Specifications)

2.5 AI Role and Telemetry-Based Governance

Telemetry-based anomaly detection models can be applied to detect abnormal conditions in the computing fabric and control systems to assist self-healing.

* 2.5.1 Telemetry Signal Collection — Collects real-time compute state, interconnect fabric latency, and power/thermal state signals to detect early warning signs. (Corresponding Literature: Suvizi et al., 2026)
* 2.5.2 Anomaly Detection and Healing Assistance — Assists isolation based on random sampling scan results, applies hardware rate limits when self-healing modules exceed resource thresholds, and facilitates role rotation upon control node overload based on consensus protocols. (Corresponding Literature: Agiakatsikas et al., 2016 Module-Based Reconfiguration / Compiler's application concept)

---

3. Fabric and Distributed Consensus Mechanisms

* Dual-Redundant & Mesh Fabric — Provides dual-redundancy between the primary high-speed data path and secondary bypass paths (compiler term: Shoulder/갓길), expanding into a mesh topology supporting dynamic node addition. (Corresponding Literature: UCIe, CXL Specifications / Alagoz, 2008 Hierarchical TMR Network)
* Bridge Chiplet (Compiler Term: Team Leader / TL Chiplet) — Translates commands and manages stream buffers between compute cores to mitigate data bottlenecks. (Corresponding Literature: HSA AQL Specification)
* Telemetry & Bi-directional Backpressure — Monitors compute, latency, and power/thermal states, generating reverse flow-control signals when command queue occupancy reaches configured thresholds to prevent overload. (Corresponding Literature: PCIe/CXL Credit-Based Flow Control Specifications)
* Distributed Consensus Control Nodes (Compiler Term: Control Tower / CCS) — Applies the Raft consensus algorithm across distributed control nodes to eliminate single points of failure (SPOF), dynamically transferring authority to adjacent nodes when the primary node exceeds load thresholds or triggers thermal warnings. (Corresponding Literature: Ongaro & Ousterhout, 2014)

---

4. Immune-Inspired Anomaly Detection, Bypass Paths, and Physical Isolation

* Traffic Shaper / Rate Limiter — Implements token bucket metering/marking specifications on bypass paths (compiler term: Shoulder/갓길) to prevent bandwidth exhaustion caused by packet bursts. (Corresponding Literature: IETF RFC 2697, RFC 2698)
* Random Sampling Anomaly Detection (Compiler Term: Leukocyte Scan) — Asynchronously samples fabric data streams, transferring abnormal packets to quarantine buffers upon detection. (Corresponding Literature: Forrest et al., 1994 Immune-Inspired Anomaly Detection / Mange et al., 1998 Biomimetic Self-Repair)
* Unauthorized Access Interception — Blocks unauthorized interrupt requests attempting access to bypass paths without prior telemetry clearance and verifies cryptographic handshake tokens. (Compiler's application concept)
* Resource Governor (Compiler Term: T-Reg) — Enforces hardware rate limits when self-healing and governance modules exceed power, clock, or bus occupancy limits to prevent excessive resource consumption. (Corresponding Literature: Vestal, 2007 Mixed-Criticality Scheduling for High-Priority Task Protection)
* Tri-State Bus Isolation (Compiler Term: Physical Cutting) — Switches to high-impedance (High-Z) tri-state modes upon unauthorized bus intrusion by faulty nodes, executing physical bus separation and hardware reset. (Corresponding Literature: Kopetz & Bauer, 2003 Bus Guardian concept)

---

5. Independent Safety IP, Anti-Tamper Circuits, and Compiler's Prior Works

* Independent Safety IP & Anti-Tamper Key Zeroization — Deploys governance IP using power and clock domains isolated from main compute cores, executing key zeroization and circuit invalidation upon physical intrusion detection. (Corresponding Literature: NIST FIPS PUB 140-3)
* Compiler's Prior Works and Reference Notes — Differential reduction docking and V-groove self-alignment mechanisms refer to concepts from the compiler's previous engineering works (CWP series). References to emergency evacuation and maritime rescue are based on the compiler's previous works (LAST-LIGHT, MAX-LIFE-ICE-BELT).

---

6. System Application Scope (Compiler's Application Concept)

* Cloud & Datacenter AI Accelerators — Fabric deadlock prevention and fault-rerouting in multi-chiplet clusters.
* Automotive & Industrial Controllers — Edge computing nodes requiring fault tolerance and functional safety compliance.
* Mission-Critical Edge Systems — Self-healing computing for autonomous mobility and harsh industrial environments.

---

7. Document Nature and Conditions

* 7.1 Primary Literature Precedence — All technical concepts in this document are attributed entirely to the original researchers and institutions. In the event of conflicts between this document and primary literature, primary literature shall prevail.
* 7.2 Public Reference Nature — This document serves as a public synthesis of academic and engineering literature.
* 7.3 Single License (CC BY 4.0) — This work is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).
* 7.4 Separation of Commercialization — This document is strictly an engineering literature review; commercialization and service rollout details are explicitly excluded.

---

8. References

[Verified Primary Literature (Titles, Authors, and Venues Confirmed)]
* Suvizi et al. (2026), "Firmware-Reconfigurable Self-Healing for Remote Glitch-Injection on Autonomous Navigation Systems," GLSVLSI '26, DOI 10.1145/3787109.3815273.
* Shawkat Sabah Khairullah, Carl R. Elks (2020), "Self-Repairing Hardware Architecture for Safety-Critical Cyber-Physical-Systems," IET Cyber-Physical Systems: Theory & Applications, 5(1):92-99, DOI 10.1049/iet-cps.2019.0022.
* Flad et al. (2026), "A Comprehensive Survey of Redundancy Systems with a Focus on Triple Modular Redundancy (TMR)," arXiv:2603.14411.
* Vestal, S. (2007), "Preemptive Scheduling of Multi-Criticality Systems with Varying Degrees of Execution Time Assurance," IEEE RTSS 2007.
* Kopetz, H., & Bauer, G. (2003), "The Time-Triggered Architecture," Proceedings of the IEEE, 91(1), 112–126.

[Secondary Citations (Verified in TMR Survey Reference List)]
* Ding, Morozov, Janschek (2017), "Classification of Hierarchical Fault-Tolerant Design Patterns"
* Agiakatsikas et al. (2016), "Reconfiguration Control Networks for TMR Systems with Module-Based Recovery", FCCM
* Mange et al. (1998), "Embryonics: A new methodology for designing field-programmable gate arrays with self-repair and self-replicating properties", IEEE TVLSI 6(3):387–399
* Alagoz (2008), "Hierarchical Triple-Modular Redundancy (H-TMR) Network For Digital Systems", arXiv:0902.0241

[Reference Literature (Titles Memory-Based / Unverified Against Primary Text)]
* UCIe Consortium, "Universal Chiplet Interconnect Express (UCIe) Specification," Rev 1.0/1.1.
* CXL Consortium, "Compute Express Link (CXL) Specification."
* PCI-SIG, "PCI Express Base Specification" (Credit-Based Flow Control Mechanism).
* HSA Foundation, "HSA System Architecture Specification / HSAIL & AQL."
* Ongaro, D., & Ousterhout, J. (2014), "In Search of an Understandable Consensus Algorithm," USENIX ATC '14.
* IETF RFC 2697, "A Single Rate Three Color Marker."
* IETF RFC 2698, "A Two Rate Three Color Marker."
* Forrest, S., et al. (1994), "Self-Nonself Discrimination in a Computer," IEEE Symposium on Research in Security and Privacy.
* NIST FIPS PUB 140-3, "Security Requirements for Cryptographic Modules" (Tamper Detection and Zeroization).

---

Appendix

Drafting and review tools were used during the preparation of this document. Final verification and responsibility rest entirely with the compiler.
