# 칩렛-APU 다중 시스템 생존 아키텍처: 선행 문헌 조사 및 구조 정리

> **정정 및 철회 고지 (Correction & Retraction Notice)**
> 본 문서는 기존에 공개된 버전(v2.8.1, Zenodo DOI 10.5281/zenodo.22374987 등)에서 독자적 기술 창안, 방어적 선행기술, 원천 IP 보유 및 발명자성으로 기술되었던 표현과 근거 없는 수치를 철회하고, 관련 선행 연구 및 공학 규격을 조사·정리한 문헌 종합 보고서로 전환한 문서이다. 기존 공개 버전(v2.8.1)은 역사적 기록으로 유지되며, 본 개정판이 모든 기술적·문헌적 내용 해석을 대체한다. 본 아키텍처 구성에 대한 독립적 발명을 주장하지 않으며, 공학적 공로는 원 문헌의 연구자 및 규격 제정 기관에 전적으로 귀속된다. 본 문서와 원 문헌의 기술 내용이 상충할 경우 원 문헌의 기술 서술 및 해석이 최우선한다.
> 
> 자료 정리자: deundeuni
> 최초 공개 버전 작성일: 2026-08-22 / 최종 개정일: 2026-10-04
> 공식 저장소: https://github.com/soma-moa/chiplet-apu-multi-system-survival-architecture
> 적용 라이선스: CC BY 4.0
> 원안 언어 고지: 한/영 번역 간 차이는 한국어 원문이 기준이나, 실질 내용과 기술적 해석은 원 문헌이 최우선한다.

---

1. 버전 변경 이력

* 2026-10-04 — 정정 및 철회 고지 추가, 독립적 발명 서술 철회, 선행 문헌 조사 및 구조 정리 문서로 전면 전환, 검수 의견 반영.
* v2.8.1 이전 (2026-08-22 ~ 2026-09-27) — 초안 및 방어적 공개 시도 버전 (역사적 기록으로만 유지됨).

---

2. 풀스택 통합 레이어 구조 (4-Tier Taxonomy)

본 문서에서 제시하는 4-Tier 계층 구조(L0~L3)는 다중 컴퓨팅 시스템의 자가 치유 및 내결함성(Fault Tolerance) 관련 선행 연구와 공학 규격을 체계적으로 정리하기 위해 정리자가 분류한 분석 틀이다.

* [L3] 사회적 알림 및 인적 통제 계층 (Escalation & Notification Layer) — 사용자 및 외부 관리자에게 이상 상태를 전달하고 제어권을 안전하게 위임하는 계층이다. (작성자의 적용 구상)
* [L2] 결정론적 제어 및 상태 검증 계층 (Deterministic Governance & Control Layer) — 하드웨어 상태 전이(FSM) 제어 및 고속 하드웨어 래치를 통해 시스템 이상 동작 시 상위 소프트웨어 및 추론 엔진의 비정상 제어 명령을 물리적으로 무효화하는 계층이다. (대응 문헌: Ding et al., 2017 계층형 내결함성 패턴 일반 / Kopetz & Bauer, 2003 Bus Guardian 개념)
* [L1] 칩렛 연산 및 패브릭 계층 (Compute & Fabric Layer) — 연산 유닛(CPU, GPU, NPU)과 이를 연결하는 패브릭 인터커넥트, 브리지 칩렛(작성자 용어: 팀리더/TL 칩렛) 및 CXL 메모리 풀로 구성된 계층이다. (대응 문헌: UCIe / CXL 규격, Flad et al., 2026 TMR 서베이, Khairullah & Elks, 2020 계층형 자가수리, Suvizi et al., 2026)
* [L0] 메인보드 물리 및 인터페이스 계층 (Physical & Material Layer) — 전원 관리 IC(PMIC), 고속 리타이머, 차동 감속 도킹 및 물리적 인터페이스 결합을 담당하는 계층이다. (대응 문헌: CXL/PCIe 리타이머 규격)

2.5 AI 역할 및 텔레메트리 기반 관제

컴퓨팅 패브릭 및 제어계의 이상 상태를 감지하고 자가 치유를 보조하기 위해 텔레메트리 기반 이상 탐지 모델이 적용될 수 있다.

* 2.5.1 텔레메트리 신호 수집 — 연산 실시간 상태, 인터커넥트 패브릭 지연시간, 전력 및 열 상태 신호를 수집하여 이상 징후를 감지한다. (대응 문헌: Suvizi et al., 2026)
* 2.5.2 이상 탐지 및 치유 보조 — 무작위 샘플링 스캔 결과에 따른 격리 처리, 자가 치유 모듈의 자원 점유 과다 시 하드웨어적 제약(Rate Limit) 적용, 관제 노드 부하 발생 시 합의 프로토콜에 기반한 역할 이관을 보조한다. (대응 문헌: Agiakatsikas et al., 2016 모듈 기반 재구성 제어 / 작성자의 적용 구상)

---

3. 패브릭 및 분산 합의 메커니즘

* 이중화 및 메쉬 패브릭 (Dual-Redundant & Mesh Fabric) — 주 데이터 처리를 담당하는 메인 고속 경로와 하드웨어/소프트웨어 기반 보조 우회 경로(작성자 용어: 갓길)의 이중화를 제공하며, 동적 노드 추가가 가능한 메쉬 구조로 확장된다. (대응 문헌: UCIe, CXL 규격 / Alagoz, 2008 계층형 TMR 네트워크 일반)
* 브리지 칩렛 (작성자 용어: 팀리더/TL 칩렛) — 연산 코어 간 명령 번역 및 스트림 버퍼 관리를 수행하여 데이터 병목을 완화한다. (대응 문헌: HSA AQL 규격)
* 텔레메트리 및 양방향 백프레셔 (Telemetry & Bi-directional Backpressure) — 연산, 지연, 전력/열 상태를 모니터링하고, 명령 큐 점유율이 설정된 임계치에 도달하면 역방향 흐름 제어 신호를 발생시켜 과부하를 예방한다. (대응 문헌: PCIe/CXL 크레딧 기반 흐름 제어 규격)
* 분산 합의 기반 제어 노드 (작성자 용어: 관제탑/CCS) — 단일 장애점(SPOF) 방지를 위해 분산 관제 노드 간 Raft 합의 알고리즘을 적용하고, 주 관제 노드의 부하가 임계치를 초과하거나 열 트립 경고 발생 시 인접 노드로 제어 권한을 순환 이관한다. (대응 문헌: Ongaro & Ousterhout, 2014)

---

4. 면역 모방 이상 탐지, 우회 경로 및 물리적 격리

* 동적 대역폭 정속화 제어기 (Traffic Shaper / Rate Limiter) — 우회 경로(작성자 용어: 갓길) 내 패킷 폭주로 인한 대역폭 고갈을 방지하기 위해 토큰 버킷 기반 계량·마킹 규격을 적용한다. (대응 문헌: IETF RFC 2697, RFC 2698)
* 무작위 샘플링 이상 탐지 (Random Sampling / Anomaly Detection, 작성자 용어: 백혈구 스캔) — 패브릭 데이터 흐름을 비동기 무작위 샘플링하여 이상 패킷 감지 시 격리 버퍼로 이송한다. (대응 문헌: Forrest et al., 1994 면역 모방 이상 탐지 계열 / Mange et al., 1998 생체 모방 자가수리)
* 비인가 인터럽트 차단 회로 (Unauthorized Access Interception) — 정식 승인 없이 우회 경로 진입을 시도하는 비인가 인터럽트 요청을 차단하고 암호화 핸드셰이크 토큰 검증을 수행한다. (작성자의 적용 구상)
* 자원 점유 제한기 (Resource Governor, 작성자 용어: T-Reg) — 자가 치유 및 관제 모듈이 시스템 자원을 과도하게 점유하는 현상을 방지하기 위해 전력·클럭·버스 점유율이 임계치를 초과할 경우 하드웨어적으로 동작을 제한한다. (대응 문헌: Vestal, 2007 혼합 중요도 스케줄링 기반 선순위 작업 보호)
* 고임피던스 물리 버스 격리 (Tri-State Bus Isolation, 작성자 용어: 물리 절단) — 비정상 노드의 버스 침범 시 트라이스테이트(High-Z) 상태로 전환하여 물리적 버스 분리 및 하드웨어 리셋을 집행한다. (대응 문헌: Kopetz & Bauer, 2003 Bus Guardian 개념)

---

5. 독립 Safety IP, 안티탬퍼 및 작성자의 이전 작업물

* 독립 Safety IP 및 안티탬퍼 키 소거 회로 — 메인 코어와 독립된 전원·클럭 영역을 사용하는 관제 IP를 배치하고, 물리적 침입 감지 시 암호화 키 소거(Zeroization) 및 회로 무효화를 수행한다. (대응 문헌: NIST FIPS PUB 140-3)
* 작성자의 이전 작업물 및 참고 메모 — 차동 감속 도킹 및 V홈 자율 정렬 메커니즘은 작성자의 이전 공학 작업물(CWP 시리즈)의 개념을 참조하여 정리하였다. 재난 피난 및 해양 구조 관련 참조는 작성자의 기존 작업물(LAST-LIGHT, MAX-LIFE-ICE-BELT)을 바탕으로 한다.

---

6. 시스템 적용 분야 (작성자의 적용 구상)

* 클라우드 및 데이터센터 AI 가속기 — 다중 칩렛 클러스터에서의 패브릭 데드락 예방 및 고장 우회 구조.
* 차량용 및 산업용 제어기 — 내결함성 및 기능안전 요구사항이 존재하는 엣지 컴퓨팅 노드.
* 미션 크리티컬 엣지 시스템 — 무인 모빌리티 및 환경 간섭이 심한 산업 현장용 자가치유 컴퓨팅.

---

7. 문서의 성격 및 조건

* 7.1 원 문헌 최우선 — 본 문서에 기재된 모든 기술적 개념의 공로는 원 문헌의 연구자 및 기관에 돌아가며, 본 문서의 서술과 원 문헌 내용이 상충할 경우 원 문헌이 최우선한다.
* 7.2 공개 자료의 성격 — 본 문서는 학술 및 공학 문헌을 정리한 공공 자료로서의 성격을 가진다.
* 7.3 CC BY 4.0 단일 적용 — 본 저작물은 크리에이티브 커먼즈 저작자표시 4.0 국제 라이선스(CC BY 4.0)에 따라 이용할 수 있다.
* 7.4 사업화 내용 분리 — 본 문서는 순수 공학 기술 문헌 정리 보고서로, 사업화 추진 및 상업적 서비스 개시 관련 내용은 포함하지 않는다.

---

8. 참고 문헌 (References)

[서지 대조 완료 (제목·저자·게재지 확인)]
* Suvizi et al. (2026), "Firmware-Reconfigurable Self-Healing for Remote Glitch-Injection on Autonomous Navigation Systems," GLSVLSI '26, DOI 10.1145/3787109.3815273.
* Shawkat Sabah Khairullah, Carl R. Elks (2020), "Self-Repairing Hardware Architecture for Safety-Critical Cyber-Physical-Systems," IET Cyber-Physical Systems: Theory & Applications, 5(1):92-99, DOI 10.1049/iet-cps.2019.0022.
* Flad et al. (2026), "A Comprehensive Survey of Redundancy Systems with a Focus on Triple Modular Redundancy (TMR)," arXiv:2603.14411.
* Vestal, S. (2007), "Preemptive Scheduling of Multi-Criticality Systems with Varying Degrees of Execution Time Assurance," IEEE RTSS 2007.
* Kopetz, H., & Bauer, G. (2003), "The Time-Triggered Architecture," Proceedings of the IEEE, 91(1), 112–126.

[2차 인용 서지 (TMR 서베이 참고문헌 목록에서 확인)]
* Ding, Morozov, Janschek (2017), "Classification of Hierarchical Fault-Tolerant Design Patterns"
* Agiakatsikas et al. (2016), "Reconfiguration Control Networks for TMR Systems with Module-Based Recovery", FCCM
* Mange et al. (1998), "Embryonics: A new methodology for designing field-programmable gate arrays with self-repair and self-replicating properties", IEEE TVLSI 6(3):387–399
* Alagoz (2008), "Hierarchical Triple-Modular Redundancy (H-TMR) Network For Digital Systems", arXiv:0902.0241

[참고 서지 (제목은 정리자 기억 기반, 원문 미대조)]
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

부록 (Appendix)

작성 과정에서 초안·검수 도구를 사용했으며, 최종 내용 확인과 책임은 작성자에게 있다.
