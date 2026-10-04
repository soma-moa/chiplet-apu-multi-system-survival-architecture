> 다국어 공개 안내: 본 문서는 동일 내용의 한/영 이중 공개 문서입니다. (영문: ARCHITECTURE_STRATEGY.md)
> 정정 고지 (Correction & Retraction Notice - v3.5 개정일: 2026-10-04): 본 문서는 독자적 설계나 선행기술 선점을 주장하는 문서가 아니라, 공개된 선행 연구·표준·실무 문헌을 조립형 생존 아키텍처 관점에서 분류 및 정리한 문헌 조사(Literature Review) 문서입니다. 이전 버전에서 명시되었던 독창성 주장, 설계자 고유 결정권 및 선행기술 선점 선언은 전면 철회합니다. 본 기술 정리의 공학적 공로는 인용된 원 문헌의 저자와 실무자들에게 귀속되며, 작성자는 자료 정리자로 기능합니다. 내용상 본 문서와 원 문헌 간 불일치가 존재할 경우 원 문헌이 최우선합니다. 기존에 출판된 공개 버전(Zenodo DOI 등)은 역사적 기록으로 유지되나, 본 개정 버전이 이를 대체합니다.
> 저작권 안내: 본 문서 및 관련 표현물, 정리 자료에는 CC BY 4.0 라이선스를 단일 적용합니다.

ARCHITECTURE_STRATEGY.ko.md — 조립형 생존 아키텍처에 관한 선행 문헌 조사 및 구조 정리 (v3.5)

> 본 문서는 특정 기업이나 브랜드를 배제하고, 구현 수단이나 제어 주체와 무관하게 시스템 생존성과 경제성을 동시에 확보하기 위한 범용 조립 방식(Modular Architecture)의 구조적 특성과 표준화 논리를 선행 연구 및 표준 문헌을 바탕으로 분류·정리한 자료입니다.

1. 두 가지 모듈 및 연산 블록 조립 방식 비교
 * 단일 통합형 (Single-Die / Monolithic Integration)
   * 구조 — 하나의 단일 구역(Die/Monolith)에 모든 연산, 제어 및 물리 블록을 통합하여 구성하는 방식입니다.
   * 특징 — 초기 제조 및 구동 단가가 비교적 저렴하며, 블록 간 내부 배선 및 인터페이스 구조가 단순합니다.
   * 제약 — 특정 영역에 단일 결함이 발생할 경우 전체 시스템 부하 및 전면 마비 위험이 존재할 수 있으며, 영역별 전원 및 보안 격리에 한계가 존재할 수 있습니다.
 * 조립 분리형 (Chiplet / Modular Integration)
   * 구조 — 기능별로 분리된 개별 칩렛·소프트웨어 모듈·물리 블록들을 패브릭 Interconnect 및 표준 인터페이스로 상호 연결하는 방식입니다. (UCIe Specification; Khairullah & Elks, 2020)
   * 특징 — 특정 블록에 장애가 발생해도 해당 영역만 독자 격리하고 인접, 개별, 또는 예비 블록으로 제어를 이양하는 자가치유 구성을 지향합니다. (FPGA 상의 chiplet-style 분할 프로토타입 실증 사례: Swift-Healer; arXiv:2603.14411) 전원, 클럭, 보안, 물리적 제어 영역을 블록 단위로 독립화할 수 있습니다.
   * 제약 — 블록 간을 연결하는 브리지 [TL Bridge (작성자 용어 / Die-to-Die Interconnect)] 인터페이스와 복잡한 물리·소프트웨어 제어 설계가 요구됩니다.

2. 이중 트랙 운용의 목적
두 방식은 우열의 문제가 아닌, 운용 목적과 적용 환경의 차이에 따라 병행됩니다.
 * 보급 및 비용 검증 트랙 (단일 통합형) — 단가 절감 및 일괄 제조·구동 수율 검증을 위해 활용됩니다.
 * 생존 및 보안 현장 트랙 (조립 분리형) — 데이터 센터, 자율주행, 산업 라인, EV 교환 시스템, 재난 대피 인프라 등 무중단 구동과 데이터/물리 격리가 요구되는 환경에 적용됩니다 (작성자의 적용 구상). 외부 검증되지 않은 블록의 무단 접근을 완화하고, 자체 블록을 독립 격리하여 안전성을 확보하는 방안이 연구되어 왔습니다. (Swift-Healer; Control Engineering "Architecture for Mitigating Effects of External Faults")

3. 조립형 아키텍처의 핵심 상호호환 논리
 * 고성능 구성 — 핵심 제어 본체 블록 + 고성능 외부 연산 가속/구동 블록 [TL Bridge (작성자 용어 / Die-to-Die Interconnect) 연결] → 최고 성능 확보 (UCIe Specification)
 * 범용·우회 구성 — 외부 가속/구동 블록 이탈/장애 시 내장 백업 연산/구동 블록으로 연산 및 제어 상태를 이행 → 시스템 무중단 유지 및 원가 최적화 도모 (arXiv:2603.14411)
상호호환성의 핵심은 TL Bridge (작성자 용어 / Die-to-Die Interconnect) 및 제어 OS 레이어의 표준화입니다. 브리지 인터페이스가 전원/클럭/제어를 독립 분리하고, 표준 상태 검증 체계를 통해 블록 상태를 파악하므로 메인보드 및 기본 기구 구조 수정 없이 요구사항에 따라 연산 및 기능 블록을 선택적으로 착탈 및 교체하는 구조적 선택지가 제공됩니다. (UCIe Specification; arXiv:2603.14411)

4. 내부 기술 노하우의 점진적 축적
 * 1단계 — 외부 고성능 블록을 연결하여 초기 시스템 성능 역량을 확보합니다.
 * 2단계 — 보급형 및 현장용 실증 환경에서 자체 내장 블록을 병행 운용함으로써 자가치유 및 제어 방식을 검증합니다.
 * 3단계 — 외부 블록이 배제된 극단적 장애 상황에서도 시스템이 최소 기능(Failsafe)을 유지하며 작동하는 자율 격리 구조를 구축합니다. (Control Engineering; AAAI FS-07-02)

5. 반도체 및 조립형 시스템의 학술·산업 동향
주요 문헌과 산업 표준(UCIe Specification, IEEE EPS)에서는 외부 고성능 연산 모듈과의 연동성을 높임과 동시에, 시스템 고장 가능성에 대비하여 예비 연산·제어 블록을 수평적으로 배치하고 검증하는 자가치유·중복 구성을 주요 아키텍처 과제로 다루고 있습니다.

6. 범용 구조 및 다층 제어 토폴로지의 문헌적 분류
본 장에서는 문헌 및 표준에서 다루어지는 모듈 분리, 상태 검증, 이상 격리 및 자가치유 메커니즘을 계층 및 토폴로지별로 분류하여 정리합니다.

 * 작성자의 이전 공개 작업물 연계 (Author's Prior Artifact References)
   * 작성자가 이전에 작업 및 출간한 조립형 아키텍처 관련 백서 및 시스템 제어기 문헌(CWP 시리즈, chiplet-apu-multi-system-survival-architecture)은 본 문헌 정리 자료의 작성자 사전 배경 작업물로서 개별 참조됩니다.

 * 실행 계층별 격리 및 치유 접근법 (Layer-Specific Isolation & Healing Methods)
   * 하드웨어 계층 — 마이크로코드, 펌웨어(FW), 메모리 컨트롤러, IOMMU/MMU, 브리지 로직 기반의 물리적·전기적 격리 (IEEE EPS; AAAI FS-07-02), 작성자의 CWP 구상에 따른 물리 기구 클러치 결합 제어.
   * 시스템 및 자원 제어 계층 — 하드웨어 및 메모리 레벨의 봉쇄 장벽(방화벽, MMU 등)을 활용한 결함 봉쇄. (Control Engineering)
   * 상위 소프트웨어/AI 계층 — SW 에이전트, AI 가속기, 인공신경망(ANN) 진단 기반 TMR 중복 제어 및 상태 검증. (arXiv:2603.14411)

 * 문헌상 제어 토폴로지 분류 (Control Topology Taxonomies in Literature)
   * 개별 노드 자율 통제 (Autonomous Individual Control) — 외부 통제기 개입 없이 개별 칩렛·모듈 단독으로 내적 이상 판단 시 자가 격리, 전원 감쇄, 또는 최소 기능 모드로 전환하는 구조. (UCIe Specification; Swift-Healer)
   * 수평적 P2P 피어 통제 (Horizontal Peer-to-Peer Control) — 동일 계층의 인접 에이전트/블록들이 수평 통신하여 이상 노드를 상호 배제하거나 대리 연산을 수행하는 국소 분산 제어 구조. (arXiv:1505.05537)
   * 다층 트리 및 매트릭스 하이브리드 (Multi-Tier Hybrid Control) — 개별 노드 ↔ 별도 치유 계층(1~3차 방어) ↔ 전역 관제로 이어지는 수직·수평 다단계 연계 제어 및 다층 자가수리 구조. (Khairullah & Elks, 2020; arXiv:2603.14411)

 * 지역 인접 선조치 및 지연 완화 (Localized Proximity Action & Latency Mitigation)
   * 이상 탐지 시 중앙 제어 지연(Latency)을 완화하기 위해 물리적·논리적으로 가장 가까운 인접 노드 또는 하위 제어 계층이 중앙 제어보다 먼저 1차 국소 격리(Local Containment)를 실행한 뒤, 상위 중간 관리자 또는 중앙 시스템으로 상위 보고하는 구조입니다. (Control Engineering; AAAI FS-07-02)

 * 예지적 선조치 및 예측 격리 (Predictive Preemptive Action)
   * 텔레메트리 누적 데이터, 시계열 동적 패턴 학습, 미세 전압·온도 섭동 감지, 또는 예측 알고리즘을 통해 실제 하드웨어 파손 및 데이터 오류가 발생하기 이전 단계에서 이상 징후를 감지하고, 유휴 레인/블록으로 미리 바이패스하거나 사전 격리를 실행하는 제어 구성입니다. (UCIe Specification; Swift-Healer) 한 프로토타입의 측정값(Swift-Healer)에서는 이상을 실시간 루프 2회 앞서(약 0.06ms) 예측했고, 복구 지연(MTTR)은 클럭 조정 약 0.005ms, 부분 재구성 약 1.1ms로 보고된 바 있습니다.

7. 실리보호 (Practical Protection)
 * 번역본 참조 원칙: 본 정리 문서의 한글본과 영문 번역본 간 문맥상 차이가 존재할 경우 한글본을 참고 기준으로 하나, 명세의 실질적 내용 및 공학적 해석은 원 문헌(Primary Literature)을 최우선 기준으로 적용한다.
 * 원 문헌 최우선 원칙: 본 정리 문서에 포함된 내용의 법적·공학적 해석 및 권리는 인용된 원문 연구자 및 표준 기구의 문헌에 속하며, 본 문서와 원 문헌 간 해석상 차이가 있을 경우 원 문헌을 우선 기준으로 적용한다.
 * 공개 자료의 성격: 본 문서는 학술적·산업적으로 공개된 선행 기술 연구와 표준 규격을 정리한 결과물이며, 신규 특허권이나 독점적 기술 선점을 주장하지 않는다.
 * 라이선스 단일 적용: 본 저장소의 문서 및 정리 자료에는 CC BY 4.0 라이선스를 단일 적용한다. 상세 법적 조건은 저장소 루트의 LICENSE 파일을 따른다.
 * 사업화 내용 분리: 본 백서 원안에는 Pure Open Source 및 선행기술 조사 내용만을 포함하며, 독자적인 수익 모델 및 사업화 세부 실행안은 별도 기술 문서로 분리 관리한다.

8. 출처 및 기록 (Sources & Records)

 * 선행 문헌 및 표준 규격 목록 (Primary Literature & Standards)
   * UCIe Consortium, "Universal Chiplet Interconnect Express (UCIe) Specification (v1.0 / v1.1)", 2022–2023. — 레인별 오류 추적, 주기적 패리티 Flit, 임계치 기반 재훈련 및 예지 고장 분석 규격
   * Suvizi, Iwu, Amberiadis, Venkataramani, "Swift-Healer: Firmware-Reconfigurable Self-Healing for Remote Glitch-Injection on Autonomous Navigation Systems", Great Lakes Symposium on VLSI (GLSVLSI '26), NIST Pub ID 961103, 2026. DOI: 10.1145/3787109.3815273 — Zynq FPGA 기반 chiplet-style 분할 프로토타입에서의 펌웨어 재구성형 자가치유 및 글리치 격리 실증
   * Lukas Flad, Mark Leyer, Felix Sebastian Nitz, Tobias Krawutschke, "A Comprehensive Survey of Redundancy Systems with a Focus on Triple Modular Redundancy (TMR)", arXiv preprint arXiv:2603.14411, 2026. DOI: 10.48550/arXiv.2603.14411 — 중복 시스템 및 TMR/ANN 수리 구조 종합 서베이
   * Shawkat Sabah Khairullah, Carl R. Elks, "Self-Repairing Hardware Architecture for Safety-Critical Cyber-Physical-Systems", IET Cyber-Physical Systems: Theory & Applications, Vol. 5, No. 1, pp. 92–99, 2020 (online 2019-11) / arXiv:1910.14127. DOI: 10.1049/iet-cps.2019.0022 — 안전 필수 사이버물리시스템을 위한 계층형(1~3차 방어) 자가수리 하드웨어 아키텍처
   * Mohsen Khalili, Xiaodong Zhang, Marios M. Polycarpou, Thomas Parisini, Yongcan Cao, "Distributed Adaptive Fault-Tolerant Control of Uncertain Multi-Agent Systems", arXiv preprint arXiv:1505.05537, 2015. DOI: 10.48550/arXiv.1505.05537 — 불확실 멀티에이전트 시스템의 분산 적응 결함허용 제어
   * AAAI, "FPGA-Based Fault Detection, Isolation, and Recovery (FDIR)", AAAI Fall Symposium Series (FS-07-02), 2007. — 차량/시스템 건전성 관리 및 FPGA 결함 처리
   * IEEE Electronics Packaging Society (EPS), "Architecting Chiplets for Product Manufacturing Test Resiliency", IEEE EPS Publications. — 칩렛 격리 및 레인 중복/시험 복원력 모범 사례
   * Tessolve, "Challenges and Solutions for Building Systems of Chiplets", Tessolve Engineering Whitepaper. — 예비 칩렛, 텔레메트리 기반 수리 및 시스템 구축 과제
   * Dave Harrold, "Architecture for Mitigating Effects of External Faults", Control Engineering Magazine, 2011. — 결함 복구(Recovery)와 봉쇄(Containment)의 개념적 구분을 다룬 매거진 기사

 * 작성자의 이전 공개 작업물 (Author's Prior Artifact References)
   * soma-moa / chiplet-apu-multi-system-survival-architecture (CERN Zenodo DOI: 10.5281/zenodo.22374987) — APU 연산 제어 및 조립형 아키텍처 관련 작성자의 이전 백서 자료
   * soma-moa / LAST-LIGHT (CERN Zenodo DOI: 10.5281/zenodo.22373189) — 재난 피난 유도 보조 인프라 작성자의 이전 백서 자료
   * soma-moa / CWP Series (CWP-Entry, CWP-Battery-Swap, CWP-Clamping-Battery-Swap-System, CWP-Rolling-Self-Align-Battery-Swap-System) (CERN Zenodo DOIs: 10.5281/zenodo.22373538, 10.5281/zenodo.22373722, 10.5281/zenodo.22373704) — 메커니즘 및 도킹 관련 작성자의 이전 백서 자료
   * soma-moa 메인 저장소 (soma-moa / soma-moa | somamoa.ai.kr) — 통합 문서 및 이전 버전 백서 저장소
