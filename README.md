<div align="center">

# 🌊 DHAKSHIN

### दक्षिण · A Cooperative Autonomous Observation System for the Southern Ocean

**A wave-powered Wave Glider and a deep-diving Argo Float that find each other across a
million square kilometres of ice-filled ocean — using machine-learning trajectory prediction,
zero fuel, and zero ship time on station.**

[![Version](.../version-v1.0.0--draft-0A2A43?style=for-the-badge)](#1-executive-summary)
[![Status](.../status-Design%20Proposal-FF9F1C?style=for-the-badge)](#16-phased-development-roadmap)
[![Programme](.../programme-NCPOR%20%C2%B7%20India-1B998B?style=for-the-badge)](#210-programme-context-within-ncpor)
[![License](.../license-Internal%20(NCPOR)-6C757D?style=for-the-badge)](#203-license-and-distribution)
[![Docs](.../docs-21%20Sections%20%C2%B7%2021%20Appendices-2EC4B6?style=for-the-badge)](#table-of-contents)

![Python] ![PyTorch] ![FastAPI] ![PostgreSQL] ![React] ![TypeScript] ![Rust]
![C / FreeRTOS] ![MQTT] ![Docker] ![GitHub Actions] ![100% Wave + Solar] ![PRs welcome]

| 🛰️ Vehicles | 🔄 Mission Cycle | 🌡️ Max Depth | 📈 Profiles / yr | ⛽ Fuel | 🚢 Ship Visits |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **2** | **~10 days** | **~2,000 m** | **30–35** | **0 L** | **~1 / yr** |

[📖 Documentation]  [🚀 Getting Started]  [🗺️ Roadmap]  [🧠 ML Models]  [🤝 Contributing]

</div>


## Table of Contents

### Part I — The System Design

1. [Executive Summary](#1-executive-summary)
   - [1.1 The idea in one paragraph](#11-the-idea-in-one-paragraph)
   - [1.2 Quick facts](#12-quick-facts)
   - [1.3 What makes this different](#13-what-makes-this-different)
   - [1.4 Mission statement, vision and objectives](#14-mission-statement-vision-and-objectives)
   - [1.5 Scope — in and out](#15-scope-in-and-out)
   - [1.6 Definition of success](#16-definition-of-success)
   - [1.7 Non-technical FAQ](#17-non-technical-faq)
2. [Why: The Southern Ocean Challenge](#2-why-the-southern-ocean-challenge)
   - [2.1 A remote ocean that controls the planet's climate](#21-a-remote-ocean-that-controls-the-planets-climate)
   - [2.2 Why it is hard to observe](#22-why-it-is-hard-to-observe)
   - [2.3 The shift to autonomous platforms](#23-the-shift-to-autonomous-platforms)
   - [2.4 The gap this project fills](#24-the-gap-this-project-fills)
   - [2.5 Target science questions](#25-target-science-questions)
   - [2.6 The observing-system landscape: where this project sits](#26-the-observing-system-landscape-where-this-project-sits)
   - [2.7 Reference operating region (illustrative)](#27-reference-operating-region-illustrative)
   - [2.8 Stakeholders and engagement](#28-stakeholders-and-engagement)
   - [2.9 Design alternatives considered](#29-design-alternatives-considered)
   - [2.10 Programme context within NCPOR](#210-programme-context-within-ncpor)
3. [The Concept at a Glance](#3-the-concept-at-a-glance)
   - [3.1 The twelve elements of the solution](#31-the-twelve-elements-of-the-solution)
   - [3.2 Design principles](#32-design-principles)
   - [3.3 Who does what](#33-who-does-what)
   - [3.4 The system as a mindmap](#34-the-system-as-a-mindmap)
   - [3.5 The twelve elements — design detail](#35-the-twelve-elements-design-detail)
   - [3.6 Concept of operations — a season in the life](#36-concept-of-operations-a-season-in-the-life)
   - [3.7 Assumptions register](#37-assumptions-register)
4. [System Architecture and End-to-End Flow](#4-system-architecture-and-end-to-end-flow)
   - [4.1 The whole system in one picture](#41-the-whole-system-in-one-picture)
   - [4.2 Following the data path](#42-following-the-data-path)
   - [4.3 Communications: what can talk, when](#43-communications-what-can-talk-when)
   - [4.4 Resilience and failover](#44-resilience-and-failover)
   - [4.5 Security architecture](#45-security-architecture)
   - [4.6 System interfaces](#46-system-interfaces)
   - [4.7 Message and data contracts](#47-message-and-data-contracts)
   - [4.8 Telemetry frame format (example)](#48-telemetry-frame-format-example)
   - [4.9 Telemetry and spectrum budget](#49-telemetry-and-spectrum-budget)
   - [4.10 Interference and electromagnetic compatibility](#410-interference-and-electromagnetic-compatibility)
   - [4.11 Shore-side system states](#411-shore-side-system-states)
5. [The Two Vehicles](#5-the-two-vehicles)
   - [5.1 The Wave Glider — the persistent surface sentinel](#51-the-wave-glider-the-persistent-surface-sentinel)
   - [5.2 The Argo Float — the deep-ocean profiler](#52-the-argo-float-the-deep-ocean-profiler)
   - [5.3 Side-by-side comparison](#53-side-by-side-comparison)
   - [5.4 Why the pairing works](#54-why-the-pairing-works)
   - [5.5 Vehicle operating modes](#55-vehicle-operating-modes)
   - [5.6 Vehicle identification and naming](#56-vehicle-identification-and-naming)
   - [5.7 Reliability, availability and maintainability (RAM) targets](#57-reliability-availability-and-maintainability-ram-targets)
   - [5.8 Calibration and metrology plan](#58-calibration-and-metrology-plan)
   - [5.9 Sensor payload at a glance](#59-sensor-payload-at-a-glance)
6. [One Mission Cycle: Dive, Predict, Meet, Recharge](#6-one-mission-cycle-dive-predict-meet-recharge)
   - [6.1 The sequence of one event](#61-the-sequence-of-one-event)
   - [6.2 The rendezvous sequence diagram](#62-the-rendezvous-sequence-diagram)
   - [6.3 How docking, data and charging work](#63-how-docking-data-and-charging-work)
   - [6.4 Mission states and transitions](#64-mission-states-and-transitions)
   - [6.5 What happens if a rendezvous fails](#65-what-happens-if-a-rendezvous-fails)
   - [6.6 Docking state machine (reference implementation sketch)](#66-docking-state-machine-reference-implementation-sketch)
   - [6.7 Timing, synchronisation and deadlines](#67-timing-synchronisation-and-deadlines)
   - [6.8 Rendezvous performance metrics and tuning](#68-rendezvous-performance-metrics-and-tuning)
   - [6.9 Rendezvous rehearsal plan](#69-rendezvous-rehearsal-plan)
   - [6.10 Rendezvous quick-reference card](#610-rendezvous-quick-reference-card)

### Part II — The Intelligent Layer

7. [The ML Brain: Trajectory Prediction](#7-the-ml-brain-trajectory-prediction)
   - [7.1 Why a single predicted point is the wrong answer](#71-why-a-single-predicted-point-is-the-wrong-answer)
   - [7.2 Model 1 — Wave Glider trajectory prediction](#72-model-1-wave-glider-trajectory-prediction)
   - [7.3 Model 2 — Argo Float surfacing prediction](#73-model-2-argo-float-surfacing-prediction)
   - [7.4 Ensemble and physics-informed forecasting](#74-ensemble-and-physics-informed-forecasting)
   - [7.5 Training data and validation protocol](#75-training-data-and-validation-protocol)
   - [7.6 Model lifecycle and retraining](#76-model-lifecycle-and-retraining)
   - [7.7 Feature engineering reference](#77-feature-engineering-reference)
   - [7.8 Prediction pseudocode (ensemble surfacing forecast)](#78-prediction-pseudocode-ensemble-surfacing-forecast)
   - [7.9 Model registry, config and experiment tracking](#79-model-registry-config-and-experiment-tracking)
   - [7.10 Monitoring models in production](#710-monitoring-models-in-production)
   - [7.11 Prediction reporting contract](#711-prediction-reporting-contract)
   - [7.12 Interpretability and operator trust](#712-interpretability-and-operator-trust)
   - [7.13 Prediction failure-mode analysis](#713-prediction-failure-mode-analysis)
   - [7.14 Dataset governance](#714-dataset-governance)
   - [7.15 A worked prediction example](#715-a-worked-prediction-example)
8. [Planning, Ice Avoidance and Energy](#8-planning-ice-avoidance-and-energy)
   - [8.1 The planning engine: from forecasts to a feasible route](#81-the-planning-engine-from-forecasts-to-a-feasible-route)
   - [8.2 The optimisation objective](#82-the-optimisation-objective)
   - [8.3 Iceberg and sea-ice hazard avoidance](#83-iceberg-and-sea-ice-hazard-avoidance)
   - [8.4 Energy: where the power comes from and where it goes](#84-energy-where-the-power-comes-from-and-where-it-goes)
   - [8.5 Checkpoint generation — reference algorithm](#85-checkpoint-generation-reference-algorithm)
   - [8.6 Cost function detail](#86-cost-function-detail)
   - [8.7 Stand-off sizing and hazard policy](#87-stand-off-sizing-and-hazard-policy)
   - [8.8 Planner output example](#88-planner-output-example)
   - [8.9 Planner test scenarios](#89-planner-test-scenarios)
   - [8.10 Bathymetry and navigation constraints](#810-bathymetry-and-navigation-constraints)
   - [8.11 Planner performance budgets](#811-planner-performance-budgets)
   - [8.12 Annual energy budget — worked numbers](#812-annual-energy-budget-worked-numbers)

### Part III — Software, Data and People

9. [Embedded Software, Health Monitoring and Debugger](#9-embedded-software-health-monitoring-and-debugger)
   - [9.1 The on-board software architecture](#91-the-on-board-software-architecture)
   - [9.2 What the system monitors](#92-what-the-system-monitors)
   - [9.3 The health-monitoring loop](#93-the-health-monitoring-loop)
   - [9.4 The hand-held pre-deployment debugger](#94-the-hand-held-pre-deployment-debugger)
   - [9.5 Event log schema](#95-event-log-schema)
   - [9.6 Watchdog and reset design](#96-watchdog-and-reset-design)
   - [9.7 Flight-software quality practices](#97-flight-software-quality-practices)
   - [9.8 Debugger report template](#98-debugger-report-template)
10. [What We Measure: Data Streams and Products](#10-what-we-measure-data-streams-and-products)
    - [10.1 The data streams](#101-the-data-streams)
    - [10.2 Representative data products](#102-representative-data-products)
    - [10.3 Data lifecycle](#103-data-lifecycle)
    - [10.4 Formats, standards and QC](#104-formats-standards-and-qc)
    - [10.5 Example data record](#105-example-data-record)
    - [10.6 Archive format example (netCDF / CF)](#106-archive-format-example-netcdf-cf)
    - [10.7 QC algorithm catalogue (shore, automated)](#107-qc-algorithm-catalogue-shore-automated)
    - [10.8 Data volumes (illustrative)](#108-data-volumes-illustrative)
   - [10.9 Metadata and provenance](#109-metadata-and-provenance)
   - [10.10 Data access levels](#1010-data-access-levels)
   - [10.11 Data latency and timing budget](#1011-data-latency-and-timing-budget)
11. [The Web Mission Dashboard](#11-the-web-mission-dashboard)
    - [11.1 What the dashboard shows](#111-what-the-dashboard-shows)
    - [11.2 Alerts, override and the human-in-the-loop](#112-alerts-override-and-the-human-in-the-loop)
    - [11.3 Roles and access control](#113-roles-and-access-control)
    - [11.4 Dashboard technology and interfaces](#114-dashboard-technology-and-interfaces)
    - [11.5 API reference (mission server)](#115-api-reference-mission-server)
    - [11.6 Data model](#116-data-model)
   - [11.7 Dashboard UX specifications](#117-dashboard-ux-specifications)
   - [11.8 Dashboard non-functional requirements](#118-dashboard-non-functional-requirements)
   - [11.9 Dashboard accessibility and internationalisation](#119-dashboard-accessibility-and-internationalisation)
   - [11.10 Dashboard smoke-test suite](#1110-dashboard-smoke-test-suite)
   - [11.11 Dashboard screen-by-screen walkthrough](#1111-dashboard-screen-by-screen-walkthrough)
12. [Optional Extension: The Iceberg Tracker](#12-optional-extension-the-iceberg-tracker)
    - [12.1 What it is and what it does](#121-what-it-is-and-what-it-does)
    - [12.2 Data flow into the planner](#122-data-flow-into-the-planner)
    - [12.3 Deployment methods and scientific side-benefits](#123-deployment-methods-and-scientific-side-benefits)
   - [12.4 Tracker engineering and link budget (optional extension)](#124-tracker-engineering-and-link-budget-optional-extension)
   - [12.5 Tracker operations (optional extension)](#125-tracker-operations-optional-extension)

### Part IV — Operations and Justification

13. [Deployment and a Year in the Field](#13-deployment-and-a-year-in-the-field)
    - [13.1 How the devices get into the water](#131-how-the-devices-get-into-the-water)
    - [13.2 The year ahead](#132-the-year-ahead)
    - [13.3 Seasonal operations logic](#133-seasonal-operations-logic)
    - [13.4 Roles and responsibilities](#134-roles-and-responsibilities)
    - [13.5 Operational contingencies playbook](#135-operational-contingencies-playbook)
    - [13.6 Deployment runbook (on-vessel)](#136-deployment-runbook-on-vessel)
    - [13.7 Contact plan and communications schedule (illustrative)](#137-contact-plan-and-communications-schedule-illustrative)
   - [13.8 Season closeout checklist](#138-season-closeout-checklist)
   - [13.9 Emergency procedures](#139-emergency-procedures)
   - [13.10 Winter operations runbook](#1310-winter-operations-runbook)
   - [13.11 Ship-visit choreography and handover](#1311-ship-visit-choreography-and-handover)
   - [13.12 Season wrap-up report template](#1312-season-wrap-up-report-template)
   - [13.13 A day in the life — three operational vignettes](#1313-a-day-in-the-life-three-operational-vignettes)
14. [Expected Benefits and Impact](#14-expected-benefits-and-impact)
    - [14.1 Scientific benefits](#141-scientific-benefits)
    - [14.2 Operational and economic benefits](#142-operational-and-economic-benefits)
    - [14.3 Capability and strategic benefits](#143-capability-and-strategic-benefits)
    - [14.4 Key performance indicators](#144-key-performance-indicators)
   - [14.5 Impact monitoring and evaluation plan](#145-impact-monitoring-and-evaluation-plan)
   - [14.6 Sustainability and environmental responsibility](#146-sustainability-and-environmental-responsibility)
   - [14.7 Capacity building and training plan](#147-capacity-building-and-training-plan)
   - [14.8 Publication and outreach plan](#148-publication-and-outreach-plan)
   - [14.9 Impact pathways](#149-impact-pathways)
15. [Risk Register and Mitigations](#15-risk-register-and-mitigations)
    - [15.1 The register](#151-the-register)
    - [15.2 The risk matrix](#152-the-risk-matrix)
    - [15.3 Escalation and reporting](#153-escalation-and-reporting)
    - [15.4 Subsystem failure catalog](#154-subsystem-failure-catalog)
   - [15.5 Lessons-learned register (template)](#155-lessons-learned-register-template)
16. [Phased Development Roadmap](#16-phased-development-roadmap)
    - [16.1 Timeline at a glance](#161-timeline-at-a-glance)
    - [16.2 Phase details and exit gates](#162-phase-details-and-exit-gates)
    - [16.3 Dependencies between phases](#163-dependencies-between-phases)
   - [16.4 Work breakdown structure and deliverables](#164-work-breakdown-structure-and-deliverables)
   - [16.5 Resourcing sketch (illustrative)](#165-resourcing-sketch-illustrative)
   - [16.6 Risk-retirement curve](#166-risk-retirement-curve)

### Part V — The Engineering Blueprint

17. [Repository Structure (Blueprint)](#17-repository-structure-blueprint)
    - [17.1 Directory layout](#171-directory-layout)
    - [17.2 Module contracts](#172-module-contracts)
    - [17.3 Technology stack](#173-technology-stack)
    - [17.4 Coding standards](#174-coding-standards)
    - [17.5 The Git workflow](#175-the-git-workflow)
    - [17.6 Environments, configuration and secrets](#176-environments-configuration-and-secrets)
    - [17.7 Dependency management and supply chain](#177-dependency-management-and-supply-chain)
    - [17.8 Test strategy](#178-test-strategy)
   - [17.9 Observability: logging, metrics and tracing](#179-observability-logging-metrics-and-tracing)
   - [17.10 Configuration management](#1710-configuration-management)
   - [17.11 Repository governance](#1711-repository-governance)
18. [Getting Started (Developers)](#18-getting-started-developers)
    - [18.1 Prerequisites](#181-prerequisites)
    - [18.2 Quick start](#182-quick-start)
    - [18.3 Running the ML models](#183-running-the-ml-models)
    - [18.4 Running the simulator](#184-running-the-simulator)
    - [18.5 Firmware development](#185-firmware-development)
    - [18.6 CI/CD pipelines](#186-cicd-pipelines)
    - [18.7 Troubleshooting and FAQ](#187-troubleshooting-and-faq)
    - [18.8 API usage examples](#188-api-usage-examples)
    - [18.9 Local full-stack with Docker Compose](#189-local-full-stack-with-docker-compose)
    - [18.10 Extended FAQ](#1810-extended-faq)
   - [18.11 Developer onboarding checklist](#1811-developer-onboarding-checklist)
   - [18.12 Release process](#1812-release-process)
   - [18.13 Development environment reference](#1813-development-environment-reference)

### Reference

19. [Glossary](#19-glossary)
20. [Contributing, Team and References](#20-contributing-team-and-references)
    - [20.1 Contributing](#201-contributing)
    - [20.2 Project team](#202-project-team)
    - [20.3 License and distribution](#203-license-and-distribution)
    - [20.4 References](#204-references)
    - [20.5 Image credits](#205-image-credits)
21. [Appendices](#21-appendices)
    - [Appendix A. Traceability to the concept document](#appendix-a-traceability-to-the-concept-document)
    - [Appendix B. Figure index](#appendix-b-figure-index)
    - [Appendix C. Mermaid diagram index](#appendix-c-mermaid-diagram-index)
    - [Appendix D. File manifest](#appendix-d-file-manifest)
    - [Appendix E. Revision history](#appendix-e-revision-history)
    - [Appendix F. KPI definitions and formulas](#appendix-f-kpi-definitions-and-formulas)
    - [Appendix G. Acronyms](#appendix-g-acronyms)
    - [Appendix H. Simulator scenario catalog](#appendix-h-simulator-scenario-catalog)
    - [Appendix I. Sign-off record](#appendix-i-sign-off-record)
    - [Appendix J. Design-review checklist](#appendix-j-design-review-checklist)
    - [Appendix K. Further reading](#appendix-k-further-reading)
    - [Appendix L. Requirements traceability matrix](#appendix-l-requirements-traceability-matrix)
    - [Appendix M. Interface control summary](#appendix-m-interface-control-summary)
    - [Appendix N. Sample season dataset (excerpts)](#appendix-n-sample-season-dataset-excerpts)
    - [Appendix O. Review meeting calendar](#appendix-o-review-meeting-calendar)
    - [Appendix P. Definition of Done](#appendix-p-definition-of-done)
    - [Appendix Q. Change-log template](#appendix-q-change-log-template)
    - [Appendix R. One-page quick reference card](#appendix-r-one-page-quick-reference-card)
    - [Appendix S. Ten open questions the mission is designed to answer](#appendix-s-ten-open-questions-the-mission-is-designed-to-answer)
    - [Appendix T. Worked numerical ledgers](#appendix-t-worked-numerical-ledgers)
    - [Appendix U. Season simulation trace (condensed)](#appendix-u-season-simulation-trace-condensed)

---

# PART I — THE SYSTEM DESIGN

---

## 1. Executive Summary

### 1.1 The idea in one paragraph

We propose an **indigenous, cooperative polar-ocean observation system** made of two autonomous
devices — a surface **Wave Glider** and an underwater **Argo Float**. The two are not independent
platforms: they are designed to operate together as a **single, self-sustaining mission** in the
Southern Ocean around Antarctica.

- **The Argo Float** spends most of its life underwater on a repeating ~10-day cycle. It sinks to
  roughly **1,000 m**, drifts with deep currents for about nine days while measuring the ocean,
  dives deeper to about **2,000 m**, and then rises while recording a continuous profile of
  temperature, salinity and depth (and optionally dissolved oxygen, chlorophyll and other
  variables). Because radio signals do not pass through water, it stores the data on board. When
  it returns to the surface, it takes a GPS fix and communicates.
- **The Wave Glider** stays on the surface the entire time. It is propelled directly by ocean
  waves (it needs **no engine or fuel**), runs its electronics and sensors on **solar panels**,
  measures the atmosphere and sea surface (wind, air temperature, pressure, humidity, radiation,
  waves), and carries GPS, satellite communications and limited steering. It is the mission's
  surface gateway and floating charging station.
- **The rendezvous is the heart of the concept.** Two machine-learning models continuously
  predict (1) where the waves and currents will carry the Wave Glider and (2) where the submerged
  Argo Float will most likely surface — expressed as **probability zones with uncertainty radii
  and confidence scores, not single points**. A planning engine turns those predictions into a
  sequence of feasible checkpoints, guiding the glider to be waiting in the right patch of ocean.
  When the float surfaces, the glider docks and locks onto it, **offloads the stored scientific
  data and recharges the float's battery from solar power**, while the float can also transmit
  directly to satellite. The glider then sends everything to shore, and the float dives to begin
  its next cycle. Icebergs and sea ice detected on board or from external feeds are **routed
  around automatically**.
- **Ashore**, NCPOR scientists see the entire mission on a live web platform: both vehicle
  positions, predicted tracks and surfacing zones, checkpoints, rendezvous status, ocean and
  weather data, battery levels and communication status, with historical mission tracks and
  automatic emergency alerts. Operators can let the system run fully autonomously or take
  **manual override** and uplink new commands. Before every deployment, an on-site engineer uses
  a rugged **hand-held debugger** on the research vessel to verify that every sensor, battery,
  link and actuator is healthy — so failures are found **on deck, not at sea**.

### 1.2 Quick facts

| Stat | Value | Meaning |
|---|---|---|
| Devices | **2** | one Wave Glider + one Argo Float, operated as one cooperative system |
| Cycle | **~10 days** | Argo dive → profile → surface → rendezvous → repeat |
| Depth | **up to ~2,000 m** | maximum profiling depth |
| Throughput | **30–35 profiles/year** | per float, across all seasons |
| Fuel | **0** | wave propulsion + solar power |
| Ship time | **~1 visit/year** | deployment (and optional end-of-season recovery) |
| Surface coverage | **continuous** | the glider stays on station all year |
| Data paths | **2** | glider gateway + float direct satellite burst |
| Decision layer | **ML + planner** | probabilistic predictions drive checkpoints |

### 1.3 What makes this different

Today, profiling floats and surface vehicles generally operate as **separate assets**. Here they
are deliberately **paired**:

| Without this project | With this project |
|---|---|
| Float battery depletes → asset lost or ship recovery needed | Float recharged **at sea** by the glider at every surfacing |
| Float data trickles through brief, power-hungry satellite windows | Float data offloaded **locally** to the glider, then relayed |
| Float is silent and blind while submerged — nobody knows where it will appear | ML predicts a **confidence-scored surfacing zone**, and the glider waits there |
| Ice is drawn on a chart; humans must react | Ice becomes **no-go polygons** the planner routes around automatically |
| Two unsteerable vehicles meeting in a million km² is "impossible" | **Probability zones + feasible checkpoints** make the meeting routine |

The surface vehicle removes the float's two biggest limits — **energy and communication** — by
meeting it at every surfacing, while the float supplies the deep-ocean measurements the glider
cannot make. Machine learning is used to make that meeting reliable even though **neither
vehicle can be steered like a boat**.

### 1.4 Mission statement, vision and objectives

> **Mission statement.** Sustain an indigenous, fuel-free, two-device observation pair in the
> Southern Ocean that delivers co-located atmospheric and full-depth ocean measurements
> year-round, without a ship on station.

**Vision.** A fleet of cooperative glider–float pairs ringing Antarctica, feeding continuous,
Argo-standard data into national and global climate services — built, operated and continuously
improved by Indian scientists and engineers.

**Objectives (in priority order):**

1. **Data continuity** — deliver 30–35 ocean profiles plus a continuous surface/atmospheric
   record per year, in all seasons.
2. **Energy self-sufficiency** — keep the float alive indefinitely via in-situ recharge.
3. **Safe autonomy** — zero losses to ice; every hazard detected or predicted is avoided.
4. **Human oversight** — every decision is visible, reversible and auditable ashore.
5. **Indigenous capability** — the architecture, software, ML and operations know-how remain
   domestic and reusable.

### 1.5 Scope — in and out

**In scope**

- One Wave Glider + one Argo Float cooperative pair (the "mission unit").
- Embedded software, health monitoring, hand-held debugger.
- Two ML models (glider trajectory, float surfacing) + planning engine.
- Shore mission server: ingest, QC, archive, alerting, command uplink.
- Web mission dashboard with override capability.
- Optional iceberg tracker integration.
- Phased development: shore build → coastal trials → polar pilot → full season.

**Out of scope (explicitly)**

- Fleets of many pairs (the architecture is designed to scale, but v1 is one pair).
- Full under-ice acoustic communication with submerged floats (a future research topic).
- On-site human repair at sea (by design there is no ship after deployment).
- Replacing the international Argo programme — we **complement and feed** it.

### 1.6 Definition of success

The project is successful when, at the end of a full field season:

1. ✅ ≥ 30 float profiles were delivered through a polar winter, with QC-pass rates comparable to
   Argo standards.
2. ✅ ≥ 80 % of rendezvous attempts succeeded (dock + offload), and **no profile was lost** in
   the remainder (satellite fallback worked).
3. ✅ The predicted surfacing zones contained the true surfacing point at the advertised
   confidence (validated coverage ≈ stated confidence).
4. ✅ The glider never entered an ice stand-off zone; no vehicle was lost.
5. ✅ The dashboard supported real operator decisions and at least one successful manual
   override was exercised and audited.
6. ✅ All code, models, data formats and operational records are archived and reusable for the
   next mission.

### 1.7 Non-technical FAQ

| # | Question | Short answer |
|---|---|---|
| 1 | What exactly are you building? | Two small ocean robots — one that surfs on waves and one that dives 2 km — that work as a team in the Southern Ocean for a year, measuring the ocean and the weather without any fuel. |
| 2 | Why both robots together? | Alone, each has a fatal limit: the diver runs out of battery, the surfer cannot see underwater. Together, the surfer recharges the diver and carries its data home — every ten days, automatically. |
| 3 | How do they find each other in a million square kilometres of ocean? | Machine learning predicts *regions* (not exact points) where the diver will surface, with a confidence score. The surfer sails to wait inside that region. When the forecast improves, the region shrinks. |
| 4 | What happens in winter, in the dark, with ice everywhere? | The system slows down deliberately: fewer transmissions, less motion, data stored safely on board. It never risks itself for a meeting; every measurement still gets home through a backup satellite channel. |
| 5 | Is anyone driving? | Mostly the software, watched by NCPOR operators on a live dashboard. Humans can adjust or take over at any time, and every decision is recorded. |
| 6 | What does this give India? | Year-round data from a part of the ocean that controls our monsoon and our climate; domestic expertise in ocean robotics and AI; and a reusable platform for more missions. |
| 7 | What could go wrong? | Ice, storms, battery exhaustion, or hardware failure. Each has a designed answer and a fallback (Section 15) — the design's goal is that nothing that goes wrong loses the science. |
| 8 | When will it fly — sorry, swim? | Phased: shore build → coastal trials → a short polar pilot → a full season. Each phase must pass its gate before the next begins (§16). |

---

## 2. Why: The Southern Ocean Challenge

### 2.1 A remote ocean that controls the planet's climate

The Southern Ocean — the ring of water surrounding Antarctica — is disproportionately important
to Earth's climate. It is where vast quantities of heat and carbon dioxide are absorbed from the
atmosphere, where the global ocean **overturning circulation** begins, and where some of the
densest, deepest water masses on Earth are formed.

| Fact | Why it matters to this project |
|---|---|
| The **Antarctic Circumpolar Current (ACC)** is the strongest current system on Earth | It connects every major ocean basin and is the dominant horizontal flow our vehicles must ride and predict |
| The Southern Ocean absorbs a large fraction of anthropogenic **heat and CO₂** | Observing it is observing the planet's thermostat — but data are scarce |
| **Antarctic Bottom Water** and other dense water masses form here | Winter observations here directly constrain global overturning estimates |
| Changes here propagate globally | Affects monsoons, sea-level rise and weather for billions — including India's monsoon |

Changes in the Southern Ocean propagate globally, affecting monsoons, sea-level rise and regional
weather for billions of people — a direct scientific stake for India and NCPOR.

<p align="center">
  <img src="assets/images/global_currents_map.jpg" alt="Global ocean-current system" width="70%"/>
</p>

*The global conveyor of ocean currents — the Southern Ocean is the hub that connects every basin, and the operating box of this mission sits on its busiest ring.*

📷 **Plate 2.1 — The Antarctic Circumpolar Current** circles the continent and links the
Atlantic, Indian and Pacific Oceans. Understanding water movement here is essential to
predicting where drifting vehicles will travel. *(Schematic map, credited in §20.5.)*

<p align="center">
  <img src="assets/images/acc_currents.png" alt="Antarctic Circumpolar Current schematic" width="60%"/>
</p>

📷 **Plate 2.2 — The operating environment**: icebergs, sea ice, cold temperatures, large swells
and long periods of darkness. Conditions that are expensive — and sometimes impossible — for
crewed ships to remain in. *(Southern Ocean iceberg A-23a, credited in §20.5.)*

<p align="center">
  <img src="assets/images/iceberg_a23a.jpg" alt="Iceberg A-23a in the Southern Ocean" width="70%"/>
</p>

### 2.2 Why it is hard to observe

Despite its importance, the Southern Ocean remains one of the **least sampled ocean regions on
Earth**. The reasons are practical and unforgiving:

| Difficulty | Consequence for observation |
|---|---|
| **Extreme remoteness** | Stations are days to weeks of steaming from the nearest port; every ship-based measurement carries a very high cost in ship time, fuel and crew. |
| **Brutal weather** | Persistent strong winds, storms, cold, ice and heavy seas limit when, where and for how long ships can safely work. |
| **Sea ice and icebergs** | Much of the region is ice-covered or ice-infested for large parts of the year; ice is both a navigation hazard and a moving, changing obstacle. |
| **Sparse, seasonal data** | Observations are biased to summer and to the narrow strips of ocean expeditions can reach. Winter, under-ice and open-ocean time-series are particularly scarce. |

📷 **Plate 2.3 — The marginal ice zone**, where open water meets broken sea ice. It is both
scientifically valuable and hazardous — exactly the kind of region where autonomous, ice-aware
platforms add the most value. *(West Antarctic Ice Sheet marginal ice zone, credited in §20.5.)*

<p align="center">
  <img src="assets/images/marginal_ice_zone.jpg" alt="Marginal ice zone, West Antarctica" width="70%"/>
</p>

📷 **Plate 2.4 — Tabular icebergs such as A-23a** drift for years and can be tens of kilometres
long. Tracking them as moving hazards is essential for any autonomous surface route. *(Credited
in §20.5.)*

<p align="center">
  <img src="assets/images/iceberg_a23a_2.jpg" alt="Tabular iceberg A-23a" width="70%"/>
</p>

### 2.3 The shift to autonomous platforms

Two classes of mature autonomous platform have already transformed ocean observation:

| Platform | What it does | Why it matters |
|---|---|---|
| **Argo profiling floats** | Thousands now measure the upper 2,000 m of the world ocean on ~10-day cycles | Proven deep-ocean coverage at low cost per profile |
| **Wave Gliders / ASVs** | Remain at sea for many months, powered only by waves and sunlight | Persistent surface presence, no fuel logistics |

They are cheap per observation compared with ships, work through storms, and return data all
year round.

> 💬 **In plain language.** An Argo float is a small, torpedo-shaped robot that sinks on purpose,
> drifts in the deep for over a week measuring the ocean, and pops back up to report. A Wave
> Glider is a surfboard-sized solar robot that surfs indefinitely without fuel, converting the
> up-and-down motion of waves into forward motion. Both already exist and are proven at sea;
> **this project combines and connects them in a new, indigenous way.**

### 2.4 The gap this project fills

Operating these platforms separately still leaves unsolved problems in polar waters:

| # | The limitation | Why it matters | How this project answers |
|---|---|---|---|
| 1 | **Energy is finite on a float** | The float's battery powers every pump stroke, sensor reading and satellite burst. Battery life dictates mission length; once depleted, the asset is lost or must be recovered by ship. | The glider recharges the float **at sea** at every rendezvous |
| 2 | **Communication only works at the surface, briefly** | A float can only transfer data (and get position fixes) during short surfacing windows. Satellite airtime is power-hungry, and at high latitude bandwidth and coverage can be limited. | The glider is a **permanent surface gateway**: high-volume local offload, then relay |
| 3 | **Neither vehicle can be driven directly to a target like a boat** | A float is at the mercy of currents while submerged; a Wave Glider moves only as the waves and currents allow, with limited steering. Making two such vehicles meet in a million square kilometres of ocean is non-trivial. | **ML-predicted probability zones** + feasible checkpoint planning turn the meeting into a solvable optimisation |
| 4 | **Ice can destroy or trap equipment** | A float surfacing under pack ice, or a glider drifting onto an iceberg, can be lost. Ice information must actively shape navigation, not just be displayed. | Hazards become **no-go polygons**; the planner routes around them or holds/aborts |

**Our answer:** make the Wave Glider a persistent, solar-powered surface **gateway and charging
station** that actively rendezvouses with the float at every surfacing, and surround that
rendezvous with **machine-learning prediction, uncertainty-aware planning and ice avoidance**.
The result is a system that keeps delivering deep-ocean profiles and surface/atmospheric
measurements for months or seasons **without a ship present**.

### 2.5 Target science questions

The mission is designed to make specific, currently-unanswered questions tractable:

| # | Science question | Data the system supplies |
|---|---|---|
| Q1 | How do winter heat and carbon fluxes evolve in a data-sparse sector of the Southern Ocean? | Continuous surface meteorology + repeated full-depth T/S profiles through winter |
| Q2 | How do mixed-layer depth and water-mass properties respond to storm passages in the marginal ice zone? | Glider storm records co-located with float profiles at the same patch of ocean |
| Q3 | What are the actual drift statistics of the deep currents at 1,000–2,000 m here? | Every float cycle is a Lagrangian drift experiment, labelled with its surfacing fix |
| Q4 | How do large tabular icebergs move, and what is their local hazard footprint? | Optional tracker records + planner hazard logs |
| Q5 | How well can machine-learned drift forecasts beat climatology in this region? | Prediction-skill diagnostics computed ashore from every cycle |

### 2.6 The observing-system landscape: where this project sits

To justify the investment, it helps to place the project in the existing landscape of Southern
Ocean observing.

| Existing capability | Strength | Gap that remains | How this project complements |
|---|---|---|---|
| Research-vessel expeditions (e.g. NCPOR's own polar cruises) | Calibrated, multi-parameter, full-depth sampling | Summer-biased, narrow tracks, expensive | Provides the year-round backbone between expeditions |
| Core Argo array | Global coverage of the upper 2,000 m | Standard floats cannot be recharged; polar coverage thinner; surface time minimal | Adds recharge + local offload + ice-aware surfacing |
| Biogeochemical (BGC) Argo | Oxygen, chlorophyll, pH, nitrate globally | Same energy/communication limits | Same extensions apply; co-located surface forcing data |
| Wave Glider / ASV missions | Long-endurance surface meteorology | Typically operated alone; no deep-water column | Pairs it with a profiler for co-located 4-D records |
| Moored buoys (where they exist) | Continuous point time-series | Extremely sparse in the Southern Ocean; fixed location | Adds spatial (drifting) coverage and mobility |
| Satellite altimetry / SST / sea-ice products | Synoptic, all-weather-ish views | Only surface; no subsurface ground truth | Provides the in-situ ground truth and calibration |
| Iceberg tracking (existing programmes) | Tracked berg positions | Usually science-only, not fed to a planner | Operationalises the same data for hazard avoidance |

**Positioning statement.** This project does not replace any of the above — it *connects* them:
the paired vehicles deliver the **year-round, co-located, energy-sustainable** observations that
ships cannot afford, satellites cannot see, and standard floats cannot sustain.

### 2.7 Reference operating region (illustrative)

| Parameter | Value | Rationale |
|---|---|---|
| Sector | Indian Ocean sector of the Southern Ocean | Aligns with NCPOR polar programmes and logistics |
| Latitude band | ~55°S–70°S (ice-dependent) | Covers ACC front, seasonal ice zone, MIZ |
| Depth | ≤ 2,000 m float rating | Upper ocean + water-mass formation depths |
| Ice season | Autumn-to-spring advance | Drives the seasonal operations logic (§13.3) |
| Cyclone/storm exposure | Year-round, winter peaks | Sizing case for docking and energy (§8.4) |

> The exact operating box is set by NCPOR around sea-ice conditions and ship logistics at the
> time of the Phase 3 pilot (§16.2) — the system itself is region-agnostic.

### 2.8 Stakeholders and engagement

| Stakeholder | Interest | Engagement cadence |
|---|---|---|
| NCPOR leadership | Strategic value, programme fit | Phase-gate reviews; annual brief |
| NCPOR scientists | Data quality, science priorities | Science working group; seasonal reviews |
| Mission operators | Safe, predictable operations | Daily ops stand-up (in season); drills |
| Engineering / ML team | System health, model quality | Weekly engineering sync; on-call rotation |
| Ship & logistics partners | Deployment slots, recovery | Lead-time planning (≥ 1 season ahead) |
| National data centres | Data delivery standards | Delivery pipeline agreed at P2 |
| International Argo community | Interoperability, standards alignment | Via data centre; publications |
| Indian ocean-tech ecosystem | Capability building, spinoffs | Technical workshops; open architecture docs |

**Communication rules**

- One dashboard for ground truth — every stakeholder reads the same mission state (§11).
- Alerts escalate by level (§15.3), never by audience — no silent channels.
- Science requests flow through the sampling-priority process (§13.4) rather than direct vehicle
  commands, preserving the audit trail.

---

### 2.9 Design alternatives considered

The blueprint is a series of decisions. The main alternatives evaluated (and why they were not
chosen) are recorded here for reviewers — this is the "why not?" register.

| Decision | Alternative considered | Why not chosen |
|---|---|---|
| Pair Wave Glider + Argo Float | A single multi-purpose vehicle doing both jobs | No existing platform both persists at the surface AND profiles to 2,000 m; a new hull would forfeit the maturity of both proven platforms |
| Rendezvous-based recharge | Larger float battery / no recharge | Battery growth has hard physical limits (weight, buoyancy); without recharge the float still dies at end-of-life — the core problem remains |
| Acoustic comms float↔glider | Keep constant contact underwater | Acoustic bandwidth/power in polar conditions is tiny and costly; the mission needs bulk transfer only at the surface — the rendezvous design matches the need |
| Point-prediction navigation | "Steer to the predicted lat/lon" | A single point is wrong and overconfident in a chaotic ocean (§7.1); probabilistic zones cost nothing extra and are honest |
| Ship-tended operations | More ship time, no autonomy | Ship days are the dominant cost and the dominant risk to people (§2.2); autonomy is the point |
| New bespoke shore software | Off-the-shelf fleet-management SaaS | Data standards (Argo), security (§4.5) and indigenous capability (§14.3) favour in-house; the dashboard scope is modest (§11) |
| Full ice-class hull | Armour the glider against ice | Armour cannot make an iceberg collision survivable; avoidance is the only credible strategy (§8.3) |

**Decision rule used.** Prefer the design that retires the most risk per rupee while keeping the
science identical — every alternative above was scored against that rule.

### 2.10 Programme context within NCPOR

This project sits inside a wider national polar programme and is designed to strengthen it.

| NCPOR programme pillar | How this mission contributes |
|---|---|
| Southern Ocean / Antarctic expeditions | Adds a year-round observing backbone between ship seasons, and a testbed for autonomous ops at the ice edge |
| Indian Argo participation | Extends India's Argo contribution with recharge, co-located surface forcing and ice-aware operations — data flow into the same GDAC pipeline |
| Monsoon and climate research | Delivers the winter Southern Ocean fluxes that ocean models of the Indian Ocean sector most lack (§2.5 Q1) |
| Polar technology development | Grows indigenous capability in ocean robotics, satellite comms at high latitude, and ML for operational oceanography |
| International collaboration | Interoperates with Argo, ice-charting services and Antarctic logistics partners; a natural joint-work platform |

**Programme positioning statement.** The mission is deliberately *small enough to be achievable
and big enough to matter*: two vehicles, one season at a time, one sector of the Southern Ocean —
but with every interface, dataset and lesson reusable at fleet scale (§16.6 scaling note).

**Lineage.** The concept reuses two internationally proven platform families (§5) and Argo-grade
sensors and QC (§10), adding only the pieces that are genuinely new: the recharge rendezvous, the
ML drift prediction, and the integrated planning stack (§8). Nothing else is invented — everything
else is integrated.

> A reviewer asked: *"What is the single hardest part?"* Not the docking, not the ML. It is
> operating **continuously and unattended through one full winter** — every other difficulty is a
> means to that end.

## 3. The Concept at a Glance

> One surface robot and one diving robot, designed as a pair. The glider patrols, measures the
> atmosphere, harvests sunlight and predicts where it can go; the float dives, measures the
> ocean and stores the data. Every ten days they meet at the surface to exchange data and
> energy, report to satellite, and start again.

### 3.1 The twelve elements of the solution

The full proposal is organised into twelve elements. Each is explained in detail later in this
document; this list is the map for the rest of the blueprint.

| # | Element | One-line description | Detailed in |
|---|---|---|---|
| 1 | **Two-device autonomous architecture** | A Wave Glider and an Argo Float operated as one cooperative system | §3–§5 |
| 2 | **Argo Float underwater observation** | ~10-day dive cycles measuring temperature, salinity, depth and optional bio-geo variables; local storage | §5.2, §6 |
| 3 | **Wave Glider surface platform** | Wave-driven, solar-powered surface gateway measuring atmospheric parameters and acting as charging station | §5.1 |
| 4 | **ML glider trajectory prediction** | Predicts the glider's probable future path, reachable region and uncertainty from waves, currents, wind and history | §7.2 |
| 5 | **ML float surfacing prediction** | Predicts a probable surfacing zone with an uncertainty radius and confidence score, not an exact point | §7.3 |
| 6 | **Feasible trajectory & checkpoints** | A planning engine combines both predictions into an adaptive sequence of reachable waypoints | §8.1 |
| 7 | **Autonomous rendezvous planning** | Maximises the probability of meeting the float while minimising travel and energy; supports manual or autonomous control | §8.1–8.2 |
| 8 | **Iceberg & ice-hazard avoidance** | Detected hazards are turned into no-go polygons; routes are re-planned around them or the safest alternative is chosen | §8.3 |
| 9 | **Rendezvous, data transfer & recharging** | Dock, lock, offload data and recharge the float; then both vehicles resume their missions | §6 |
| 10 | **Embedded OS & health monitoring** | Shared on-board software monitors sensors, battery, GPS, comms, storage and faults, with time-stamped logs; a hand-held debugger for pre-deployment checks | §9 |
| 11 | **Web-based mission monitoring** | A live dashboard for NCPOR: positions, predictions, data, status, alerts, history — with override capability | §11 |
| 12 | **Iceberg tracker (optional)** | A small tracker deployed on an iceberg/ice floe feeds live hazard maps directly into the planner | §12 |

### 3.2 Design principles

```mermaid
mindmap
  root((Design Principles))
    Principle 1
      Work with the ocean not against it
        waves give propulsion
        sunlight gives electricity
        predict natural drift
        plan within it
    Principle 2
      Predict regions not points
        probability zones
        uncertainty radii
        confidence scores
    Principle 3
      Close the loops at sea
        energy loop closed by recharge
        data loop closed by offload
        no recovery ships needed
    Principle 4
      Safety and human stay in the loop
        ice avoidance
        automatic alerts
        manual override
        pre-deployment verification
```

**PRINCIPLE 1 — Work with the ocean, not against it.**
Waves provide propulsion and sunlight provides electricity. We predict the vehicles' natural
drift and plan within it, rather than pretending the glider can travel anywhere on demand.

**PRINCIPLE 2 — Predict regions, not points.**
The ocean is chaotic. Both ML models output probability zones, uncertainty radii and confidence
scores, and the planner reasons about those regions.

**PRINCIPLE 3 — Close the energy and data loops at sea.**
Recharging and data offload happen at the rendezvous, removing the need for recovery ships and
the limits of short satellite windows.

**PRINCIPLE 4 — Safety and the human stay in the loop.**
Ice avoidance, automatic alerts, manual override and on-deck pre-deployment verification keep
people in control of an otherwise autonomous system.

### 3.3 Who does what

| Role | Wave Glider (surface) | Argo Float (underwater) | Shore / NCPOR |
|---|---|---|---|
| **Motion** | Wave-propelled, limited steering; drifts with surface currents | Controls buoyancy to dive/rise; otherwise drifts with deep currents | Issues routes, checkpoints and overrides |
| **Measurement** | Atmosphere + sea surface + waves | Temperature, salinity, depth profiles; optional O₂, chlorophyll | Receives, QC-checks, archives and visualises data |
| **Power** | Solar panels + battery; propulsion free | Battery, recharged at rendezvous | Monitors battery and mission status |
| **Comms** | GPS + satellite gateway + short-range link | Surface-only short-range link + optional satellite burst | Satellite link to the glider; web dashboard |
| **Cadence** | Continuous surface station | ~10-day dive/profile/surface cycle | Live monitoring; interventions by exception |
| **Decision** | Executes checkpoints; on-board hazard response | Executes dive/rise; ice-aware surfacing logic | ML predictions, planning engine, approvals |

### 3.4 The system as a mindmap

```mermaid
mindmap
  root((Cooperative Polar-Ocean<br/>Observation System))
    Vehicles
      Wave Glider
        wave propulsion
        solar power
        met sensors
        satcom gateway
        docking hardware
      Argo Float
        buoyancy engine
        CTD profile
        on-board storage
        backup sat burst
    Intelligence
      Model 1 glider trajectory
      Model 2 surfacing zone
      Planning engine
      Ice avoidance
    Rendezvous
      dock and lock
      data offload
      battery recharge
    Shore
      mission server
      ML retraining
      live dashboard
      alerts and override
    Optional
      iceberg tracker
```

### 3.5 The twelve elements — design detail

Each element of §3.1 is specified here: components, interfaces and acceptance criteria. This is
the checklist the phase gates (§16.2) are built from.

#### Element 1 · Two-device autonomous architecture

| Aspect | Specification |
|---|---|
| Components | One Wave Glider, one Argo Float, one shore mission server, one dashboard |
| Pairing contract | The pair shares one mission identity; either vehicle can continue alone under its fallback rules (§6.5) |
| Interfaces | Rendezvous link (dock), satellite relay, shore uplink/downlink (§4.3) |
| Acceptance | Both vehicles operate for ≥ 1 season from one deployment; either vehicle survives loss of the other's support for ≥ 2 cycles |

#### Element 2 · Argo Float underwater observation

| Aspect | Specification |
|---|---|
| Cycle | ~10 days: descend → park (~1,000 m) → drift ~9 days → deep profile (~2,000 m) → ascent with continuous CTD sampling |
| Payload | CTD core; optional O₂, chlorophyll-a, backscatter, nitrate, pH |
| Storage | ≥ 2 full profiles on board, never overwritten before delivery |
| Ice awareness | Surfacing logic informed by ice conditions (§8.3) |
| Acceptance | 30–35 QC-passing profiles per year (§14.4) |

#### Element 3 · Wave Glider surface platform

| Aspect | Specification |
|---|---|
| Propulsion | Wave-driven fin rack; rudder steering; no fuel |
| Power | Solar + battery; sized for winter with margin (§8.4) |
| Payload | Wind, air temperature, pressure, humidity, radiation, waves, SST |
| Gateway role | Satellite relay for the pair; store-and-forward buffer |
| Acceptance | Continuous met record through the mission with ≥ 95 % uptime of scheduled samples (energy-adjusted) |

#### Element 4 · ML glider trajectory prediction

| Aspect | Specification |
|---|---|
| Outputs | Most probable trajectory, reachable set per time horizon, uncertainty envelope |
| Inputs | Position/heading, track history, wave/wind/current forecasts, solar state, steering limits (§7.2) |
| Validation | Held-out track segments; reachable-set coverage vs confidence (§7.5) |
| Acceptance | Reachable-set coverage at advertised confidence; envelope grows monotonically with horizon |

#### Element 5 · ML float surfacing prediction

| Aspect | Specification |
|---|---|
| Outputs | Surfacing ellipse, uncertainty radius (km), confidence score, time window |
| Inputs | Last fix, dive profile, depth-resolved currents, drift climatology (§7.3) |
| Validation | Held-out floats: true surfacing point inside the zone at advertised confidence |
| Acceptance | Coverage ≈ confidence ± 5 %; radius shrinks as information improves |

#### Element 6 · Feasible trajectory & checkpoints

| Aspect | Specification |
|---|---|
| Generation | Candidates sampled downwave/downcurrent, filtered by reachability and hazard clearance, scored by progress + probability + energy (§8.5) |
| Cadence | 6-hourly replan; 1-hourly inside D−1 day; event-triggered on new data |
| Output | Ordered checkpoint list with ETAs and types (transit / loiter / hold) |
| Acceptance | Every checkpoint in the reachable set and outside stand-offs; plan never enters ice |

#### Element 7 · Autonomous rendezvous planning

| Aspect | Specification |
|---|---|
| Objective | Maximise P(successful rendezvous), minimise travel & energy (§8.2) |
| Arrival policy | Arrive early (≥ half-day) and loiter inside the surfacing zone |
| Control modes | Full autonomy / operator-adjusted / manual override (§11.2) |
| Acceptance | ≥ 80 % rendezvous success across a season; every failure falls back without data loss |

#### Element 8 · Iceberg & ice-hazard avoidance

| Aspect | Specification |
|---|---|
| Sources | On-board radar/optical, ice charts, iceberg analyses, optional tracker (§8.3) |
| Representation | Hazard polygon + safety stand-off, refreshed per source cadence |
| Response ladder | Re-route → hold → divert → abort, with operator alerting |
| Acceptance | Zero stand-off violations; zero ice-related losses |

#### Element 9 · Rendezvous, data transfer & recharging

| Aspect | Specification |
|---|---|
| Sequence | Approach → loiter → surfacing → dock & lock → offload → recharge → release (§6) |
| Data-first rule | Profiles transfer in the first minutes; charging to a safe target SOC afterwards (§6.3) |
| Fault handling | Retry once, then satellite fallback; never leave the pair coupled on fault |
| Acceptance | Lock confirmation sensor-based; release on command or automatically on fault |

#### Element 10 · Embedded OS & health monitoring

| Aspect | Specification |
|---|---|
| Architecture | Shared layered stack: hardware → embedded OS → mission apps + shared core (§9.1) |
| Monitoring | Sensors, battery, GPS, comms, storage, mission state, system errors (§9.2) |
| Logging | Time-stamped event log on both vehicles, synced to shore |
| Debugger | Hand-held pre-deployment gate with pass/fail per subsystem (§9.4) |
| Acceptance | Every fault attributable to a layer (sensor/comm/power/control); debugger gate blocks any bad deployment |

#### Element 11 · Web-based mission monitoring

| Aspect | Specification |
|---|---|
| Views | Live map, tracks, predictions, checkpoints, data products, health, alerts, audit (§11.1) |
| Control | Accept / adjust / override, role-gated and fully audited (§11.2–11.3) |
| Availability | Works in degraded mode during satellite outages |
| Acceptance | Operators complete the §13.5 playbook responses entirely through the dashboard |

#### Element 12 · Iceberg tracker (optional)

| Aspect | Specification |
|---|---|
| Hardware | Ruggedised GPS/GNSS beacon + satellite modem, solar-assisted battery |
| Product | Tracked hazard with predicted drift path and exclusion zone (§12) |
| Integration | Polygons injected straight into the planning engine |
| Acceptance | Tracker report cadence met; planner consumes polygons without operator action |

### 3.6 Concept of operations — a season in the life

A narrative walkthrough of one full field season, tying every section of this blueprint
together. *(Illustrative — timings, positions and numbers are representative, not commitments.)*

**Late November — deployment cruise.**
The research vessel reaches the operating box around 62°S. The on-site engineer runs the
hand-held debugger on both vehicles (§9.4): sensors respond, pumps cycle, the short-range link
passes a pairwise test on deck, both modems register with the shore gateway. The debugger app
records "ALL SYSTEMS READY" for each vehicle, signed by the engineer. The float goes over the
side by crane, then the glider, ~200 m apart. Within the hour both vehicles report GPS fixes and
first telemetry; the dashboard flips both cards to OPERATIONAL. The ship departs.

**December — commissioning.**
The float executes its first 10-day cycle: descend to 1,000 m, drift, dive to 2,000 m, ascent
with continuous CTD sampling. The glider patrols, logging wind, waves and SST every minute. The
mission server ingests the first profiles, applies QC (§10.7), and archives them. Model 2's
first surfacing-zone forecast is wide — a 60 km ellipse — and honest about it. The planner lays
checkpoints that ride the currents. On day 10 the float surfaces 4 km from the ellipse centre;
the glider, loitering inside the zone, docks on the first attempt. Profiles transfer in four
minutes; charging tops the float from 61 % to 82 % over the following hour. The combined dataset
relays to shore the same afternoon. Scientists begin examining the first winter-adjacent
profiles from a sector their ships rarely reach this late in the year.

**January–March — routine operations.**
The cycle repeats every ten days. Coverage statistics accumulate: after 9 cycles, the 80 %
zones contain the surfacing point 7 times — 78 % coverage, within tolerance. The zone radius has
shrunk from 60 km to 25 km as the model learned this float's drift habits. One storm in
February: 9 m seas, docking envelope exceeded. The pair never attempts the dock; the float
bursts its essentials and dives; the full profile transfers next cycle. No data lost. The
dashboard shows the decision and its reason — operators do nothing, by design.

**April–June — winter onset.**
Sea-ice charts show the marginal ice zone advancing toward the box. The planner biases
checkpoints north and tightens stand-offs (§8.7). Solar yield falls; the energy planner begins
load-shedding (§8.4): satellite passes drop from three to one per day, met sampling from 1-minute
to 10-minute. The glider battery walks down to 38 % during the darkest week — inside the
shedding band, above the critical floor. Science cadence slows but never stops; profiles
accumulate on board and drain in the daily pass.

**July–August — deep winter.**
The box is largely inside the ice edge. The float's ice-aware surfacing logic holds it
subsurface once rather than surface under pack ice. The glider rides the ice edge, holding clear
of every polygon. For three cycles there is no rendezvous — the float's direct bursts carry
decimated profiles, and the full datasets wait. When a polynya opens in early August, the
planner routes the glider into it; the pair meets for the first time in 40 days and the backlog
offloads completely.

**September–October — recovery decision.**
Daylight returns; solar yield climbs; the battery recovers to 85 % by mid-September. NCPOR
decides between recovery, servicing and a second year. The closeout checklist (§13.8) is run:
all data archived, logs pulled, models retrained on 35 labelled cycles, season report drafted.
If the mission continues, the same pair — batteries topped, storage cleared, software updated
via validated uplink (§17.10) — sails into its second season without a ship visit.

**What this narrative demonstrates.** Every mechanism in this blueprint exists to make this
story ordinary: predictions that are honest about uncertainty, plans that respect physics, ice
that is avoided by construction, and fallbacks that make every failure an inconvenience rather
than a loss.

### 3.7 Assumptions register

The blueprint rests on stated assumptions. If an assumption changes, the affected design
decisions are revisited — this register is reviewed at every phase gate.

| ID | Assumption | Impact if false | Owner |
|---|---|---|---|
| A-01 | Wave Glider-class vehicles can operate in the target sea states for a season | R1 missed rendezvous; revisit transit/loiter margins | Engineering |
| A-02 | A polar-capable satellite constellation offers the assumed pass cadence | R6 comm outages; revisit contact plan and power budget | Ops |
| A-03 | Solar yield follows the seasonal model (Fig. 18) with safe margin | R4 insufficient power; re-size array or reduce cadence | Engineering |
| A-04 | A compliant dock mechanism can capture the float within `SELL` sea states | R5 docking; revisit mechanism in P1/P2 | Mechanical |
| A-05 | Depth-resolved current forecasts are accurate enough to train/predict drift | R2 prediction error; strengthen climatology prior | ML |
| A-06 | Ice chart products arrive daily in the operating box | R3 ice encounter; widen stand-offs or add sensing | Ops |
| A-07 | Float CTD and glider sensors meet Argo-grade accuracy | Data quality KPIs; revisit payload in P1 | Science |
| A-08 | One ship visit per season is schedulable | Whole ops concept; fall back to assisted ops | Programme |
| A-09 | NCPOR IT can host the mission server and dashboard at required availability | R9 command safety + ops; fall back to cloud hosting | Engineering |
| A-10 | Regulatory and environmental approvals for deployment are obtainable | Programme viability; begin early | Programme |

---

## 4. System Architecture and End-to-End Flow

### 4.1 The whole system in one picture

The system spans three worlds: the **vehicles at sea**, a **satellite relay overhead**, and the
**NCPOR mission-control systems on shore**. The figure below shows how data, commands and energy
move between them.

🖼️ **Figure 1 — Overall architecture** (generated by `scripts/gen_figures.py`):

<p align="center">
  <img src="assets/figures/fig_01_system_architecture.png" alt="System architecture diagram" width="95%"/>
</p>

The same architecture as editable Mermaid (kept in sync with the figure — edit both):

```mermaid
flowchart TB
    subgraph SEA["AT SEA — the mission unit"]
        WG["WAVE GLIDER<br/>surface sentinel · gateway ·<br/>charging station"]
        AF["ARGO FLOAT<br/>deep-ocean profiler<br/>~10-day cycle · ~2,000 m"]
        ICE["ICE HAZARDS<br/>icebergs · sea ice"]
        WG <-- "rendezvous:<br/>data offload + recharge<br/>short-range link" --> AF
        ICE -. "avoided via<br/>no-go polygons" .-> WG
        ICE -. "ice-aware<br/>surfacing logic" .-> AF
    end

    subgraph SAT["SATELLITE CONSTELLATION"]
        SATL["polar-capable relay<br/>(e.g. Iridium-type)"]
    end

    subgraph SHORE["SHORE — NCPOR MISSION CONTROL"]
        SRV["mission server<br/>decode · QC · archive"]
        ML["ML models 1 & 2<br/>trajectory + surfacing zones"]
        PLN["planning engine<br/>checkpoints · rendezvous · ice"]
        DASH["live dashboard<br/>digital twin · alerts · override"]
        ENV["environmental feeds<br/>currents · weather · ice charts"]
        ENV --> ML --> PLN --> SRV
        SRV --> DASH
        PLN -- "commands · checkpoints<br/>overrides" --> SATL
    end

    WG -- "science + health downlink" --> SATL --> SRV
    AF -. "backup burst<br/>brief surface windows" .-> SATL
    SATL -- "command uplink" --> WG
```

### 4.2 Following the data path

```mermaid
flowchart LR
    A["① COLLECT<br/>float stores profiles<br/>glider logs met data"] -->
    B["② RENDEZVOUS<br/>short-range handover<br/>+ recharge"] -->
    C["③ RELAY<br/>glider forwards dataset<br/>+ health telemetry"] -->
    D["④ PROCESS & PREDICT<br/>decode · QC · archive<br/>ML + planner run"] -->
    E["⑤ DISPLAY & DECIDE<br/>live dashboard · alerts<br/>accept or intervene"] -->
    F["⑥ COMMAND<br/>checkpoints / overrides<br/>uplinked via satellite"]
    F -. "loop closes" .-> A
```

1. **Collect.** The float stores ocean profiles during its dive; the glider logs atmospheric and
   sea-surface data continuously.
2. **Rendezvous.** At the surface, the float hands stored data to the glider over the short-range
   link and is recharged; it may also send a short burst directly to satellite as a backup.
3. **Relay.** The glider forwards the combined dataset and both vehicles' health telemetry over
   the satellite link to the shore gateway.
4. **Process & predict.** The mission server decodes, quality-controls and archives the data; the
   ML models and planning engine produce trajectories, surfacing zones and checkpoints using
   current/forecast currents, weather and ice information.
5. **Display & decide.** The web dashboard shows the live mission; alerts are raised
   automatically; operators may accept the autonomous plan or intervene.
6. **Command.** New checkpoints, mission changes or overrides travel back up via satellite and
   are executed by the vehicles — **closing the loop**.

<p align="center">
  <img src="assets/figures/fig_02_operational_flow.png" alt="End-to-end operational flow with feedback loop" width="85%"/>
</p>

*Figure 2 — the operational loop as a diagram: collect → rendezvous → relay → predict → plan → command, with the data-driven feedback that closes the cycle every ten days.*

### 4.3 Communications: what can talk, when

| Link | When available | Purpose | Fallback if lost |
|---|---|---|---|
| **Float ↔ Glider** (short-range radio / near-field) | Only during a surface rendezvous | High-volume transfer of stored profiles; charging control; no satellite airtime cost | Float bursts essentials directly to satellite |
| **Glider ↔ Satellite** | Anytime at the surface | Primary science & health downlink; command & override uplink; works at high latitude via a polar-capable constellation (e.g. Iridium-type) | Store-and-forward buffer on the glider |
| **Float ↔ Satellite** (direct) | Brief surface windows | Backup burst if a rendezvous is missed, ensuring no profile is ever lost | Next cycle retry; data remain on board |
| **Shore ↔ Dashboard** | Continuous (terrestrial) | NCPOR scientists and operators view the mission and authorise commands | — (shore infrastructure) |

```mermaid
flowchart TB
    WG["WAVE GLIDER"]
    AF["ARGO FLOAT"]
    SAT["SATELLITE<br/>(polar-capable)"]
    SH["SHORE / NCPOR<br/>dashboard"]

    AF <-->|"short-range / near-field<br/>ONLY at rendezvous<br/>high-volume offload · charge control"| WG
    WG <-->|"primary link — anytime at surface<br/>science & health down · commands up"| SAT
    AF -.->|"backup burst — brief windows<br/>essential profile only"| SAT
    SAT <-->|"terrestrial · continuous<br/>dashboard · API · alerts"| SH
```

> 🛡️ **Design resilience.** Every critical transfer has a fallback. If a rendezvous fails, the
> float still bursts its essential data to satellite and simply starts the next cycle; the
> glider's store-and-forward buffer holds data through satellite outages; and the vehicles
> continue safe, pre-programmed behaviour even if shore is silent.

<p align="center">
  <img src="assets/figures/fig_23_comms.png" alt="Communications matrix diagram" width="80%"/>
</p>

*Figure 23 — the communications matrix at a glance: who can talk to whom, when, and how much.*

### 4.4 Resilience and failover

The architecture is deliberately **store-and-forward everywhere**:

| Failure | Layer that absorbs it | Behaviour |
|---|---|---|
| Satellite outage (hours–days) | Glider buffer + float storage | Data accumulate on board; transmissions resume automatically when the link returns |
| Missed rendezvous | Float direct burst + planner | Essential profile goes by satellite; full dataset waits for next cycle |
| Shore server down | Vehicles + satellite | Vehicles continue safe pre-programmed behaviour; data queue in the gateway |
| Command link broken | Vehicle autonomy | Each vehicle runs its last validated plan; safe-hold on anomalies |
| Dock hardware fault | Separation logic | Vehicles separate, retry once, fall back to satellite transfer, fault logged |

### 4.5 Security architecture

| Concern | Measure |
|---|---|
| **Command authenticity** | Authenticated, encrypted command channel; signed messages per vehicle identity |
| **Mission-plan integrity** | Plans are versioned, checksummed and validated before execution |
| **Human approval** | Major actions (mission reconfiguration, abandon station) require operator approval |
| **Auditability** | Every command, override and autonomous decision is logged with its author (human or system) |
| **Data privacy** | Science data encrypted in transit; access controlled by role (see §11.3) |
| **Key management** | Per-vehicle keys provisioned at the pre-deployment debugger gate |

### 4.6 System interfaces

| Interface | Direction | Contract (summary) |
|---|---|---|
| Float → Glider (dock link) | During rendezvous | Profile packets + float health + charge-control messages |
| Glider → Satellite | Anytime | Compressed telemetry frames + science data + acknowledgements |
| Satellite → Shore gateway | Continuous | Frame relay to the mission server message bus |
| Shore → Satellite → Glider | On operator action | Checkpoint lists, mission config, overrides, time sync |
| Dashboard ↔ Mission server | Continuous | REST + WebSocket; RBAC-filtered views and commands |
| Env. feeds → Planning engine | Scheduled pull | Current/wind/wave forecasts, ice charts, iceberg analyses |
| ML models ↔ Planning engine | Each planning cycle | Prediction artefacts (trajectory, ellipse, confidence) |

### 4.7 Message and data contracts

Every byte that crosses a link is defined here. The full machine-readable schemas live in
`data/schemas/` and are versioned; this table is the human-readable catalogue.

| # | Message | Direction | Cadence | Typical size | Priority | Fallback |
|---|---|---|---|---|---|---|
| M01 | `profile_packet` | Float → Glider (dock link) | Once per rendezvous | 10–100 KB (full profile) | Science (highest) | Float direct burst (essentials only) |
| M02 | `profile_burst` | Float → Satellite | Only if rendezvous missed | 1–5 KB (decimated) | Science | Retry next cycle |
| M03 | `float_health` | Float → Glider / Satellite | Every surface window | ~200 B | Health | Buffered on float |
| M04 | `glider_met_sample` | Glider (log) → shore via relay | 1–60 min (configurable) | ~150 B/sample | Science | Store-and-forward on glider |
| M05 | `glider_telemetry` | Glider → Satellite | Every pass (scheduled) | 1–3 KB | Health + ops | Buffered on glider |
| M06 | `relay_frame` | Glider → Satellite | After rendezvous + per pass | Up to ~100 KB | Science | Retransmit until ACK |
| M07 | `checkpoint_list` | Shore → Glider (uplink) | On planner update (event-driven) | ~1 KB | Command | Last valid plan retained |
| M08 | `mission_config` | Shore → Glider/Float | Rare (mission changes) | 2–10 KB | Command (versioned) | Rejected if checksum invalid |
| M09 | `override_cmd` | Shore → Glider | Operator action | ~200 B | Command (highest) | Requires operator signature + ACK |
| M10 | `time_sync` | Shore → vehicles | Per pass | ~50 B | Ops | On-board clock drift budget holds |
| M11 | `ack_nack` | Vehicles → Shore | Per command | ~100 B | Ops | Command retry logic |
| M12 | `alert_event` | Vehicles → Shore | Event-driven | ~300 B | Alert | Re-sent until acknowledged |
| M13 | `tracker_report` | Iceberg tracker → Satellite | Scheduled (e.g. 6-hourly) | ~200 B | Ops (optional ext.) | Store-and-forward on tracker |
| M14 | `hazard_update` | Shore → Glider | On chart/tracker update | ~1 KB | Command | — |

**Rules**

1. Science messages are never overwritten until delivery is acknowledged (or, for the float,
   until the profile has survived one complete cycle on board plus one delivery attempt).
2. Command messages are signed, versioned and checksummed; invalid commands are rejected and
   logged (§4.5).
3. All messages carry `vehicle_id`, `msg_id` (monotonic), `timestamp_utc` and `schema_version`.
4. Priority order under congestion: `override_cmd` > `checkpoint_list` > `alert_event` >
   science > telemetry > logs.

### 4.8 Telemetry frame format (example)

```json
{
  "header": {
    "vehicle_id": "NCPOR-WG-001",
    "msg_id": 182347,
    "timestamp_utc": "2026-11-12T03:58:12Z",
    "schema_version": "1.3"
  },
  "position": { "lat": -62.418, "lon": 34.207, "sog_kt": 0.8, "cog_deg": 131 },
  "battery": { "soc_pct": 82, "charge_current_a": 1.1, "cell_temp_c": 4.2 },
  "links": { "satellite_rssi": -98, "satellite_pending_bytes": 41200, "short_range": "idle" },
  "mission_state": "loiter_in_zone",
  "checkpoint_index": 6,
  "next_rendezvous_eta_h": 12.5,
  "flags": { "ice_alert": false, "load_shedding": false, "watchdog_resets_24h": 0 },
  "payloads": [
    { "type": "met_sample", "count": 24, "interval_min": 30 },
    { "type": "health_log", "count": 3 }
  ]
}
```

> The shore server validates every frame against the schema **before** decoding payloads —
> malformed frames are quarantined, never silently dropped.

### 4.9 Telemetry and spectrum budget

The satellite link is the mission's most expensive and most constrained resource. Its use is
budgeted like energy.

| Budget item | Daily allowance (illustrative) | Rationale |
|---|---|---|
| Downlink volume (science) | ≤ 60 KB/day average | Profiles are bursty (rendezvous days) — smoothed over the cycle |
| Downlink volume (telemetry/logs) | ≤ 5 KB/day | Health packets + log deltas |
| Uplink volume | ≤ 2 KB/day | Checkpoints, configs, time sync, ACKs |
| Passes (summer) | 3/day | §13.7 plan A/B/C |
| Passes (winter) | 1/day | Load-shedding §8.4 |
| Peak pass duration | ≤ 15 min | Constellation geometry + power |
| Rendezvous relay | +1 dedicated pass | Highest-priority science |

**Queue discipline (§4.7 rule 4).** When the queue exceeds the budget, the comms manager sheds
in priority order: logs first, then telemetry, then science — commands and alerts always flow.
Every shed item stays on board (store-and-forward) — shedding is delay, never deletion.

### 4.10 Interference and electromagnetic compatibility

| Concern | Mitigation |
|---|---|
| On-deck interference (glider antennas vs float burst) | Antenna separation rules; rendezvous coordination: float burst suppressed while docked (dock link takes priority) |
| Satellite pass collision between vehicles | Pass scheduling offset by the shore contact plan (§13.7) |
| HF/VHF noise from vehicle electronics | EMC design reviews; ferrites/filtering; pre-deployment spectrum sniff on the debugger |
| Polar ionospheric effects on GNSS/satcom | Constellation choice; degraded-mode tolerances; no reliance on precision timing from space during outages |
| Charging noise during dock | Charge path designed to EMC limits; sensor sampling windows protected during charging |

> EMC is tested on the bench at P1 and on deck at P2 — a vehicle that jams its own satellite
> pass is discovered before it ever matters (§9.4 debugger comms check).

### 4.11 Shore-side system states

The mission server and dashboard are stateful systems with their own defined states:

| State | Meaning | Entry/exit |
|---|---|---|
| `NO_MISSION` | No active mission configured | Pre-season; configuration only |
| `COMMISSIONING` | Vehicles deployed; first cycles under close watch | From deployment until N cycles validated |
| `OPERATIONAL` | Routine autonomy with supervision | Automatic after commissioning criteria |
| `DEGRADED_SHORE` | Reduced capability (feed outage, partial server) | On detected failures; recoverable |
| `INCIDENT` | An active L3/L4 alert being worked (§15.3) | Manual entry by operators |
| `CLOSEOUT` | Season end procedures (§13.8) | Manual entry at season end |

Shore-state transitions are logged like vehicle events (§9.5) and appear in the audit trail —
the state of the people's tools matters as much as the state of the vehicles.

---

## 5. The Two Vehicles

### 5.1 The Wave Glider — the persistent surface sentinel

The Wave Glider is the mission's surface platform. It consists of a surface float roughly the
size of a surfboard, connected by a flexible tether (typically several metres long) to a
**submerged fin-rack**. The surface float carries the solar panels, batteries, electronics,
antennas and atmospheric sensors; the submerged part provides propulsion.

📷 **Plate 5.1 — A Wave Glider-style surface vehicle**: a low, surfboard-shaped hull covered in
solar panels, with GPS and satellite-communication masts. It operates at the surface for months
at a time without fuel. *(Representative photograph, credited in §20.5.)*

<p align="center">
  <img src="assets/images/wave_glider_at_sea.jpg" alt="Wave Glider surface vehicle at sea" width="80%"/>
</p>

🖼️ **Figure 4 — How the Wave Glider moves, and why it needs no fuel:**

<p align="center">
  <img src="assets/figures/fig_04_waveglider_propulsion.png" alt="Wave Glider propulsion mechanics" width="80%"/>
</p>

#### What it carries and does

| Subsystem | Description |
|---|---|
| **Wave-propulsion system** | Submerged body with rows of hinged fins plus a rudder; no engine and no fuel. Available steering/control mechanisms (and, where fitted, small auxiliary thrusters) are used within their strict energy and sea-state limits. |
| **Solar power system** | Deck-mounted solar panels and rechargeable batteries keep sensors, computers and satellite links running through the long polar summer daylight. |
| **Atmospheric & sea-surface sensors** | Wind speed and direction, air temperature, barometric pressure, humidity, incoming solar radiation, wave height/period/direction and sea-surface temperature. This continuous weather record is one of the glider's primary scientific products. |
| **Navigation & communications** | GPS receiver, a satellite modem for data and commands, and a short-range link to talk to the Argo Float when it surfaces. |
| **Docking / charging hardware** | A mechanical capture-and-lock feature and charge connector that physically secure the float and pass power and data during a rendezvous. |

#### Reference performance envelope (illustrative)

| Parameter | Typical value (illustrative) | Note |
|---|---|---|
| Speed | 0.5–1.5 kt depending on sea state | Wave-driven; calm = slow |
| Endurance | Months, effectively indefinite | Wave + solar |
| Payload power budget | TBD at design review | Sized with winter margin |
| Steering | Rudder, limited authority | Plus optional auxiliary thruster |
| Operating temperature | Polar-rated | Cold-tolerant electronics & battery |

#### Atmospheric & sea-surface sensor suite (reference specification)

| Sensor | Variable | Unit | Typical polar range | Target accuracy (illustrative) | Sample interval |
|---|---|---|---|---|---|
| Anemometer | Wind speed | m/s | 0–40 | ±0.3 m/s or ±3 % | 1 min |
| Wind vane | Wind direction | deg | 0–360 | ±5° | 1 min |
| Air temperature | Air temp | °C | −40…+20 | ±0.2 °C | 1 min |
| Barometer | Pressure | hPa | 950–1050 | ±0.3 hPa | 1 min |
| Hygrometer | Relative humidity | % | 0–100 | ±3 % | 1 min |
| Pyranometer | Incoming solar radiation | W/m² | 0–1400 | ±5 % | 1 min |
| Wave sensor (accelerometer/IMU) | Wave height / period / direction | m · s · deg | 0–15 m · 3–25 s | ±5 % Hₛ | continuous → 30-min spectra |
| SST probe | Sea-surface temperature | °C | −2…+10 | ±0.05 °C | 1 min |
| GPS | Position / time | deg / UTC | — | < 5 m | 1–10 min |
| Compass/IMU | Heading, attitude | deg | — | ±2° | 1 s (nav loop) |

#### Wave Glider power budget (illustrative — to be replaced by as-built measurements)

| Consumer | Duty cycle | Average draw (mW) | Note |
|---|---|---|---|
| Navigation & computing | Continuous (low-power mode between cycles) | 250 | Sleep/wake managed by OS |
| Meteorological sensors | 1 sample/min each | 180 | Warm-up transients accounted |
| GPS | 1 fix/10 min (more during approach) | 60 | Fix schedule adaptive |
| Satellite modem | Per pass (2–4 passes/day) | 400 (during pass) | Dominant comm cost |
| Steering (rudder) | During active transit only | 150 | Wave propulsion itself is free |
| Docking/charging hardware | Rendezvous only | — | Fed from battery during charge |
| **Total electrical load (nominal)** | | **≈ 1.1 W average** | Solar array sized ≥ 3× this, polar winter margin |
| **Per-cycle float top-up reserve** | At each rendezvous | +4–8 Wh delivered | Sized per §8.4 |

#### Wave Glider subsystem interfaces

| Subsystem | Talks to | Interface | Notes |
|---|---|---|---|
| Propulsion (fin rack + rudder) | Nav controller | CAN / serial, actuator commands | No electrical draw for thrust; rudder only |
| Power (MPPT + battery) | OS health monitor | I²C/PMBus, SOC & temps | MPPT logged for solar-yield science |
| Satcom modem | Comms manager | Serial (AT/Iridium-style) + antenna | Store-and-forward queue in flash |
| Short-range link | Docking controller | RF module, rendezvous-only wake | Wakes on proximity beacon |
| Dock/lock mechanism | Docking controller | GPIO + current sense, limit switches | Lock-confirm sensor per §6.3 |
| Charge connector | Docking controller | Wet-rated connector / inductive link | Handshake + charge curve control |

> ⚠️ **Engineering honesty — the glider is NOT a motorboat.** Wave propulsion works best with
> some sea state; in perfectly flat calm water, forward progress slows, and strong currents or
> headwinds can push the vehicle off course. Steering authority is limited. The whole planning
> approach (Sections 7–8) is built around this reality: we predict the region the glider can
> realistically reach and plan routes that go mostly **with** the waves and currents, rather than
> assuming free movement in any direction.

### 5.2 The Argo Float — the deep-ocean profiler

The Argo Float is a small, autonomous, vertically profiling robot, typically a cylindrical
pressure hull one to two metres tall carrying sensors at the top and a **buoyancy engine**. It
cannot propel itself horizontally; instead it changes its buoyancy to rise or sink, and the
ocean's currents carry it horizontally between profiles.

📷 **Plate 5.2 — A profiling float being deployed from a research vessel.** Floats are compact,
cylindrical and designed to be launched by hand or crane. *(Representative photograph, credited
in §20.5.)*

<p align="center">
  <img src="assets/images/argo_float_deployment.jpg" alt="Argo float deployment" width="80%"/>
</p>

#### How it dives and rises

A float has no propeller. A small hydraulic pump moves mineral oil between an internal reservoir
and an external flexible bladder. Pumping oil **out** expands the bladder, increasing volume and
making the float positively buoyant so it rises; drawing oil **in** shrinks the bladder and it
sinks. By matching the density of the surrounding water it can hover at a "parking" depth and
drift almost for free.

🖼️ **Figure 6 — The buoyancy engine:**

<p align="center">
  <img src="assets/figures/fig_06_buoyancy_engine.png" alt="Buoyancy engine mechanics" width="80%"/>
</p>

🖼️ **Figure 5 — The Argo Float's repeating ~10-day mission cycle:**

<p align="center">
  <img src="assets/figures/fig_05_argo_cycle.png" alt="Argo float 10-day cycle depth profile" width="72%"/>
</p>

#### What it measures

| Class | Variables | Notes |
|---|---|---|
| **Core (every profile)** | Temperature, salinity (from measured conductivity), pressure (gives depth) | The classic CTD vertical profile |
| **Optional biogeochemical** | Dissolved oxygen, chlorophyll-a fluorescence (phytoplankton proxy), optical backscatter/turbidity, nitrate, pH | Where fitted |
| **Sampling resolution** | Fine vertical intervals — order of every couple of metres | Recorded during the ~2,000 m ascent |
| **Storage** | All data stored on board until the surface window | Transferred at rendezvous or via burst |

#### Argo Float engineering specification (reference)

| Parameter | Value (illustrative) | Notes |
|---|---|---|
| Hull | Cylindrical pressure vessel, 1–2 m | Proven deep-rating (≥ 2,000 m + margin) |
| Buoyancy engine | Hydraulic pump + external bladder | Oil volume change = dive/rise control |
| Depth rating | ≥ 2,000 m | With safety margin per manufacturer |
| Cycle | ~10 days (configurable 5–15) | Park depth, profile depth, window all configurable |
| Core sensors | CTD (conductivity, temperature, pressure) | Argo-standard accuracy classes |
| Optional sensors | O₂, chlorophyll-a fluorescence, backscatter, nitrate, pH | Per mission configuration |
| Storage | ≥ 2 full cycles of profiles | Never overwritten before delivery |
| Surface comms | Short-range link + satellite burst modem | Burst only used as fallback |
| Energy | Primary battery + recharge input from dock | Recharge via glider per §6.3 |
| Ice handling | Ice-aware surfacing logic | Waits subsurface or shortens window near ice |

#### Float cycle parameters (configurable, illustrative defaults)

| Parameter | Default | Range | Constraint |
|---|---|---|---|
| `park_depth_m` | 1000 | 800–1500 | Below mixed layer, above profile depth |
| `profile_depth_m` | 2000 | 1500–2000 | Hull rating limit |
| `cycle_days` | 10 | 5–15 | Set with energy budget |
| `surface_window_min` | 60 | 15–240 | Extended on demand at rendezvous |
| `ascent_sampling_interval_m` | 2 | 1–10 | Sensor power vs resolution trade |
| `burst_if_missed` | true | — | Essentials always delivered |
| `ice_surface_rule` | wait-or-shorten | — | Per §8.3 fail-safe |

📷 **Plate 5.3 — A compact CTD sensor**, the core instrument on a profiling float. Conductivity
is converted into salinity, and pressure into depth, producing the classic vertical ocean
profile. *(Representative product photograph, credited in §20.5.)*

<p align="center">
  <img src="assets/images/ctd_sensor_2.jpg" alt="CTD sensor" width="55%"/>
</p>

### 5.3 Side-by-side comparison

| Attribute | Wave Glider | Argo Float |
|---|---|---|
| **Where it operates** | At the surface, continuously | Mostly underwater; brief surface windows every ~10 days |
| **How it moves** | Wave energy → fins → thrust; limited rudder steering | Changes buoyancy; horizontal motion is purely with currents |
| **Energy** | Solar panels + battery; propulsion costs no fuel | Battery, topped up by the glider at each rendezvous |
| **Primary measurements** | Atmosphere, waves, sea surface; gateway & charging | Vertical ocean profiles: T, S, P (+ O₂, chlorophyll) |
| **Navigation** | Continuous GPS; follows checkpoints | GPS fix only at the surface; dead-reckoned drift underwater |
| **Communication** | Satellite anytime; short-range link to float | Short-range to glider; satellite burst as backup |
| **Greatest strength** | Persistence, power, two-way connectivity | Access to the full upper 2,000 m of the ocean |
| **Greatest constraint** | Limited control; needs sea state to move | Cannot steer; blind & silent while submerged |

### 5.4 Why the pairing works

```mermaid
quadrantChart
    title Why the pairing works: each platform's weakness is the other's strength
    x-axis Low Energy Budget --> High Energy Budget
    y-axis Weak Connectivity --> Strong Connectivity
    quadrant-1 "Complementary strength"
    quadrant-2 "Glider's world"
    quadrant-3 "Float's world"
    quadrant-4 "Redundant"
    "Argo Float": [0.22, 0.2]
    "Wave Glider": [0.8, 0.85]
    "Combined pair": [0.85, 0.8]
```

> 💡 **Why the pairing works.** The float can gather the deep data but has no power budget or
> surface time to spare; the glider has continuous power and connectivity but cannot dive. By
> meeting every ten days, **the glider gives the float energy and a voice, and the float gives
> the glider the water column**.

### 5.5 Vehicle operating modes

Each vehicle implements a small set of named operating modes; every mode has defined power,
comms and safety behaviour. The mission state machine (§6.4) selects between them.

**Wave Glider modes**

| Mode | Behaviour | Power profile | When |
|---|---|---|---|
| `PATROL` | Station-keeping in the operating box; full met sampling | Nominal | Between rendezvous |
| `TRANSIT` | Following checkpoints toward the surfacing zone | Elevated (steering active) | Approach phase |
| `LOITER` | Hold inside the zone, minimal motion | Reduced | D−1 day to surfacing |
| `RENDEZVOUS` | Dock, offload, charge | Peak (charge path active) | Surfacing window |
| `ICE_AVOID` | Divert around hazard or hold clear | Variable | Hazard on route |
| `SAFE_HOLD` | Minimal motion, data protected, awaits resolution | Minimal | Anomaly |
| `DEGRADED` | Load-shedded operation: reduced sampling/tx | Minimal (managed) | Low solar / faults |
| `SURVIVAL` | Bare electronics alive, store data, occasional tx | Absolute minimum | Critical energy state |

**Argo Float modes**

| Mode | Behaviour | Energy use | When |
|---|---|---|---|
| `DESCEND` | Pump-in to park depth | Pump stroke | Cycle start |
| `PARK_DRIFT` | Neutral buoyancy, drift & log | Sensors only | ~9 days |
| `DEEP_PROFILE` | Dive to ~2,000 m, then ascent with CTD | Pump + sensors | Cycle end |
| `SURFACED` | GPS fix, announce, await dock | Peak (radio) | Surface window |
| `DOCKED` | Offload + charging via glider | Charge input | Rendezvous |
| `BURST_FALLBACK` | Direct satellite essentials, then dive | Burst cost | Missed rendezvous |
| `ICE_WAIT` | Hold subsurface short of ice, retry window | Minimal | Ice overhead |
| `SAFE_HOLD` | Surface (ice-safe spot) and beacon | Minimal | Anomaly |

> 📝 **Mode transitions are data-driven, not timer-driven.** A mode change requires either a
> sensor-verified condition, a validated command, or a watchdog expiry — never a bare timeout
> guess. This is what makes the hand-held debugger's actuator checks (§9.4) meaningful: every
> mode's exit depends on actuators that were verified before launch.

### 5.6 Vehicle identification and naming

| Field | Example | Rule |
|---|---|---|
| Platform ID (WMO-style) | `NCPOR-WG-001`, `NCPOR-FLT-001` | Registered in the shore registry before deployment |
| Mission ID | `CPO-2026-S1` | Cooperative Polar-Ocean, year, season |
| Callsign (satcom) | Per modem IMEI/ID | Bound to the vehicle's command keys (§4.5) |
| Cycle numbering | 1..36 per float | Incremented at each surfacing, logged in every profile |

### 5.7 Reliability, availability and maintainability (RAM) targets

| Measure | Target (illustrative) | How it is engineered |
|---|---|---|
| Mission availability (both vehicles operational) | ≥ 90 % of days over a season | Redundant data paths; safe-hold recovery; seasonal servicing |
| Glider survival probability (1 season) | ≥ 95 % | Ice avoidance by construction; energy margin; watchdogs |
| Float survival probability (1 season) | ≥ 90 % | Ice-aware surfacing; recharge; conservative cycle limits |
| Mean time to diagnose (fault → layer identified) | < 4 h | Layer-coded event logs (§9.2), dashboard fault view |
| Mean time to recover (software anomaly) | < 24 h | Watchdog resets, safe-hold, validated reconfiguration |
| Profile loss rate (any cause) | 0 % (essentials always delivered) | Dual delivery paths; data-first docking (§6.3) |
| Single points of failure | Catalogue maintained and shrinking | §15.4 failure catalog review per phase |

**Maintainability rules**

1. Anything replaceable on deck is replaceable in the field plan (spares manifest).
2. Firmware and configs are updatable over the air with rollback (§17.10).
3. Every fault carries enough logged context to reproduce it in the simulator on shore
   (§17.8 chaos drills) — "fix on the digital twin before touching the vehicle".

### 5.8 Calibration and metrology plan

Every sensor that produces science must be traceable, before and after the mission.

| Instrument | Pre-deployment | In-mission | Post-recovery |
|---|---|---|---|
| CTD (float) | Factory calibration + lab intercomparison | QC rules QC-07/QC-10 (§10.7); co-located glider SST reference | Post-mission drift analysis vs ship CTD |
| Anemometer/vane (glider) | Bench wind-tunnel or field intercomparison | Cross-check vs reanalysis winds | Drift report |
| Barometer | Lab pressure standard | QC range checks | Re-verification |
| Pyranometer | Radiometer standard | Solar-yield model comparison (Fig. 18) | Re-verification |
| SST probe | Ice-bath + standard | Daily self-check vs in-situ range | Lab check |
| Wave sensor | Bench motion calibration | Energy-spectrum sanity vs wind | Re-verification |

**Metrology rules**

1. Calibration certificates are archived with the data (§10.11 provenance).
2. Any sensor failing a pre-deployment check blocks the debugger gate (§9.4).
3. Delayed-mode QC (§10.7) uses the post-recovery calibrations to refine the final archive.

---

### 5.9 Sensor payload at a glance

Everything this mission publishes starts as a physical measurement on one of two platforms. This
section pins down exactly what is measured, how often, how accurately, and what it costs in
power — the contract every downstream section (ML inputs in §7, data products in §10) builds on.

<p align="center">
  <img src="assets/figures/fig_25_sensor_payload.png" alt="Sensor payload at a glance" width="95%"/>
</p>

*Figure 25 — the payload map: every sensor on both vehicles and where its data ends up. The two
platforms deliberately overlap in only one measurement (sea-surface temperature) so that one
calibrates the other (§5.8).*

| Platform | Sensor | Quantity measured | Nominal accuracy / notes |
|---|---|---|---|
| Glider | Air temperature / humidity | Near-surface meteorology | ±0.2 °C / ±2 % RH (class); aspirated shield |
| Glider | Barometer | Surface pressure | ±0.5 hPa — storm detection (Q2, §2.5) |
| Glider | Wind sensor (anemometer + vane) | Wind speed & direction at ~0.5 m | Corrected to 10 m using log-profile |
| Glider | Pyranometer | Downwelling shortwave radiation | ±5 % — drives the solar-energy model (Fig. 18) |
| Glider | IMU / wave sensing | Wave spectra, significant wave height | From platform motion; validated in P2 |
| Glider | Sea-surface temperature probe | SST at 0.1 m | ±0.05 °C — the shared calibration anchor |
| Glider | GPS/GNSS | Position, time | Standard positioning; ionospheric-tolerant receiver |
| Float | CTD | Conductivity, temperature, pressure | Argo target: ±0.002 °C, ±0.01 PSU, ±2.4 dbar |
| Float | Dissolved-oxygen optode | O₂ concentration | Argo BGC standard; in-situ drift corrected (§10.7 QC-10) |
| Float | Chlorophyll fluorometer | Chl-a proxy | Qualitative-to-semi-quantitative; used for bloom timing |
| Float | GPS (surfacing only) | Position fix | Label for the Lagrangian drift experiment (§2.5 Q3) |

**Sampling cadence and power cost** (the reason cadence exists as a planner variable, §8.4):

| Measurement | Cadence (summer) | Cadence (winter) | Power per sample (illustrative) |
|---|---|---|---|
| Met suite (glider) | 1 min | 10 min (§13.10) | ~0.5 Wh/day at 1-min cadence |
| Wave spectra | 20 min bursts | 2 h bursts | ~0.3 Wh/day |
| SST | 1 min | 1 min (cheap, kept full-rate) | negligible |
| CTD profile (float) | 1 per cycle | 1 per cycle | ~1.5 Wh/profile (pump + sensors) |
| O₂ / Chl | with profile | with profile | ~0.4 Wh/profile |

```mermaid
flowchart TD
    S1["🌡️ SENSOR SUITE<br/>met · SST · waves (glider)<br/>CTD · O₂ · Chl (float)"] --> P1["⏱️ SAMPLING SCHEDULER<br/>cadence per §13.3 season logic"]
    P1 --> P2["📦 PACKAGING<br/>units, timestamps, sensor flags"]
    P2 --> P3{"platform healthy?"}
    P3 -- "yes" --> P4["💾 STORE<br/>glider store-and-forward<br/>float profile memory"]
    P3 -- "no" --> P5["🚩 FLAG<br/>health event §9.2, keep raw anyway"]
    P5 --> P4
    P4 --> P6["🚚 DELIVERY<br/>rendezvous offload / relay / burst (§10.11)"]
    P6 --> P7["✅ QC<br/>real-time flags → delayed-mode (§10.7)"]
    P7 --> P8["🗄️ ARCHIVE<br/>netCDF/CF with provenance (§10.6, §10.9)"]
```

<p align="center">
  <img src="assets/images/ocean_glider_launch.jpg" alt="Researchers prepare to launch an ocean glider" width="60%"/>
</p>

*Preparing a profiling ocean glider for launch (NOAA AOML photo). The glider family — Slocum,
Seaglider, Spray — is the mature, energy-frugal technology lineage the paired mission builds on;
the Argo float brings the same philosophy to the deep water column (§2.3).*

> 📐 **Design rule.** A sensor earns its place on this payload only if it (a) serves a §2.5
> question, (b) survives the winter power budget (§8.4), and (c) has a calibration story (§5.8).
> Everything else is left ashore — mass and watts are the mission's scarcest resources.

## 6. One Mission Cycle: Dive, Predict, Meet, Recharge

The mission is easiest to understand by following a single ~10-day event. While the float is
invisible underwater, the glider and the shore-side intelligence are already working out where
it will reappear and how to get there.

### 6.1 The sequence of one event

🖼️ **Figure 3 — One surfacing event shown as a four-lane timeline** (timing stretched for
clarity):

<p align="center">
  <img src="assets/figures/fig_03_mission_timeline.png" alt="Mission cycle timeline" width="95%"/>
</p>

| # | Phase | What happens | Who is busy |
|---|---|---|---|
| 1 | **Days 0–9 — the float is away** | The float descends to ~1,000 m, drifts with deep currents while logging, then sinks toward ~2,000 m before beginning its measured ascent profile. Meanwhile the glider patrols the surface, recording the atmosphere, and the two ML models continuously update their forecasts. | Float: diving/logging. Glider: patrolling. Shore: forecasting. |
| 2 | **Approach** | As the predicted surfacing day nears, the planning engine lays checkpoints through the currents (and around any ice) so the glider arrives in the high-probability zone **before** the float surfaces, then loiters there. | Planner + glider. |
| 3 | **Surfacing** | The float breaks the surface, takes a GPS fix and announces itself on the short-range link. | Float. |
| 4 | **Dock & lock** | The glider manoeuvres to the float and a capture/lock feature physically joins the two so they move together in the swell. | Both. |
| 5 | **Data + energy, together** | Stored scientific data transfers to the glider while the float's battery is recharged from the glider's solar-charged store. The glider forwards the dataset up to satellite; the float may also transmit directly in parallel. | Both + satellite. |
| 6 | **Release** | On confirmation that data are delivered and the battery target is reached, the vehicles unlock. The float starts its next dive; the glider resumes its surface patrol. | Both. |

### 6.2 The rendezvous sequence diagram

```mermaid
sequenceDiagram
    autonumber
    participant F as Argo Float
    participant G as Wave Glider
    participant S as Satellite
    participant M as Mission Server
    participant D as Dashboard / Ops

    Note over F: Days 0–9: park ~1,000 m,<br/>drift, log, dive ~2,000 m
    loop every 6 h while submerged
        M->>M: ML update: surfacing zone forecast
    end
    M->>G: uplink revised checkpoints
    Note over G: approach + loiter inside<br/>predicted surfacing zone
    F->>F: begin ascent, continuous CTD profile
    F->>G: surface: announce (short-range link)
    F->>S: GPS fix (position corrects prediction)
    G->>F: dock & lock
    Note over F,G: mechanical capture, lock confirmed
    F->>G: offload stored profiles
    G->>F: recharge to target SOC
    G->>S: forward combined dataset + health
    S->>M: relay frames
    M->>M: decode · QC · archive
    M->>D: publish products + update state
    F->>S: (optional parallel burst — backup)
    G->>F: release
    F->>F: start next dive cycle
    M->>M: retrain labels: surfacing outcome
```

### 6.3 How docking, data and charging work

The exact mechanism is an indigenous engineering design task, but the functional elements are
well understood and will include:

| Function | Options under study | Requirements |
|---|---|---|
| **Capture and lock** | Guided capture feature (funnel/cone and compliant arm), magnetic capture, or a grapple | Tolerates wave motion; sensors confirm a secure lock; release on command or automatically on fault |
| **Data transfer** | Short-range, high-reliability radio or wired link across the docked interface | Far faster and lower-power than sending the whole profile over satellite; CRC/retransmit; resumable |
| **Recharging** | Wet-rated connector or inductive (contactless) charging link fed by the glider's battery | Charge control and temperature monitoring managed by both vehicles' embedded software |

> 📝 **Design note — managing the surface window.** A conventional Argo float spends only a short
> time at the surface transmitting. A docked recharge needs a longer, planned surface interval.
> The mission design accounts for this: **profiles are transferred in the first minutes**,
> charging proceeds to a **safe target charge (not necessarily 100 %)** over the following
> window, and the interval is scheduled around daylight, weather and sea state. If conditions
> deteriorate, the pair can separate and the float can complete on satellite power — **science
> data are never held hostage to the recharge**.

### 6.4 Mission states and transitions

```mermaid
stateDiagram-v2
    [*] --> PreDeployment: engineer on deck
    state PreDeployment {
        [*] --> DebuggerCheck
        DebuggerCheck --> Ready: all subsystems pass
        DebuggerCheck --> DebuggerCheck: fix on deck, re-test
    }
    PreDeployment --> Deployed: crane launch
    Deployed --> GliderPatrol: glider on station
    state GliderPatrol {
        [*] --> Measuring
        Measuring --> IceAvoid: hazard on route
        IceAvoid --> Measuring: hazard cleared
    }
    GliderPatrol --> Approach: surfacing day −1.5 d
    Approach --> Loiter: inside surfacing zone
    Loiter --> DockLock: float announced
    DockLock --> Transfer: lock confirmed
    Transfer --> Release: data done + SOC target met
    Release --> GliderPatrol: next cycle begins
    DockLock --> RetryOnce: lock fault
    RetryOnce --> DockLock: retry
    RetryOnce --> SatFallback: still failing
    Loiter --> SatFallback: float never announced
    SatFallback --> GliderPatrol: essential data sent by float burst
    GliderPatrol --> SafeHold: anomaly (low battery / ice / fault)
    SafeHold --> GliderPatrol: resolved by planner or operator
    SafeHold --> [*]: mission end (recovery / abandonment)
```

### 6.5 What happens if a rendezvous fails

| Situation | Automatic response | Data outcome |
|---|---|---|
| **Glider will be late** (waves/currents worse than forecast) | Planner re-optimises; float is commanded to extend its surface window within battery limits, or waits one cycle at a pre-agreed loiter pattern. | Delayed, not lost |
| **Float surfaces outside the predicted zone** | GPS fix from its direct satellite burst reveals its true position; glider reroutes to the new location while battery permits. | Usually recovered |
| **Vehicles cannot meet at all this cycle** (ice, storm, fault) | Float transmits the essential profile directly to satellite (backup path), starts its next dive; glider attempts recharge next cycle. | **No data are lost** — essentials delivered, full dataset next cycle |
| **Lock or charging fails on contact** | Vehicles separate, retry once, then fall back to satellite transfer; the fault is logged and alerted for diagnosis. | Satellite fallback |
| **Glider battery too low to safely charge the float** | Data offload and glider self-preservation take priority; float uses its reserve and satellite burst; planner reduces glider energy use until solar recovery. | Data offloaded; charge deferred |
| **Float surfaces under/near ice** | Ice-aware surfacing logic: float waits subsurface or shortens the window; glider holds clear; alert raised to operators. | Safe, next attempt |

> 💬 **In plain language.** The system is designed so that a failed meeting is an *inconvenience,
> not a loss*. Every fallback keeps the science flowing: the profile always has at least one
> route to shore.

### 6.6 Docking state machine (reference implementation sketch)

The docking controller runs on the glider with the float's surface logic as the counterpart.
This is the control-flow contract both firmware teams implement against:

```python
class DockingController:
    """Reference sketch — glider side. Contract for firmware teams."""

    def run(self, events):
        # States: SEARCH, APPROACH, CAPTURE, LOCKED, TRANSFERRING, CHARGING,
        #         RELEASING, ABORT
        match self.state:
            case "SEARCH":
                # float announced on short-range link + GPS fix shared
                if events.float_announced:
                    self.set_heading_towards(events.float_position)
                    self.state = "APPROACH"
            case "APPROACH":
                # steer within capture envelope; abort if sea state exceeds limits
                if self.within_capture_envelope():
                    self.deploy_capture_mechanism()
                    self.state = "CAPTURE"
                elif events.sea_state > SELL:   # sea-state envelope limit
                    self.abort("sea_state")
            case "CAPTURE":
                if events.lock_sensor_confirmed:
                    self.state = "LOCKED"
                    self.log("lock confirmed", both_vehicles=True)
                elif self.capture_attempts >= MAX_ATTEMPTS:
                    self.abort("lock_fault")
            case "LOCKED":
                # data first, then charge — science is never hostage to power
                self.start_transfer(priority="profile_data")
                self.state = "TRANSFERRING"
            case "TRANSFERRING":
                if events.transfer_complete:
                    self.log("offload done", bytes=events.transferred)
                    self.start_charging(target_soc=self.target_soc())
                    self.state = "CHARGING"
                elif events.transfer_stalled:
                    self.abort("link_fault")    # float falls back to burst
            case "CHARGING":
                if self.soc_reached(events.float_soc) or events.window_expired:
                    self.state = "RELEASING"
                if events.charge_overtemp or events.surge_detected:
                    self.abort("charge_fault")  # data already safe on glider
            case "RELEASING":
                self.release_lock()
                if events.release_confirmed:
                    self.log("release ok")
                    self.state = "SEARCH"       # next cycle begins
            case "ABORT":
                self.release_lock()             # never leave the pair coupled
                self.log("abort", reason=self.abort_reason)
                self.notify_shore(alert="rendezvous_fault", reason=self.abort_reason)
                self.state = "SEARCH"

    def target_soc(self):
        # safe target charge within the planned surface window — NOT necessarily 100 %
        return min(100.0, self.float_soc + self.charge_rate * self.window_remaining)
```

**Contract notes**

- The float side mirrors these states (`SURFACED`, `DOCKED`, `SENDING`, `CHARGING`,
  `RELEASED`, `FALLBACK`) and independently decides its own fallback — the two controllers
  never assume the other's state without sensor confirmation.
- Every transition is time-stamped in both vehicles' event logs (§9.2) and shipped in the next
  telemetry pass.
- `SELL` (sea-state envelope limit), `MAX_ATTEMPTS`, charge curves and target-SOC policy are
  configuration, validated in Phase 1 tank tests and Phase 2 coastal trials (§16.2).

### 6.7 Timing, synchronisation and deadlines

| Clock | Sync method | Drift budget |
|---|---|---|
| Glider RTC | GNSS time at every fix; NTP-style offset estimate | < 1 s/day without fix |
| Float RTC | GNSS time at surfacing; free-run underwater | < 5 s/cycle (corrected each surface) |
| Shore server | NTP/PTP | < 50 ms |

| Deadline (relative to surfacing) | Action | Owner |
|---|---|---|
| D − 2 days | Surfacing zone published; approach checkpoints uplinked | Planner |
| D − 1 day | Glider should enter zone; loiter pattern begins | Glider |
| D − 6 h | Final zone update (shrink); loiter position adjusted | Planner + Glider |
| T + 0 | Float surfaces; announces on short-range link | Float |
| T + 5 min | Lock completed or abort declared | Docking controller |
| T + 15 min | Profile transfer complete (data-first rule) | Both |
| T + 15 min → window end | Charging to target SOC | Glider |
| Window end | Release; float dives or falls back to burst | Both |

> 📝 **Why "data first"?** A profile is the whole point of the mission. The window may be cut
> short by weather at any moment, so the first minutes are reserved exclusively for offload.
> Charging is important; the data are irreplaceable.

### 6.8 Rendezvous performance metrics and tuning

After each rendezvous the system computes a standard scorecard; the planner team reviews the
trends at the weekly engineering sync.

| Metric | Definition | Healthy band (illustrative) |
|---|---|---|
| Arrival offset | Glider-on-station time minus float surfacing time | −36 h to −6 h (early) |
| Zone containment | Did the float surface inside the predicted zone? | Coverage ≈ confidence (§7.5) |
| Dock attempts | Attempts before lock confirmation | Median 1; ≤ 2 in 95 % of cases |
| Offload completeness | Bytes delivered vs bytes collected | 100 % at dock; ≥ essentials via burst |
| Charge delivered | Wh into float vs plan | 80–100 % of planned top-up |
| Window utilisation | Time from surfacing to release vs planned window | 40–90 % |
| Energy spent (glider) | Transit + loiter + dock energy vs budget | Within −10 %/+20 % of plan |

**Tuning loop.** Off-band metrics trigger planner-parameter reviews (§8.5 tuning table): a
systematically late glider suggests raising the arrival margin; repeated zone misses suggest
ensemble recalibration; dock attempts > 2 suggest reviewing the capture envelope or sea-state
gate (`SELL`, §6.6). Every change is trialled in the simulator against the last 10 real cycles
before uplink.

### 6.9 Rendezvous rehearsal plan

Before the first live rendezvous in each phase, the sequence is rehearsed in escalating
fidelity — the dock never meets the ocean before it has met the bench.

| Rehearsal | Where | What is proven |
|---|---|---|
| R-0 Desk | Simulator | Control-flow contract (§6.6) executes end-to-end with synthetic events |
| R-1 Bench | Lab rig | Dock mechanism capture/release; charge path; lock sensors |
| R-2 Tank | Tank/pool | Capture under motion; wet connector; offload through water |
| R-3 Coastal | Sheltered water | Full sequence with real vehicles, easy recovery, chase boat present |
| R-4 Operational | Open water | The real thing, ship support within range (P3) |

**Rehearsal gate rules**

- R-1 must pass 50/50 capture-release cycles without fault.
- R-2 adds motion: success at sea-state representative of `SELL` (§6.6).
- Every failure in any rehearsal is logged and either fixed or explicitly accepted as a known
  risk with a fallback (§15.1 R5).

### 6.10 Rendezvous quick-reference card

The one-page card kept in the ops room (and in the dashboard help panel):

**Before the window (D−2 to D−1)**

- [ ] Surfacing zone published by Model 2 — confidence ≥ 0.8, or degraded flag visible
- [ ] Checkpoints uplinked and acknowledged (M11 §4.7)
- [ ] Glider on track — arrival offset ≤ −12 h (target −24 h or earlier)
- [ ] Ice clear of the zone ± stand-off; no hazard updates pending

**At the window (T−6 h to surfacing)**

- [ ] Final zone update consumed; loiter position confirmed inside
- [ ] Battery: glider SOC ≥ floor + recharge budget for the planned top-up
- [ ] Sea state below `SELL` — otherwise plan the fallback, not the dock

**During the rendezvous**

- [ ] Float announced; lock confirmed (sensor-based, both logs)
- [ ] Data first: transfer complete before charging starts
- [ ] Charge to target SOC only; watch window remaining

**After release**

- [ ] Release confirmed; float dives on schedule
- [ ] Relay pass scheduled for the combined dataset
- [ ] Scorecard computed (§6.8) and posted to the engineering sync

**If anything fails:** the ladder in §6.5 decides — extend window, satellite fallback, or retry
next cycle. No data are lost; no vehicle is risked.

---

**Part I in one breath.** A hostile ocean justifies the mission (§2); two complementary, proven
vehicles form one cooperative system (§3 and §5); the architecture moves every profile from the
deep ocean to shore without a ship (§4); and every ten days the pair proves the whole concept —
dive, predict, meet, recharge (§6). Part II adds the intelligence that makes the meeting happen.

---

# PART II — THE INTELLIGENT LAYER

---

## 7. The ML Brain: Trajectory Prediction

The hardest part of the mission is making two vehicles that **cannot be freely steered** meet in
a remote ocean. The answer is prediction: two dedicated machine-learning models that estimate
where each vehicle will be, how confident to be, and what is reachable.

### 7.1 Why a single predicted point is the wrong answer

The ocean is a chaotic, turbulent fluid. Tiny differences in current speed or wind amplify over
ten days, so a forecast of the float's surfacing location made on day 0 is necessarily uncertain.
Predicting one exact latitude and longitude would be both wrong and dangerously overconfident.
Instead, **both models output probability distributions**:

| Model | Probabilistic output |
|---|---|
| **Model 1 — glider** | Most probable path with a widening **uncertainty envelope** and a **reachable region** |
| **Model 2 — float** | Most probable surfacing zone as an **ellipse** with an uncertainty radius in kilometres and a **confidence score** (e.g. 80 % probability of surfacing within the zone) |

The planner then routes the glider to maximise its chance of being **inside** that zone.

🖼️ **Figure 7 — The two prediction models** (blue = Model 1, amber = Model 2):

<p align="center">
  <img src="assets/figures/fig_07_ml_models.png" alt="The two ML models" width="95%"/>
</p>

🖼️ **Figure 8 — Predict regions, not points** — envelopes, ellipses and no-go polygons on one
chart:

<p align="center">
  <img src="assets/figures/fig_08_uncertainty.png" alt="Uncertainty visualisation" width="80%"/>
</p>

> 💬 **In plain language — what "uncertainty" buys us.** Instead of the system confidently saying
> "the float will surface here" and being wrong, it says "it will surface somewhere in this
> 40 km ellipse, and we are 80 % sure." The glider is sent to **loiter inside that ellipse**
> rather than to a point — which is exactly how the probability of a successful meeting is
> maximised. As the surfacing moment approaches and information improves, **the ellipse shrinks
> and the route tightens**.

### 7.2 Model 1 — Wave Glider trajectory prediction

This model forecasts the glider's motion over the mission horizon. It blends **data-driven
learning** with a **simplified physical propulsion model** (how waves and the rudder translate to
speed and heading).

| Aspect | Specification |
|---|---|
| **Inputs** | Current GPS position, heading and speed over ground; the glider's own recent track; forecast/nowcast wave direction/height/period; surface-current vectors; wind; solar/sea-state conditions; known steering and propulsion limits |
| **Outputs** | (1) most probable future trajectory; (2) the **reachable set** — locations the glider can realistically get to by each time; (3) an **uncertainty envelope** that grows with forecast horizon |
| **Architecture** | Time-series/sequence model (recurrent or temporal network) + physical propulsion head (physics-informed) |
| **Horizon** | Mission-relevant: typically out to the next rendezvous (+ a few days margin) |
| **Update cadence** | Re-run ashore on each new telemetry batch; lightweight inference on board between uplinks |

### 7.3 Model 2 — Argo Float surfacing prediction

This model predicts where the submerged, unpowered float will end up during its next surfacing.

| Aspect | Specification |
|---|---|
| **Inputs** | The float's last known GPS position and time; its park depth and dive profile; depth-resolved ocean-current forecasts and historical drift climatology; elapsed mission time |
| **Outputs** | A probability surface over surfacing locations (visualised as a confidence ellipse); an **uncertainty radius**; a **confidence score**; the likely **surfacing time window** |
| **Architecture** | Drift-learned sequence model + ensemble over perturbed current fields |
| **Horizon** | One cycle ahead (~10 days) |
| **Key calibration** | Coverage vs confidence — see §7.5 |

<p align="center">
  <img src="assets/figures/fig_19_rendezvous_prob.png" alt="Rendezvous probability and zone shrinkage" width="75%"/>
</p>

*Figure 19 — how the prediction sharpens: the probability zone shrinks as the surfacing window approaches, and the confidence score tells the planner how much to trust it.*

### 7.4 Ensemble and physics-informed forecasting

```mermaid
flowchart TB
    subgraph IN["inputs"]
        POS["positions & tracks"]
        WAV["wave / wind forecasts"]
        CUR["surface & depth-resolved currents"]
        CLIM["drift climatology<br/>(historical Argo + glider tracks)"]
        PHY["propulsion physics<br/>(glider limits)"]
    end

    subgraph ENS["ensemble forecasting"]
        SIM1["simulation 1<br/>perturbed conditions"]
        SIM2["simulation 2"]
        SIM3["simulation … N"]
        SPR["spread of outcomes<br/>= measure of uncertainty"]
    end

    subgraph OUT["outputs"]
        TRAJ["most probable trajectory<br/>+ reachable set"]
        ELL["surfacing ellipse<br/>+ radius + confidence"]
        WIN["surfacing time window"]
    end

    POS --> SIM1
    WAV --> SIM1
    CUR --> SIM1
    POS --> SIM2
    WAV --> SIM2
    CUR --> SIM2
    POS --> SIM3
    CLIM --> SIM1
    CLIM --> SIM2
    CLIM --> SIM3
    SIM1 --> SPR
    SIM2 --> SPR
    SIM3 --> SPR
    SPR --> TRAJ
    SPR --> ELL
    SPR --> WIN
    PHY --> TRAJ
```

| Design choice | Why |
|---|---|
| **Ensemble / Monte-Carlo** | Running many simulations with perturbed conditions and using the spread of outcomes as the uncertainty measure is the most honest way to represent a chaotic ocean. |
| **Physics guidance** | ML predictions are blended with established ocean-current and weather forecast models, so the system cannot predict motion that violates known currents or the glider's propulsion physics. |
| **No single-point outputs** | Every consumer of a prediction (planner, dashboard) works with regions, radii and scores. |

### 7.5 Training data and validation protocol

| Aspect | Approach |
|---|---|
| **Training data** | The global Argo float database (thousands of real dive/drift cycles), historical Wave Glider tracks, ocean reanalysis products, current/wind/wave forecasts, and any prior missions in the region |
| **Validation set** | Past cycles the models have **never seen** — predict where a real float actually surfaced |
| **Coverage metric** | Does the true surfacing point fall inside the predicted zone **at the advertised confidence**? (e.g. 80 %-zones should contain the truth ~80 % of the time) |
| **Sharpness metric** | How tight the zone is (radius in km at each horizon) — smaller is better **only if coverage holds** |
| **Regression gate** | A new model version must not reduce coverage below the advertised confidence or inflate the radius beyond the previous version's, on the full held-out set |
| **Continuous improvement** | Every real surfacing becomes a new labelled example; models re-train ashore and validated versions are deployed to the mission server; vehicles run lightweight inference on the latest predictions sent to them |

> 🧪 **ML validation precedes deployment.** Models are validated against existing Argo and glider
> datasets — predicting surfacings they have never seen — **before** ever guiding a vehicle. No
> model version ships without a validation report.

### 7.6 Model lifecycle and retraining

```mermaid
flowchart LR
    A["mission data<br/>+ historical Argo / glider"] --> B["feature engineering<br/>& dataset versioning"]
    B --> C["training + ensemble tuning"]
    C --> D["held-out cycle validation<br/>coverage + radius"]
    D --> E{"regression gate<br/>passed?"}
    E -- "no" --> C
    E -- "yes" --> F["model registry<br/>versioned artefact"]
    F --> G["deploy to mission server"]
    G --> H["serve predictions<br/>to planner + dashboard"]
    H -. "every surfacing outcome<br/>joins the dataset" .-> A
```

| Stage | Owner | Artefact |
|---|---|---|
| Dataset assembly & versioning | ML team | Versioned datasets + data cards |
| Training | ML team | Training runs, experiment tracker |
| Validation | ML team + NCPOR science | Validation report (coverage, radius, skill vs climatology) |
| Registry | Engineering | Model registry entry, SBOM, config |
| Deployment | Engineering + operators | Canary deploy to shadow mode → live |
| Monitoring | Dashboard + alerts | Prediction-skill diagnostics (§10.2, §11) |

### 7.7 Feature engineering reference

Features are versioned with the datasets they are derived from; a change to a feature definition
is a change to the training contract.

**Model 1 — glider trajectory features**

| Group | Features | Encoding notes |
|---|---|---|
| Kinematics | position (lat/lon), SOG, COG, heading | Last N samples as sequence window |
| Track history | displacement vectors over 6 h / 12 h / 24 h | Learned drift signature |
| Wave forcing | Hₛ, T_p, direction (forecast + nowcast) | Aligned to glider timestamps |
| Wind forcing | U10, V10, gust factor | Physical windage term |
| Currents | Surface current U, V from ocean forecast | Both absolute and relative-to-track |
| Energy | Solar yield history, SOC, load-shedding state | Constrains future mobility |
| Steering | Rudder position, recent turn rates | Encodes steering-effort cost |
| Static | Vehicle configuration, fin status, biofouling age | One-hot / scalar covariates |

**Model 2 — float surfacing features**

| Group | Features | Encoding notes |
|---|---|---|
| Origin | Last GPS fix (lat, lon, time), fix quality | Anchor for drift integration |
| Dive plan | Park depth, profile depth, cycle length | Determines which current layers matter |
| Currents | Depth-resolved U, V at park depth ±200 m | From ocean forecast/analysis |
| Climatology | Historical drift vectors for the region/month | Prior when forecasts are poor |
| Seasonal | Sea-ice fraction along projected path | Ice-aware surfacing prior |
| Mission | Elapsed cycles, battery state | Behavioral covariates (e.g. shortened cycles) |

### 7.8 Prediction pseudocode (ensemble surfacing forecast)

```python
def predict_surfacing_zone(float_state, currents, climatology, n_members=200):
    """Reference sketch — Model 2 ensemble. Contract for the ML team."""
    members = []
    for k in range(n_members):
        # 1. perturb the forcing fields within their known uncertainties
        cur_k = perturb(currents,  sigma=currents.sigma_field())
        # 2. integrate the float's drift: park phase → deep dive → ascent drift
        drift_k = integrate_drift(
            start=float_state.last_fix,
            park_days=float_state.cycle_days - 1.0,
            park_depth=float_state.park_depth_m,
            currents=cur_k,
            depth_profile=float_state.dive_profile,
        )
        # 3. add learned residual (data-driven correction to physics)
        residual_k = residual_model(float_state.features(), seed=k)
        members.append(drift_k + residual_k)

    # 4. fit the outcome distribution
    ellipse   = fit_confidence_ellipse(members, level=0.80)   # 80 % zone
    radius_km = ellipse.semi_major_km                          # headline uncertainty
    score     = calibrated_confidence(members, ellipse, level=0.80)
    window    = time_window(members)                           # earliest-latest surfacing
    return SurfacingZone(ellipse=ellipse, radius_km=radius_km,
                         confidence=score, window=window, n_members=n_members)
```

**Calibration requirement:** `score` must equal the empirical coverage of the ellipse on the
held-out set (§7.5). If coverage < score, the zone is *overconfident* — widen it. If coverage ≫
score, the zone is *underconfident* — tighten it. The planner consumes whatever the model
honestly reports.

### 7.9 Model registry, config and experiment tracking

| Practice | Tooling (proposed) | Requirement |
|---|---|---|
| Dataset versioning | DVC or Git-LFS pointers | Training data pinned per run |
| Experiment tracking | MLflow or W&B | Every run logged with config, metrics, artefacts |
| Model registry | MLflow registry or custom | Versioned artefacts with SBOM; only validated versions deploy |
| Config management | YAML + schema validation | Config is code — reviewed like code |
| Evaluation snapshots | `ml/validation/report-<date>.md` | Committed with every model PR |

Example training config (`configs/float_v1.yaml`, illustrative):

```yaml
model: float_surfacing_v1
data:
  argo_history: s3://ocean-data/argo/gdac/*.nc      # public Argo profiles
  region: [-75, -45, -90, 20]                       # Southern Ocean bounding box
  min_cycles_per_float: 20
  split: { train: 0.7, val: 0.15, test: 0.15 }      # test = held-out floats (not cycles!)
ensemble:
  n_members: 200
  perturbation: { currents: 0.15, wind: 0.10 }      # relative sigma
training:
  epochs: 60
  patience: 8
  loss: crps                                        # proper scoring rule, not MSE
validation:
  confidence_levels: [0.5, 0.8, 0.95]
  coverage_tolerance: 0.05                          # coverage ≈ level ± 5 %
  radius_regression_limit_pct: 10                   # max allowed radius growth
```

> 🧪 Note the split rule: the test set is **held-out floats**, not held-out cycles of seen
> floats. Testing on unseen platforms is the only honest estimate of how the model will behave
> for *our* float, which it has never seen before.

### 7.10 Monitoring models in production

| Watch item | Alert when | Response |
|---|---|---|
| Coverage drift | 10-cycle rolling coverage < advertised − 5 % | Retrain window; enlarge ensemble spread temporarily |
| Radius inflation | Rolling median radius > 1.5× validation baseline | Investigate current-field quality; fall back to climatology prior |
| Input staleness | Forecasts older than TTL | Planner flags degraded inputs and widens zones |
| Label latency | Surfacing outcomes not ingested within 48 h | Data-pipeline alert (§10.3) |
| Concept drift | Seasonal shift in residual distribution (KS test) | Schedule seasonal retraining |

### 7.11 Prediction reporting contract

Every prediction artefact consumed by the planner or dashboard conforms to this contract — no
consumer ever parses a "point forecast".

| Field | Type | Meaning |
|---|---|---|
| `model_version` | string | Registry version (§7.9) that produced the artefact |
| `valid_from_utc` / `valid_to_utc` | ISO 8601 | Forecast validity window |
| `geometry` | GeoJSON | Trajectory band / reachable set / ellipse polygon |
| `uncertainty_radius_km` | float | Headline uncertainty at the advertised level |
| `confidence` | float 0–1 | Calibrated probability of containment |
| `window` | [t₀, t₁] | Expected surfacing time interval |
| `n_members` | int | Ensemble size behind the artefact |
| `inputs_freshness` | map | Age of each forcing input at generation time |
| `degraded` | bool | True if inputs stale → zone widened accordingly |

**Downstream rules**

- The planner must widen zones when `inputs_freshness` exceeds TTLs (§7.10) — a stale
  prediction is consumed more cautiously, never less.
- The dashboard renders the zone as a filled ellipse with the confidence in the label, and the
  input staleness as a corner badge.
- Prediction artefacts are archived with the cycle (§11.6) so every decision can be replayed.

### 7.12 Interpretability and operator trust

Operators do not need to understand the model's internals — but they do need to be able to ask
"why is the zone where it is?" and get a useful answer.

| Question an operator asks | Answer the system provides |
|---|---|
| Why is the zone here? | Attribution panel: dominant current component (direction/speed), climatology contribution, learned residual — all from the last run |
| Why has it moved since yesterday? | Diff view: which input changed most (currents / wind / ice / model update) |
| How much should I trust it? | Confidence + calibration history for this region/season + input staleness badge |
| What would make it wrong? | Worst-case ensemble members shown as ghost ellipses (the spread) |

**Trust design rules**

1. Show the *spread*, not just the summary — ghost members make uncertainty tangible.
2. Never display a single-point surfacing forecast anywhere in the UI.
3. Every override exercised by an operator is compared post-hoc against what the model would
   have done — the diff is discussed, not scored, keeping humans in the loop without blame.

### 7.13 Prediction failure-mode analysis

Every way a prediction can be wrong has a designed response. This table is reviewed whenever
the models change.

| Failure mode | Symptom | Consequence if unhandled | Designed response |
|---|---|---|---|
| Overconfident zone | Coverage < advertised confidence | Glider loiters outside the true surfacing area | Calibration monitor (§7.10) widens zones; climatology fallback |
| Underconfident zone | Coverage ≫ confidence, huge radius | Glider wastes energy loitering a vast area | Calibration tightens; planner caps loiter radius at reachability limits |
| Stale forcing inputs | Forecast age > TTL | Drift diverges from reality silently | `inputs_freshness` propagated (§7.11); zones widened; planner flagged degraded |
| Regime change | Season/ice shift the drift statistics | Learned model wrong in new regime | Drift monitoring (KS test §7.10); seasonal retraining; prior re-weighting |
| Ensemble collapse | Members converge artificially | Spread understates risk | Ensemble diagnostics: spread floor enforced by perturbation budget (§7.4) |
| Label poisoning / bad fix | Surfacing GPS fix wrong | Next cycle trained on garbage | Fix-quality gate (M02/M11 §4.7); outlier rejection in training |
| Model-server outage | No predictions available | Planner blind | Planner falls back to last valid zone + climatological growth rate; alerts raised |

**Governing principle.** A *wide but honest* prediction is always preferable to a *tight but
wrong* one. The system is allowed to be slow; it is not allowed to be silently wrong.

### 7.14 Dataset governance

| Policy | Rule |
|---|---|
| Dataset versioning | Every training dataset has an ID, manifest and checksum (§7.9) |
| Licensing | Public datasets (Argo GDAC, reanalysis) used per their licences; mission data governed by §20.3 |
| Privacy | No personal data anywhere in the pipeline |
| Provenance | Datasets cite sources and preprocessing scripts; reproducible via `ml/` code |
| Retention | Mission-derived training data retained for the programme's life |
| Access | Datasets internal by default; exportable with the data policy at publication |

---

### 7.15 A worked prediction example

To make the machinery of §7.1–7.14 concrete, here is one synthetic cycle followed number by
number. It mirrors the artefacts shown in Appendix N and is the canonical example used in
training (§14.7) and in the planner test bank (§8.9, scenario T-03).

**The setup (cycle 17, synthetic).** The float last surfaced at 62.31°S, 33.99°E and dived to a
2,000 m target with a 1,000 dbar park depth. The glider finished its previous rendezvous with
78 % SOC and 340 Wh of solar reserve. Currents at park depth are forecast at 0.12 m/s toward
055° with 40 % uncertainty; the surface wind is 12 m/s from 300°.

| Day (D−N) | Zone radius | Ensemble spread (km) | Empirical coverage at 80 % target | Operator action |
|---|---|---|---|---|
| D−9 | 54 km | 41 | 0.71 — underconfident, widened | Watch only; climatology prior dominant |
| D−6 | 35 km | 26 | 0.78 | Checkpoint set toward zone edge (§8.5) |
| D−3 | 21 km | 14 | 0.81 | Loiter box tightened; charge budget confirmed |
| D−0 (window) | 11 km | 7 | 0.84 | Glider holds inside final zone; dock on announce |

```mermaid
flowchart LR
    A["🧊 INPUTS<br/>last fix · park depth<br/>current forecasts · ice"] --> B["🧬 DRIFT MODEL<br/>sequence model, physics prior (§7.2)"]
    B --> C["🎲 ENSEMBLE<br/>200 perturbed members (§7.4)"]
    C --> D["📐 ZONE + SPREAD<br/>ellipse, radius, confidence"]
    D --> E["📜 CONTRACT<br/>coverage/radius reported (§7.11)"]
    E --> F{"coverage ≥ 0.8?"}
    F -- "yes" --> G["✅ publish zone<br/>planner consumes directly"]
    F -- "no" --> H["⚠️ widen + flag degraded<br/>climatology fallback"]
    H --> G
    G --> I["📈 SCORECARD<br/>post-surfacing skill update (§6.8, §7.10)"]
```

<p align="center">
  <img src="assets/figures/fig_26_prediction_worked_example.png" alt="Worked prediction example" width="95%"/>
</p>

*Figure 26 — the zone shrinks as the window nears (left), and the ensemble spread falls while
empirical coverage rises (right). This is the shape of a healthy prediction: honest early,
tight late.*

**What the numbers teach**

1. **Early zones are wide on purpose.** At D−9 the model is dominated by the climatology prior;
   advertising a 54 km zone is correct behaviour, not failure (§7.1).
2. **Coverage is the contract, not the zone size.** The system never claims precision it cannot
   back (§7.11); the D−0 claim is "80 % of the time the float is inside 11 km" — and the
   scorecard will verify exactly that.
3. **Every cycle retrains the prior.** The D−0 fix becomes the next cycle's training label
   (§7.5); over a season the climatology prior recedes and the learned prior dominates.

> 🎯 If the float surfaces outside even the D−0 zone, the fix is still useful: it feeds the
> failure-mode analysis of §7.13 and, through the burst path (§6.5), the glider reroutes to the
> true position while battery permits.

## 8. Planning, Ice Avoidance and Energy

### 8.1 The planning engine: from forecasts to a feasible route

A trajectory-planning engine combines the outputs of the two ML models with live environmental
data and vehicle constraints. It does **not** simply aim the glider at the predicted point: it
searches for the route the glider can **actually sail** under the prevailing waves and currents,
expressed as a sequence of **feasible checkpoints**, and it re-runs continuously as new
information arrives.

```mermaid
flowchart LR
    M1["Model 1<br/>glider reachable region"] --> IN["intersect with<br/>currents · waves · bathymetry<br/>ice charts · detected hazards"]
    M2["Model 2<br/>float surfacing zone"] --> IN
    IN --> GEN["generate feasible checkpoints<br/>biased to travel WITH waves & currents"]
    GEN --> CH["choose rendezvous region<br/>+ target arrival time<br/>arrive early → loiter"]
    CH --> UP["uplink checkpoints to glider"]
    UP -. "new data triggers re-run" .-> M1
    UP -. "new data triggers re-run" .-> M2
```

🖼️ **Figure 9 — The adaptive planning loop** (five steps, re-run on every new input):

<p align="center">
  <img src="assets/figures/fig_09_planning_loop.png" alt="Adaptive planning loop" width="85%"/>
</p>

🖼️ **Figure 10 — Feasible checkpoint generation.** The straight dashed route toward the float's
surfacing zone is blocked by an ice hazard and ignores currents. The engine instead produces a
green sequence of numbered checkpoints that stay in reachable water and clear of the safety
stand-off, bending around the danger to arrive in the predicted surfacing zone.

<p align="center">
  <img src="assets/figures/fig_10_checkpoints.png" alt="Checkpoint generation around hazard" width="85%"/>
</p>

#### The adaptive planning loop (expanded)

1. **Predict** the glider's reachable region (Model 1) and the float's surfacing zone (Model 2).
2. **Intersect** both with current/wind/wave forecasts, bathymetry, sea-ice charts and detected
   hazards.
3. **Generate** a sequence of feasible checkpoints toward the target zone, biased to travel with
   waves and currents.
4. **Choose** the rendezvous region and target arrival time so the glider can arrive early and
   loiter, balancing expected travel, drift of the loiter point, and confidence.
5. **Re-run** whenever new forecasts, positions, sensor data or hazard updates arrive; uplink the
   revised checkpoints.

### 8.2 The optimisation objective

The objective is deliberately stated as a **probability, not a distance**:

> **Maximise P(successful rendezvous), while minimising unnecessary travel and energy
> consumption**, subject to wave, current, steering, battery and ice constraints.

| Term | Formal meaning |
|---|---|
| `P(successful rendezvous)` | Probability that the glider is inside the float's surfacing zone at surfacing time, and both vehicles are healthy and ice-clear |
| `travel cost` | Expected distance/energy the glider spends to reach and hold the zone |
| `constraints` | Reachable-set membership, steering limits, battery floor, ice no-go polygons, loiter drift |

The glider can operate fully autonomously under this logic, follow operator-set checkpoints, or
be manually overridden at any time.

### 8.3 Iceberg and sea-ice hazard avoidance

Hazards enter the planner from three complementary sources:

1. **On-board sensing** — e.g. marine radar, optical sensing, AIS where relevant;
2. **External environmental data** — operational sea-ice charts and iceberg analyses;
3. **The optional dedicated iceberg tracker** (§12).

Each detected hazard is converted into a **polygon plus a safety stand-off buffer** and placed on
the same map as the routes.

```mermaid
flowchart TB
    subgraph SRC["hazard sources"]
        RAD["on-board radar / optical"]
        CHT["sea-ice charts<br/>iceberg analyses"]
        TRK["optional on-ice tracker"]
    end
    RAD --> POLY["fuse → hazard polygon<br/>+ safety stand-off buffer"]
    CHT --> POLY
    TRK --> POLY
    POLY --> TEST{"planned & predicted<br/>trajectories intersect?"}
    TEST -- "no" --> OK["continue plan"]
    TEST -- "yes" --> REROUTE["generate alternative<br/>checkpoints around danger"]
    REROUTE --> CHECK{"safe route exists?"}
    CHECK -- "yes" --> OK
    CHECK -- "no" --> SAFE["safest alternative:<br/>HOLD / DIVERT / ABORT<br/>+ alert operators"]
```

🖼️ **Figure 11 — Three-step ice-hazard handling** (fuse → test → respond):

<p align="center">
  <img src="assets/figures/fig_11_ice_handling.png" alt="Ice hazard handling steps" width="95%"/>
</p>

> 🛡️ **Fail-safe philosophy.** When in doubt, the system chooses the safe option: keep clear of
> the hazard, preserve the vehicle and its data, and surface the situation to operators. **An
> unreachable rendezvous never justifies forcing the glider into ice.** The float retains its
> direct-satellite backup so a profile is still delivered even if the glider must hold clear.

### 8.4 Energy: where the power comes from and where it goes

The glider exploits two different forms of ocean energy, kept deliberately separate — this is the
key to long endurance:

- **Waves** provide mechanical propulsion — the fin mechanism needs **no electricity**.
- **Sunlight** provides electrical energy through the deck solar panels, which charge the
  batteries that power sensors, computing, satellite bursts, steering and the float recharge.

```mermaid
flowchart LR
    SUN["SUNLIGHT"] --> PAN["solar panels"]
    PAN --> BAT["glider battery"]
    BAT --> SEN["sensors"]
    BAT --> GPS["GPS / satcom"]
    BAT --> STR["steering"]
    BAT --> CPU["compute"]
    BAT --> FLT["float recharge<br/>(per-cycle top-up via dock)"]
    WAV["WAVE MOTION"] --> FIN["submerged fin rack"]
    FIN --> THR["mechanical thrust — 0 Wh"]
```

🖼️ **Figure 12 — Energy flow and budget logic:**

<p align="center">
  <img src="assets/figures/fig_12_energy_flow.png" alt="Energy flow diagram" width="95%"/>
</p>

#### Budget rules

| Rule | Consequence |
|---|---|
| Solar array sized for **electrical loads + per-cycle float top-up** | Covers sensors, compute, satcom, steering **and** the rendezvous recharge |
| Margin for **cloudy spells, winter darkness, heavy satellite use** | Worst-week, not average-day, sizing |
| **Load-shedding priority order** | (1) non-essential motion & transmission frequency → (2) protect safety functions & the weather record → (3) fall back to float's direct satellite burst |
| **Charge targets, not full charges** | Each rendezvous charges to a safe target SOC within the planned surface window (§6.3) |

🖼️ **Figure 18 — Seasonal energy reality check** — illustrative solar yield and a year-long
battery simulation with load-shedding band:

<p align="center">
  <img src="assets/figures/fig_18_solar_soc.png" alt="Seasonal solar yield and battery SOC simulation" width="95%"/>
</p>

> ⚠️ **Engineering honesty.** Antarctic winter is dark and long. The energy plan does not promise
> full operations through midwinter — it promises **safe degradation**: reduced motion and
> transmission, protected data, and full recovery when daylight returns.

### 8.5 Checkpoint generation — reference algorithm

```python
def plan_checkpoints(glider, zone, hazards, env, horizon_h=72):
    """Reference sketch — the planner's core loop. Contract for the planner team."""
    checkpoints = []
    t = 0.0
    pos = glider.position
    reachable = model1.reachable_set(pos, env, horizon_h)

    while t < horizon_h:
        # 1. candidate next point: bias travel WITH the waves and currents
        candidates = sample_candidates(pos, env, n=200, bias="downwave_downcurrent")
        # 2. filter: physically reachable within the step window
        candidates = [c for c in candidates if c in reachable(t + step_h)]
        # 3. filter: clear of every hazard polygon plus stand-off
        candidates = [c for c in candidates if not hazards.intersects(c, standoff=glider.standoff)]
        if not candidates:
            # 4. no safe progress → hold (or abort per fail-safe policy §8.3)
            checkpoints.append(Hold(pos, reason="no_safe_progress"))
            break
        # 5. score by: progress toward zone + P(rendezvous) + energy cost
        best = max(candidates, key=lambda c: score(c, zone, t, glider.battery))
        checkpoints.append(Checkpoint(best, t_est=t + step_h))
        pos, t = best, t + step_h
        # 6. early exit: inside the zone → switch to loiter plan
        if zone.contains(pos, pad=loiter_radius(zone)):
            checkpoints.append(Loiter(pos, until=zone.window.open))
            break
    return checkpoints

def score(candidate, zone, t, battery):
    """Optimisation objective from §8.2, scalarised for search."""
    p_meet    = P(glider_in_zone_at_window | via=candidate)
    energy    = travel_cost(candidate) + hold_cost(zone.window)
    risk      = hazard_proximity_penalty(candidate)
    return p_meet - LAMBDA_E * energy - LAMBDA_R * risk
```

| Tuning parameter | Meaning | Default source |
|---|---|---|
| `step_h` | Replan granularity (hours) | 6 h; shortened to 1 h inside D−1 day |
| `LAMBDA_E` | Energy weight in the objective | Phase 2 coastal calibration |
| `LAMBDA_R` | Risk-aversion weight | Never tuned below the fail-safe floor |
| `loiter_radius` | Zone-inside hold distance | 0.5–1.0 × zone semi-minor axis |

### 8.6 Cost function detail

The scalarised objective in §8.2 expands to:

```text
J(plan) = w1 · P(successful rendezvous | plan)
        − w2 · E[energy used by glider | plan]
        − w3 · Σ hazard-proximity penalties
        − w4 · steering-effort penalty          (mechanical wear on rudder/fins)

subject to:
    every checkpoint ∈ reachable set (Model 1)
    every checkpoint ∉ hazard polygon ⊕ stand-off
    battery SOC ≥ SOC_floor at all times
    loiter drift ≤ zone containment margin
```

- `w1` is the mission's reason to exist; `w2..w4` stop the glider wasting itself.
- `SOC_floor` is dynamic: it rises (more conservative) as solar season weakens, per §8.4.
- The planner reports the *top-K* plans with their probabilities — the dashboard shows the
  chosen one and the margin to the runner-up, so operators can see how much "slack" exists.

### 8.7 Stand-off sizing and hazard policy

| Hazard type | Detection | Stand-off (illustrative) | Response |
|---|---|---|---|
| Tabular iceberg (tracked) | Tracker + charts + radar | 2× predicted drift/day + 5 km | Polygonal exclusion, refreshed per report |
| Tabular iceberg (untracked, charted) | Iceberg analyses | 20 km | Conservative exclusion; decay when chart ages |
| Pack ice / marginal ice zone | Sea-ice charts (daily) | Ice-edge + 10 km | No-go past edge; float surfacing logic ice-aware |
| Growlers / bergy bits (local) | On-board radar/optical | 1 km detection-triggered | Immediate divert + slow speed |
| Unknown contact (AIS/radar) | AIS + radar | COLREG-informed | Track and avoid; log encounter |

> 🛡️ **Stand-off philosophy.** Stand-offs are chosen so that a *worst-case* drift of the hazard
> during one planning cycle cannot close the gap before the next replan. If a stand-off cannot
> be honoured, the system holds and alerts (§8.3), it never squeezes through.

### 8.8 Planner output example

```json
{
  "plan_id": "PLN-2026-11-09T06Z-4",
  "generated_utc": "2026-11-09T06:00:00Z",
  "objective": { "p_success": 0.81, "expected_energy_wh": 340, "risk_score": 0.02 },
  "zone": { "center": [-62.40, 34.20], "semi_major_km": 18, "semi_minor_km": 11,
            "bearing_deg": 41, "confidence": 0.80, "window": "2026-11-12T03:30Z/05:30Z" },
  "checkpoints": [
    { "index": 1, "lat": -62.05, "lon": 33.40, "eta": "2026-11-09T12:00Z", "type": "transit" },
    { "index": 2, "lat": -62.18, "lon": 33.75, "eta": "2026-11-10T00:00Z", "type": "transit" },
    { "index": 3, "lat": -62.31, "lon": 34.05, "eta": "2026-11-10T18:00Z", "type": "transit" },
    { "index": 4, "lat": -62.40, "lon": 34.20, "eta": "2026-11-11T06:00Z", "type": "loiter",
      "loiter_until": "2026-11-12T03:30Z" }
  ],
  "hazards_considered": ["A-23a (tracked)", "MIZ 2026-11-09 chart"],
  "fallbacks": ["extend_surface_window", "float_direct_burst"],
  "status": "awaiting_execution"
}
```

### 8.9 Planner test scenarios

The planner ships with a regression suite; each scenario has a stored expectation, and CI
re-runs them on every planner change.

| # | Scenario | Setup | Expected behaviour |
|---|---|---|---|
| T-01 | Calm drift | Weak currents, no hazards | Checkpoints mostly downwave/downcurrent; minimal steering cost |
| T-02 | Headwind transit | Strong adverse wind, calm sea | Planner either waits or takes the long way — never burns energy against the wind |
| T-03 | Iceberg on direct route | Polygon between glider and zone | Route bends around with full stand-off; objective degrades gracefully |
| T-04 | Zone unreachable | Zone beyond reachable set within window | Plan targets best partial progress; operator alerted; float-burst fallback armed |
| T-05 | Hazard appears mid-plan | Tracker update 6 h after plan | Re-plan triggered; new checkpoints uploaded within one cycle |
| T-06 | Hazard drifts onto loiter point | Loiter inside zone, iceberg approaches | Glider re-loiters to zone edge; stand-off never violated |
| T-07 | Battery floor breached | SOC below floor during transit | Load-shedding plan; rendezvous deprioritised vs survival |
| T-08 | Forecast quality collapse | Inputs marked stale | Zones widened; plan becomes conservative; `degraded` flag propagated |
| T-09 | Surface window shift | Float window moves ± 6 h | Arrival timing re-optimised; early-arrival margin maintained |
| T-10 | Full-season replay | 365-day simulation | Year-level KPIs (§14.4) within targets; no stand-off violations |

**Property checks (always on)**

- **Reachability invariant** — no checkpoint outside the reachable set.
- **Safety invariant** — no checkpoint inside a hazard polygon or stand-off.
- **Monotonicity** — a plan never prefers a strictly worse P(success) with higher energy.
- **Idempotence** — re-running with identical inputs reproduces the identical plan.

### 8.10 Bathymetry and navigation constraints

| Constraint | Source | Planner handling |
|---|---|---|
| Tether depth vs seabed | Glider tether several metres; float dives 2,000 m | Operating box must have sufficient depth; bathymetry checked at box selection (§2.7) and per checkpoint |
| Shallow banks/ridges | Bathymetric charts | Checkpoints never route across features shallower than safe margin |
| Current shear at fronts | Forecast fields | Checkpoint generation favours across-front travel at slack/least-shear times |
| Restricted ice corridors | Ice charts + polygons | Corridor width vs glider tracking error enforced (§8.7 stand-off) |
| Depth-rated hull | Float rating ≥ 2,000 m | `profile_depth_m` config bounded by hull margin (§5.2 params) |

### 8.11 Planner performance budgets

The planner runs ashore on the mission server; its performance is part of the ops contract.

| Budget | Target (illustrative) |
|---|---|
| Full replan latency (6-hourly) | ≤ 60 s wall clock |
| Emergency replan (hazard update) | ≤ 15 s to first new checkpoint |
| Prediction inference (Model 1 + 2) | ≤ 10 s combined |
| Simulation ensemble (200 members) | ≤ 120 s (parallelised) |
| Availability | ≥ 99.5 % during ops season |
| Determinism | Same inputs → same plan (§8.9 idempotence) |

**Scaling note.** The architecture (§17.2 contracts) keeps the planner a pure function of its
inputs — a second pair of vehicles is additional planner instances, not additional complexity.

---

### 8.12 Annual energy budget — worked numbers

§8.4 describes the energy flow qualitatively; here is the same logic as a year of numbers. The
worked budget below is the *sizing case* for the whole mission — it is what the array area, the
load-shedding ladder and the winter doctrine (§13.10) are all derived from.

| Month (SH season) | Solar (kWh/day) | Wave (kWh/day) | Consumption (kWh/day) | Daily balance | Notes |
|---|---|---|---|---|---|
| Nov (deploy) | 0.5 | 1.1 | 1.3 | +0.3 | Post-deployment checks, gentle transit |
| Dec | 0.3 | 1.0 | 1.2 | +0.1 | High-latitude daylight |
| Jan | 0.4 | 1.2 | 1.3 | +0.3 | First full rendezvous cadence |
| Feb | 0.2 | 1.2 | 1.4 | 0.0 | Break-even month |
| Mar | 1.3 | 1.0 | 1.6 | +0.7 | Light returns; charge the reserve |
| Apr | 2.6 | 1.2 | 1.7 | +2.1 | Reserve build-up begins in earnest |
| May | 4.0 | 1.0 | 1.7 | +3.3 | Peak charging season |
| Jun | 4.6 | 1.4 | 1.8 | +4.2 | Winter solstice lighting at its best |
| Jul | 4.3 | 1.5 | 1.9 | +3.9 | Midwinter: still net positive |
| Aug | 3.2 | 1.3 | 1.8 | +2.7 | Light fading; store while possible |
| Sep | 2.0 | 1.4 | 1.6 | +1.8 | Shoulder season |
| Oct (recover) | 0.9 | 1.3 | 1.4 | +0.8 | Recovery transit |

<p align="center">
  <img src="assets/figures/fig_27_annual_energy_budget.png" alt="Annual energy budget" width="95%"/>
</p>

*Figure 27 — production against consumption through a Southern Hemisphere season, with the
cumulative balance below. The design target: **never let the cumulative balance cross zero**
during the dark months — the load-shedding ladder (§8.4) is the guarantee, not a hope.*

```mermaid
flowchart TD
    A{"⚡ energy state<br/>this hour"} --> B{"balance > 0?"}
    B -- "yes, ample" --> C["🔋 charge float at next rendezvous<br/>raise SOC target (§6.8)"]
    B -- "yes, modest" --> D["🌊 normal ops<br/>full science cadence"]
    B -- "no, deficit" --> E{"deficit < 15 %?"}
    E -- "yes" --> F["🪜 shed ladder 1–3<br/>met cadence 10 min · wave bursts 2 h · fewer passes"]
    E -- "no" --> G{"deficit < 40 %?"}
    G -- "yes" --> H["🪜 shed ladder 4–6<br/>science to store-and-forward · transit-only ops · no charging"]
    G -- "no" --> I["🛡️ SAFE HOLD<br/>minimal motion, solar-only recharge, alert operators (§9.3)"]
    I --> A
    H --> A
    F --> A
    D --> A
    C --> A
```

**The load-shedding ladder, priced.** Each rung buys a known amount of energy per day
(illustrative); the planner climbs rungs in order and descends as soon as the balance allows:

| Rung | Action | Energy saved (kWh/day) | Science cost |
|---|---|---|---|
| 1 | Met sampling 1 min → 10 min | 0.35 | Coarser surface record (still useful, §10.2) |
| 2 | Wave bursts 20 min → 2 h | 0.20 | Coarser wave spectra |
| 3 | Passes 3/day → 1/day | 0.15 | Slower alert round-trips (§13.7) |
| 4 | Science to store-and-forward only | 0.30 | Delayed, not lost (§6.5) |
| 5 | Transit-only glider motion | 0.50 | No active science between checkpoints |
| 6 | No float charging | 0.40 | Float runs on its own reserve |
| — | SAFE HOLD | 1.20+ | Everything except survival and position |

> 🔢 **The winter arithmetic.** Jun–Aug consumes ~49.5 kWh; production supplies ~56.9 kWh. The
> surplus (~7.4 kWh) is the entire winter margin — it is why the float is charged in summer, why
> the glider carries a large battery (§5.7 RAM targets), and why winter operations doctrine
> treats every watt as mission-critical.

## 9. Embedded Software, Health Monitoring and Debugger

Autonomy is only trustworthy if the machines continuously monitor their own health **and** a
human can verify everything before it touches the water. A shared embedded software layer runs
on both vehicles, and a rugged hand-held debugger is used by the on-site engineer during
deployment.

### 9.1 The on-board software architecture

Both vehicles run the **same layered embedded architecture** (compiled for their different
hardware):

```mermaid
flowchart TB
    subgraph WG["WAVE GLIDER build"]
        WGA["mission apps<br/>navigation · comms<br/>dock/charge controller"]
        WGO["embedded OS<br/>scheduler · drivers · storage<br/>timekeeping · watchdogs"]
        WGH["hardware<br/>sensors · actuators<br/>batteries · radios"]
        WGA --> WGO --> WGH
    end
    subgraph CORE["SHARED CORE (both vehicles)"]
        SM["mission state machine"]
        HM["health & battery monitor"]
        LG["time-stamped event logger"]
        FL["fault localisation<br/>sensor / comm / power / control"]
    end
    subgraph AF["ARGO FLOAT build"]
        AFA["mission apps<br/>dive/profile control<br/>ice-aware surfacing<br/>buoyancy engine driver"]
        AFO["embedded OS<br/>scheduler · drivers · storage<br/>timekeeping · watchdogs"]
        AFH["hardware<br/>CTD · pump · bladder<br/>battery · radio"]
        AFA --> AFO --> AFH
    end
    WGA -.-> SM
    WGA -.-> HM
    WGA -.-> LG
    AFA -.-> SM
    AFA -.-> HM
    AFA -.-> LG
    FL -.-> SM
    FL -.-> HM
    FL -.-> LG
```

🖼️ **Figure 13 — Matched software stacks** for the glider and float (hardware → embedded OS →
mission applications), plus the shared core:

<p align="center">
  <img src="assets/figures/fig_13_software_stack.png" alt="Embedded software stack" width="95%"/>
</p>

| Layer | Responsibility | Notes |
|---|---|---|
| **Hardware** | Sensors, actuators, batteries, radios | Different per vehicle |
| **Embedded OS** | Scheduling, drivers, storage, accurate timekeeping, watchdog timers | Same core, per-vehicle drivers |
| **Mission applications** | Float: dive/profile control. Glider: navigation, communications, docking/charge controller | Vehicle-specific |
| **Shared core** | Mission state machine, health-and-battery monitor, time-stamped data-and-event logger, fault localisation | Identical on both |

### 9.2 What the system monitors

| Monitor | Signals checked | Failure action |
|---|---|---|
| **Sensor status** | Every instrument responding and reading within valid ranges | Flag sensor; keep others running (graceful degradation) |
| **Battery & charging** | State of charge, charging current, cell temperature | Load-shedding; charge termination on over-temperature |
| **GPS** | Fix quality and time-to-fix at the surface | Retry; dead-reckon; alert |
| **Communications** | Short-range link and satellite link quality and traffic | Store-and-forward; retry schedules |
| **Storage** | Capacity used, read/write integrity, successful data offload | Protect science data; prioritise offload |
| **Mission state** | Which phase of the cycle each vehicle is in | Detect stuck states; watchdog recovery |
| **System errors** | Watchdog resets, pump/rudder faults, leaks, temperature extremes | Safe-hold; alert; localise fault layer |

Every reading and every fault is **time-stamped** and appended to a persistent event log, both on
the vehicle and (when connected) on the shore server. These logs let engineers trace a problem to
its layer — **sensor, communication, power or vehicle control** — whether it appears in real time
on the dashboard or after recovery.

### 9.3 The health-monitoring loop

```mermaid
flowchart LR
    READ["read all sensors<br/>& subsystems"] --> EVAL{"values in range?"}
    EVAL -- "yes" --> LOG["append timestamped<br/>entry to event log"]
    EVAL -- "no" --> CLASS["classify fault layer:<br/>sensor / comm / power / control"]
    CLASS --> ACT["act: retry / shed load /<br/>safe-hold / switch path"]
    ACT --> ALERT["queue alert for next<br/>uplink (or immediate if critical)"]
    LOG --> SYNC["sync log to shore<br/>when link available"]
    ALERT --> SYNC
    SYNC -. "watchdog ensures loop never dies" .-> READ
```

### 9.4 The hand-held pre-deployment debugger

Before either device leaves the ship, the on-site engineer works through a verification checklist
on a **rugged, water-resistant hand-held tablet** that plugs into each vehicle. It turns
deployment from an act of faith into a **signed-off quality gate**.

```mermaid
flowchart TB
    PLUG["plug debugger into vehicle"] --> RUN["run guided checklist"]
    RUN --> C1{"sensors pass?"} -->|"no"| FIX["fix on deck → re-test"]
    C1 -->|"yes"| C2{"power / charging pass?"} -->|"no"| FIX
    C2 -->|"yes"| C3{"GPS fix acquired?"} -->|"no"| FIX
    C3 -->|"yes"| C4{"sat + short-range links pass?"} -->|"no"| FIX
    C4 -->|"yes"| C5{"storage, clock & logging pass?"} -->|"no"| FIX
    C5 -->|"yes"| C6{"actuators self-test pass?"} -->|"no"| FIX
    C6 -->|"yes"| C7{"mission plan loaded & confirmed?"} -->|"no"| FIX
    C7 -->|"yes"| READY["ALL SYSTEMS READY<br/>signed off · cleared for launch"]
    FIX --> RUN
```

🖼️ **Figure 20 — The pre-deployment quality gate** — every subsystem must pass before the
vehicle is cleared for launch:

<p align="center">
  <img src="assets/figures/fig_20_debugger_gate.png" alt="Debugger quality gate" width="85%"/>
</p>

| Pre-deployment check | What the engineer verifies |
|---|---|
| **Sensors** | Each instrument powers up, responds and returns in-range readings; CTD pump/factory checks as applicable. |
| **Power** | Battery is charged; charging circuit and dock connector tested; solar input confirmed. |
| **GPS** | A valid position fix is acquired within the expected time. |
| **Communications** | Satellite modem registers and passes a test message; short-range float↔glider link tested pairwise. |
| **Storage & clock** | Memory empty/writable; on-board clock set and synchronised; logging confirmed. |
| **Actuators** | Float buoyancy pump self-test; glider rudder/fin and lock mechanisms cycle correctly. |
| **Mission plan** | Correct mission configuration and cycle parameters loaded, reviewed and confirmed. |

> ⚠️ **Why this matters.** Once deployed, a vehicle may not see a human again for a year. A
> five-minute checklist on deck — with clear pass/fail gates and a logged record — prevents the
> most common and most avoidable cause of lost missions: a dead battery, loose connector, wrong
> clock or failed link discovered only after the device is over the horizon.

### 9.5 Event log schema

Every vehicle maintains a ring-buffer event log in flash; entries are streamed to shore whenever
a link exists. The schema is shared with the shore server so logs merge seamlessly.

| Field | Type | Example | Notes |
|---|---|---|---|
| `seq` | u32 | 00041832 | Monotonic per vehicle; gaps indicate resets |
| `ts_utc` | ISO 8601 | `2026-11-12T03:58:12.041Z` | Vehicle clock, corrected by GNSS |
| `source` | enum | `health_monitor` | Which component logged it |
| `event` | enum | `watchdog_reset` | Controlled vocabulary |
| `layer` | enum | `power` | sensor / comm / power / control (fault localisation) |
| `severity` | enum | `warning` | info / warning / error / critical |
| `value` | JSON | `{"soc_pct": 41}` | Structured context |
| `linked` | u32[] | `[00041830]` | Correlation to cause events |

Example entries:

```json
{ "seq": 41832, "ts_utc": "2026-11-12T03:58:12Z", "source": "health_monitor",
  "event": "charge_complete", "layer": "power", "severity": "info",
  "value": { "float_soc_pct": 78, "target_soc_pct": 78, "wh_delivered": 6.1 } }
```

```json
{ "seq": 41833, "ts_utc": "2026-11-12T03:58:13Z", "source": "docking_controller",
  "event": "release_confirmed", "layer": "control", "severity": "info",
  "value": { "lock_sensor": "open", "attempts": 1 }, "linked": [41820, 41832] }
```

### 9.6 Watchdog and reset design

| Watchdog | Scope | Action on timeout | Rationale |
|---|---|---|---|
| OS task watchdog | Every task must kick within N ms | Task restart, then system reset | Catches stuck loops |
| Mission watchdog | State transitions must occur within Tₘₐₓ | Safe-hold state entry | Catches stuck states (§6.4) |
| Comms watchdog | Link health check each pass | Re-queue, then safe-hold | Catches silent radio faults |
| Power watchdog | SOC below floor and falling | Immediate load-shedding + alert | Protects the battery |
| Shore watchdog | No valid uplink for Tₛ | Vehicles continue last valid plan | Shore absence is survivable |

**Reset policy**

1. Reset counters are logged and reported (the event log must survive the reset).
2. Three resets in 24 h escalate severity to `critical` and force safe-hold.
3. A reset **never** clears the science data store (§10.1 store-and-forward integrity).

### 9.7 Flight-software quality practices

Firmware that cannot be reached by a human for a year is written to a different standard.

| Practice | Requirement |
|---|---|
| Static analysis | MISRA-inspired ruleset; zero critical warnings merged |
| Code size & RAM budgets | Per-module budgets tracked in CI; regression on exceedance |
| Watchdog-aware design | Every blocking call has a bound; every loop feeds a watchdog (§9.6) |
| Deterministic scheduling | Fixed-priority scheduling; no unbounded dynamic allocation in the OS layer |
| Fault injection | Every error path exercised by injection tests (chaos harness, §17.8) |
| Field update safety | A/B boot slots; update validated by checksum; automatic rollback on failed boot |
| Logging discipline | Logs are cheap, structured, layer-coded (§9.5) and survive resets |
| Review | Two-person review; the reviewer runs the HITL scenario linked to the change |

**"Never trust the last message" rule.** Firmware always assumes the last command may be
corrupted, stale or hostile: validate, version-check, bound, then execute (§4.5).

### 9.8 Debugger report template

The debugger app produces this report at every deployment; it is archived with the mission.

```text
PRE-DEPLOYMENT DEBUGGER REPORT
==============================
Mission            : CPO-2026-S1
Vehicle            : NCPOR-FLT-001 (Argo Float)
Engineer           : ____________________   Date/UTC : _______________
Location           : R/V ______________ ,  ____°S ____°E

 1. Sensors         [ PASS ]  notes: CTD factory check OK, O2 warm-up normal
 2. Power           [ PASS ]  SOC 100 %, charge input verified, solar OK
 3. GPS             [ PASS ]  fix in 38 s, HDOP 1.1
 4. Communications  [ PASS ]  sat test msg ACKed 14:22Z; short-range pair test OK
 5. Storage & clock [ PASS ]  1.9 GB free; RTC synced, drift < 0.5 s
 6. Actuators       [ PASS ]  pump self-test 5/5 cycles; bladder nominal
 7. Mission plan    [ PASS ]  config v3 checksum 8f3a… verified & reviewed

GATE RESULT        : ALL SYSTEMS READY — cleared for launch
Engineer signature : ____________________
```

**Rules.** Any FAIL blocks the gate (§9.4). Reports are immutable once signed and are uploaded
to the mission server before deployment. A vehicle without a signed report is not launched.

---

## 10. What We Measure: Data Streams and Products

Together the two vehicles cover two complementary halves of the environment: the **atmosphere and
sea surface** from the glider, and the **ocean interior** from the float.

### 10.1 The data streams

| Stream | Variables | Character & cadence | Primary use |
|---|---|---|---|
| **Atmospheric / surface (glider)** | Wind speed & direction, air temperature, pressure, humidity, solar radiation, waves, sea-surface temperature | Near-continuous time-series, minute-to-hourly samples, every day for the whole mission | Weather & climate records; forecasting the glider's own drift |
| **Ocean interior (float)** | Temperature, salinity, depth; optional dissolved oxygen, chlorophyll-a, backscatter, nitrate, pH | A full vertical profile (surface to ~2,000 m and back) every ~10 days — about 30–35 profiles per float per year | The classic Argo product: water-column structure and water-mass properties |
| **Vehicle telemetry (both)** | Position, battery, charging, link quality, mission state, faults | Regular health packets | Drives the dashboard, alerts and planning |
| **Mission metadata** | Checkpoints, plans, predictions, overrides, alerts | Every planning cycle and operator action | Auditability and prediction-skill diagnostics |

### 10.2 Representative data products

🖼️ **Figure 14 — Illustrative science and performance products** (synthetic curves, not
measurements):

<p align="center">
  <img src="assets/figures/fig_14_data_products.png" alt="Data products" width="95%"/>
</p>

| Panel | Product | Consumer |
|---|---|---|
| (a) | Temperature & salinity profile from one Argo ascent — cold fresh surface layer over warmer saltier deep water | Oceanographers, climatology |
| (b) | Glider wind and air-temperature excerpt including a storm passage | Meteorologists, flux studies |
| (c) | Predicted position uncertainty growing with forecast horizon (the basis of the surfacing-zone radius) | Planner, ML team |
| (d) | Float battery discharged across each cycle and restored at each rendezvous | Operators, engineering |

Additional products computed ashore:

- **Drift trajectories** for both vehicles (Lagrangian current observations).
- **Prediction-skill diagnostics** — coverage vs confidence, zone radius vs horizon.
- **Rendezvous performance reports** — success rate, timing offsets, energy accounting.
- **Ice-hazard logs** — detected, avoided, tracked (with optional tracker data).

### 10.3 Data lifecycle

```mermaid
flowchart LR
    ACQ["acquisition on board<br/>float profiles + glider series"] -->
    XFR["transfer at rendezvous<br/>(short-range) or burst"] -->
    RLY["satellite relay to<br/>shore gateway"] -->
    ING["ingest & decode on<br/>mission server"] -->
    QC["automated QC<br/>Argo-standard flags"] -->
    ARC["archive: netCDF + TSDB<br/>versioned · immutable"] -->
    PROD["derived products<br/>climatology · skill reports"] -->
    PUB["publish to dashboard /<br/>API / national archives"]
```

🖼️ **Figure 24 — Shore-side data pipeline:**

<p align="center">
  <img src="assets/figures/fig_24_data_pipeline.png" alt="Shore data pipeline" width="95%"/>
</p>

### 10.4 Formats, standards and QC

| Topic | Standard / convention |
|---|---|
| **Profile & trajectory format** | netCDF, CF conventions — compatible with the international Argo programme format |
| **QC flags** | Argo real-time QC convention (per-variable flags: good / probably good / probably bad / bad / missing) |
| **Units & vocabularies** | SI units; CF standard names; controlled vocabularies for instruments |
| **Timestamps** | UTC, ISO 8601, synchronised from GNSS time |
| **Identifiers** | WMO-style platform identifiers per vehicle; globally unique profile IDs |
| **Archives** | Deliverable to national ocean data centre and (by agreement) to global Argo GDAC |

> 📦 **Data philosophy.** Wherever possible the mission uses open, community-standard formats and
> quality-control conventions (the same standards used by the international Argo programme), so
> the data can flow directly into national and global ocean databases and be compared with
> decades of existing observations.

### 10.5 Example data record

A (trimmed) example of the float profile message as ingested by the mission server —
*illustrative structure*:

```json
{
  "platform_id": "NCPOR-FLT-001",
  "cycle_index": 17,
  "dive_time_utc": "2026-11-02T04:12:00Z",
  "surface_time_utc": "2026-11-12T03:58:00Z",
  "gps_fix": { "lat": -62.418, "lon": 34.207, "quality": 3 },
  "profile": {
    "pressure_dbar": [1.2, 2.1, 3.0, 4.1],
    "temperature_c": [-0.81, -0.79, -0.75, -0.70],
    "salinity_psu": [34.02, 34.03, 34.05, 34.08],
    "qc_flags": [1, 1, 1, 1]
  },
  "optional_biogeo": {
    "oxygen_umol_kg": [312.1, 312.4, 312.6, 312.8]
  },
  "battery_soc_pct": 74,
  "rendezvous_outcome": "success",
  "transfer_method": "glider_offload"
}
```

> The full schema lives in `data/schemas/` (see §17.1) and is versioned — the vehicles, server
> and dashboard must never drift apart (§18.6 contract tests).

### 10.6 Archive format example (netCDF / CF)

The float profile is archived as a netCDF file following CF conventions and Argo vocabulary —
*illustrative header and attributes*:

```text
dimensions:
    N_PROF = 1 ;
    N_LEVELS = 986 ;
variables:
    float PRES(N_LEVELS) ;
        PRES:long_name = "Sea water pressure" ;
        PRES:units = "decibar" ;
        PRES:_FillValue = 99999.f ;
        PRES:qc_flag = ... ;              // Argo real-time QC convention
    float TEMP(N_LEVELS) ;
        TEMP:long_name = "Sea temperature in-situ ITS-90" ;
        TEMP:units = "degree_Celsius" ;
        TEMP:standard_name = "sea_water_temperature" ;
    float PSAL(N_LEVELS) ;
        PSAL:long_name = "Practical salinity" ;
        PSAL:units = "psu" ;
        PSAL:standard_name = "sea_water_practical_salinity" ;
// global attributes
    :platform = "NCPOR-FLT-001" ;
    :cycle_number = 17 ;
    :date_surface = "2026-11-12T03:58:00Z" ;
    :project = "COOPERATIVE_POLAR_OCEAN_OBSERVATION" ;
    :data_mode = "R" ;                    // R = real-time, D = delayed-mode
    :history = "QC applied by mission server pipeline v1.4" ;
```

### 10.7 QC algorithm catalogue (shore, automated)

| # | Check | Rule (illustrative) | Flag on fail |
|---|---|---|---|
| QC-01 | Global range | T ∈ [−2.5, 40] °C · S ∈ [2, 42] psu · P ∈ [−2, 6500] dbar | 4 (bad) |
| QC-02 | Regional range | Climatological envelope ± 4σ for region/month | 4 |
| QC-03 | Spike | |xᵢ − median(window)| > threshold (depth-adaptive) | 3 or 4 |
| QC-04 | Gradient | Inversion of T/S beyond physical plausibility | 3 |
| QC-05 | Stuck sensor | Constant value over N consecutive levels | 4 |
| QC-06 | Pressure monotonicity | Non-monotonic P during ascent | 3 + re-grid |
| QC-07 | Surface reference | T near surface vs glider SST (co-located) | 3 on mismatch |
| QC-08 | Density stability | Unstable density inversions beyond tolerance | 3 |
| QC-09 | Inter-sensor consistency | O₂/chlorophyll vs T/S water-mass expectations | 3 |
| QC-10 | Cross-cycle drift | Slow sensor drift vs neighbours in time | 3 (delayed mode) |

- Real-time flags ship with the data; delayed-mode QC (expert review) refines flags for the
  final archive, per Argo practice.
- Every QC decision is logged with the rule ID, so any flag can be traced and audited.

### 10.8 Data volumes (illustrative)

| Stream | Volume | Yearly total |
|---|---|---|
| Float profiles (full resolution) | ~100 KB/cycle | ~3.5 MB/year |
| Float bursts (fallback) | ~2 KB/cycle | ≤ 0.1 MB/year |
| Glider met samples | ~150 B/min avg | ~80 MB/year (raw) |
| Telemetry & logs | ~5 KB/day | ~2 MB/year |
| **Total to archive (compressed)** | — | **< 100 MB/year** — trivially archivable |

### 10.9 Metadata and provenance

Every science product carries provenance so that any number can be traced to its instrument,
calibration and processing.

| Provenance field | Example | Source |
|---|---|---|
| `instrument_id` + `serial` | `CTD-NKE-0042` | Debugger record at deployment (§9.4) |
| `calibration_ref` | `CAL-2026-003 (lab intercomparison)` | Metrology plan (§5.8) |
| `platform_id`, `cycle_index`, `profile_id` | `NCPOR-FLT-001 / 17 / NCPOR-FLT-001-017` | Mission metadata |
| `qc_version` | `pipeline v1.4 · rules QC-01…QC-10` | Server pipeline (§10.7) |
| `data_mode` | `R` (real-time) → `D` (delayed-mode) | Archive lifecycle |
| `processing_history` | Ordered list of transformations with timestamps | Server pipeline |
| `retrieval_method` | `glider_offload` / `direct_burst` | Rendezvous log (§6.5) |
| `model_version` (if QC/downsampling used ML) | `qcmodel_v2` | Model registry (§7.9) |

> Provenance is what makes the data reusable by strangers a decade later — the international
> Argo programme's own data policy is built on exactly this discipline.

### 10.10 Data access levels

| Level | Who | What |
|---|---|---|
| L0 · Raw vehicle frames | Engineering (on-call) | As received, pre-decode — for diagnosis only |
| L1 · Real-time QC'd | All project staff | Profiles/series with automatic QC flags |
| L2 · Delayed-mode QC'd | Scientists + data centres | Final quality-controlled archive products |
| L3 · Derived products | Public (post-publication policy) | Climatology, skill reports, published figures |

**Rules.** L0 is never released outside the engineering team; L1 is the default internal working
level; L2 is the archive deliverable; L3 follows NCPOR's publication policy. Every export from
the dashboard records level, requester and timestamp.

---

### 10.11 Data latency and timing budget

"How long until the measurement is in the archive?" has a different answer on every path —
and the answer is a designed number, not an accident. The table and figure below are the
latency budget the ops team reviews each season (§10.10 ties each path to an access level).

| Path | At sea | Satellite pass | Shore ingest + QC | Archive publish | Total (illustrative) |
|---|---|---|---|---|---|
| Rendezvous offload (full profile) | ≤ 10 days (to next window) | ≤ 2 h (relay pass) | ≤ 1 h | ≤ 30 min | ≈ 10 days + 3.5 h |
| Satellite relay (compressed science) | ≤ 1 day | ≤ 2 h | ≤ 1 h | ≤ 30 min | ≈ 1 day + 3.5 h |
| Float direct burst (essentials) | ≤ 1 day | ≤ 6 h | ≤ 1 h | ≤ 30 min | ≈ 1 day + 7.5 h |
| Glider met log (store-and-forward) | ≤ 1 day | ≤ 2 h | ≤ 1 h | ≤ 30 min | ≈ 1 day + 3.5 h |

<p align="center">
  <img src="assets/figures/fig_28_data_latency_budget.png" alt="Data latency budget" width="95%"/>
</p>

*Figure 28 — each delivery path's latency budget, ocean to NCPOR archive. The rendezvous path is
slow by design (it waits for the next meeting); the burst and relay paths are fast by design
(they exist for exactly this reason).*

<p align="center">
  <img src="assets/images/satellite_ground_station.jpg" alt="Large satellite ground-station antenna" width="60%"/>
</p>

*A large satellite ground-station antenna of the class that would terminate the polar
constellation downlink at the shore gateway (§4.3). The shore segment of the latency budget —
ingest, QC, publish — is the part the team controls directly, so it is engineered to be minutes,
never days.*

**Timing budget across one cycle** (the cadence the whole system marches to, §6.7):

| Event | When | Deadline notes |
|---|---|---|
| Float surfacing window opens | T0 | ±2 h uncertainty from §7.15 zone |
| Full offload complete (nominal) | T0 + 20 min | "Data first" rule (§6.3) |
| Relay pass for the dataset | next scheduled pass (≤ 6 h) | M06 retransmits until ACK (§4.7) |
| Real-time QC on the server | ingest + 1 h | Flags visible on the dashboard |
| Delayed-mode QC | + 30 days | After cross-calibration inputs (§10.7) |
| Archive version stamp (`R` → `D`) | + 60 days | Final publish to data centre |

> ⏱️ **Design rule.** Real-time data are *fast and provisional*; delayed-mode data are *slow and
> authoritative*. No consumer is ever left unsure which one they are looking at — the
> `data_mode` flag (§10.9) rides on every record.

## 11. The Web Mission Dashboard

NCPOR scientists and operators interact with the mission through a **secure web platform**: a
live digital twin of what is happening in the Southern Ocean. It is both a scientific viewer and
the control surface from which autonomous behaviour is supervised and, when necessary,
overridden.

🖼️ **Figure 21 — Mission dashboard wireframe** — map, tracks, predictions, side panels:

<p align="center">
  <img src="assets/figures/fig_21_dashboard.png" alt="Dashboard wireframe" width="95%"/>
</p>

### 11.1 What the dashboard shows

| View | Contents |
|---|---|
| **Live positions & tracks** | Both vehicles, with full historical mission tracks that can be replayed |
| **Predictions** | Glider predicted trajectories & reachable regions; float surfacing zone (uncertainty ellipse + confidence) |
| **Plan** | Checkpoints and the planned rendezvous, with target time and current confidence of success |
| **Ocean & atmosphere** | Recent profiles, weather time-series, currents and forecast overlays |
| **Ice** | Sea-ice fields and iceberg/tracker hazard polygons |
| **System health** | Battery levels & charging, communication status, storage, sensor health, mission state per vehicle |
| **Rendezvous status** | Upcoming window, distance, estimated time, whether the glider is on track |
| **Alerts & event log** | Searchable, time-stamped history of every action and fault |

### 11.2 Alerts, override and the human-in-the-loop

The platform raises automatic emergency alerts for defined conditions — ice hazard on the route,
low battery, loss of contact beyond a threshold, sensor or actuator fault, or a rendezvous at
risk. Each alert shows the **affected vehicle, the evidence and the system's proposed response**.

```mermaid
flowchart TB
    COND["condition detected<br/>(ice on route · low battery · contact lost<br/>sensor/actuator fault · rendezvous at risk)"] -->
    ALERT["alert raised<br/>vehicle + evidence + proposed response"] -->
    OPS{"operator decision"}
    OPS -- "1 · accept" --> AUTO["autonomous response<br/>(default in routine ops)"]
    OPS -- "2 · adjust" --> ADJ["modify plan: checkpoints,<br/>rendezvous zone, cycle, loiter"]
    OPS -- "3 · override" --> MAN["manual control of glider<br/>heading/steering within limits"]
    AUTO --> Q["queue approved commands"]
    ADJ --> Q
    MAN --> Q
    Q --> UL["uplink via satellite"] --> EX["vehicle executes"] --> LOG["execution confirmed & logged<br/>auditable record"]
```

The system therefore supports the full spectrum from **lights-out autonomy to direct human
driving**, and keeps an auditable record of who or what made each decision.

> 🎨 **Designing for scientists, not just engineers.** The dashboard separates *mission control*
> (positions, plans, health, alerts) from *data views* (profiles, time-series, maps and exports),
> so oceanographers can focus on the science while operators handle the vehicles. Role-based
> access, map-based situational awareness and clear colour-coded statuses make the system usable
> during a high-workload storm event.

### 11.3 Roles and access control

| Role | Can view | Can command |
|---|---|---|
| **Operator** | Everything | Checkpoints, rendezvous parameters, overrides, acknowledge alerts |
| **Scientist** | Data views, profiles, exports, predictions | Sampling priorities, operating regions (via approved requests) |
| **Engineer (ashore)** | Everything + logs + model performance | Software updates (change-managed), model deploys |
| **Reviewer / Guest** | Read-only mission view | Nothing |
| **Administrator** | Everything + audit trail + user management | Role assignment, system configuration |

### 11.4 Dashboard technology and interfaces

| Aspect | Specification |
|---|---|
| **Architecture** | Single-page web app over the mission-server API; WebSocket push for live telemetry |
| **Map layer** | MapLibre-class web map with polar projection support; WMS/WMTS overlays for ice and currents |
| **AuthN/AuthZ** | OIDC login, role-based access control, full audit log of views and commands |
| **Alerting** | In-app + email/SMS escalation for critical alerts |
| **Data access** | REST API + export (netCDF/CSV) for scientists |
| **Resilience** | Works in degraded mode from the last cached state during satellite outages |

### 11.5 API reference (mission server)

| Method | Endpoint | Purpose | Role |
|---|---|---|---|
| `GET` | `/api/v1/mission` | Mission summary: vehicles, cycle, next rendezvous | all |
| `GET` | `/api/v1/vehicles/{id}` | Vehicle state, health, last contact | all |
| `GET` | `/api/v1/vehicles/{id}/track?from=&to=` | Historical track (GeoJSON) | all |
| `GET` | `/api/v1/predictions?type=surfacing` | Latest surfacing zone + confidence | all |
| `GET` | `/api/v1/predictions?type=trajectory` | Glider predicted path + reachable set | all |
| `GET` | `/api/v1/plan` | Current checkpoint plan + objective | all |
| `GET` | `/api/v1/profiles?cycle=` | Archived profiles (netCDF/JSON) | scientist+ |
| `GET` | `/api/v1/timeseries?stream=met` | Glider meteorological series | scientist+ |
| `GET` | `/api/v1/hazards` | Ice polygons + tracker positions | all |
| `GET` | `/api/v1/alerts?state=open` | Open alerts with evidence | operator+ |
| `POST` | `/api/v1/alerts/{id}/ack` | Acknowledge alert | operator |
| `POST` | `/api/v1/plan/checkpoints` | Upload revised checkpoints (uplink) | operator |
| `POST` | `/api/v1/plan/rendezvous` | Adjust rendezvous zone / window | operator |
| `POST` | `/api/v1/override` | Manual steering/heading override | operator |
| `POST` | `/api/v1/commands` | Generic validated command (uplink) | operator |
| `GET` | `/api/v1/audit?from=&to=` | Decision audit trail | admin |
| `WS` | `/ws/live` | Live telemetry, predictions, alerts push | all |

Rules: every mutating call requires role ≥ operator, is validated against the plan schema, and
is recorded in the audit trail with the calling identity — the same audit trail the vehicles'
logs are merged into (§9.5).

### 11.6 Data model

The shore database models the mission as versioned entities — nothing is ever updated in place.

```mermaid
erDiagram
    MISSIONS ||--o{ VEHICLES : operates
    MISSIONS ||--o{ CYCLES : contains
    VEHICLES ||--o{ TELEMETRY_PACKETS : emits
    VEHICLES ||--o{ EVENT_LOGS : writes
    CYCLES ||--o| PROFILES : yields
    PROFILES ||--o{ PROFILE_LEVELS : has
    CYCLES ||--o| RENDEZVOUS_EVENTS : includes
    RENDEZVOUS_EVENTS }o--|| VEHICLES : involves
    PLANS ||--o{ CHECKPOINTS : lists
    PLANS }o--|| MISSIONS : generated_for
    HAZARDS ||--o{ HAZARD_POSITIONS : tracks
    ALERTS }o--|| VEHICLES : raised_for
    ALERTS }o--|| OPERATORS : handled_by
    COMMANDS }o--|| OPERATORS : issued_by
    COMMANDS }o--|| VEHICLES : executed_by
    PREDICTIONS }o--|| CYCLES : forecast_for
```

```mermaid
classDiagram
    class Mission {
        +UUID id
        +String name
        +DateTime start_utc
        +DateTime end_utc
        +String region_polygon
        +String status
    }
    class Vehicle {
        +UUID id
        +String type
        +String callsign
        +Float battery_soc_pct
        +String mission_state
        +DateTime last_contact_utc
    }
    class Cycle {
        +Int index
        +DateTime dive_time_utc
        +DateTime surface_time_utc
        +String outcome
    }
    class Profile {
        +Int cycle_index
        +Float[] pressure_dbar
        +Float[] temperature_c
        +Float[] salinity_psu
        +Int[] qc_flags
    }
    class RendezvousEvent {
        +Int cycle_index
        +String outcome
        +Float wh_delivered
        +Float soc_after_pct
    }
    class Prediction {
        +String model_version
        +Float confidence
        +Float radius_km
        +String geometry
    }
    class Plan {
        +String plan_id
        +Float p_success
        +String status
    }
    class Checkpoint {
        +Int index
        +Float lat
        +Float lon
        +String type
        +DateTime eta_utc
    }
    class Alert {
        +String severity
        +String condition
        +String evidence
        +String proposed_response
        +String state
    }
    class Command {
        +String cmd_type
        +String payload
        +String issued_by
        +String ack_status
    }
    Mission "1" --> "1..2" Vehicle
    Mission "1" --> "36" Cycle
    Cycle "1" --> "0..1" Profile
    Cycle "1" --> "0..1" RendezvousEvent
    Cycle "1" --> "0..*" Prediction
    Mission "1" --> "0..*" Plan
    Plan "1" --> "1..*" Checkpoint
    Vehicle "1" --> "0..*" Alert
    Vehicle "1" --> "0..*" Command

```

### 11.7 Dashboard UX specifications

The dashboard is used during storms, at night, by tired people. UX requirements are
requirements.

| Area | Specification |
|---|---|
| **Glanceability** | Core status (both vehicles, next rendezvous, battery, alerts) visible without interaction; colour-coded green/amber/red with text labels (never colour alone) |
| **Map defaults** | Polar projection; layer defaults: tracks on, zones on, ice on, currents off (togglable) |
| **Alert design** | Alert = vehicle + evidence + proposed response; one-click accept; adjust/override paths within 3 clicks; every action confirmable and undoable before uplink |
| **Degraded mode** | Clear "LAST CONTACT +14 h" banners; cached views; no commands attempted without a fresh link |
| **Time handling** | All times UTC with local-time toggle; relative ages ("3 h ago") alongside absolute stamps |
| **Data views** | Profile viewer with QC flags per level; hover values; export buttons (netCDF/CSV) |
| **Replay** | Historical track replay with speed control; prediction artefacts overlayable at any past instant |
| **Accessibility** | Keyboard-accessible command paths; screen-reader labels on all status indicators |
| **Latency budgets** | Live telemetry ≤ 2 s from server push; map interactions ≤ 100 ms feedback |
| **Mobile** | Read-only "pocket view" for operators on call: positions, battery, alerts |

**Usability acceptance tests (run at P3 gate)**

1. A new operator, given the §13.5 playbook, completes every contingency response in under
   3 minutes per alert.
2. A scientist locates and exports cycle 17's salinity profile in under 1 minute.
3. Under a simulated satellite outage, the operator correctly states what the system will do
   next (degraded-mode banner test).
4. Colour-blind users can distinguish all statuses (colour + symbol test).

### 11.8 Dashboard non-functional requirements

| NFR | Target |
|---|---|
| Availability during ops season | ≥ 99.5 % |
| Concurrent users | ≥ 25 without degradation |
| Map refresh rate (live mode) | ≤ 2 s |
| Time to first meaningful paint | ≤ 3 s on a 5 Mbps link |
| Browser support | Last 2 major versions of Chrome/Firefox/Edge |
| Data retention (audit + events) | Full mission + 5 years |

### 11.9 Dashboard accessibility and internationalisation

The dashboard is used by scientists, operators and reviewers — often under stress, sometimes at
sea with poor displays. Accessibility is treated as an operational requirement, not a courtesy.

| Aspect | Requirement |
|---|---|
| Colour | No colour-only status encoding; every alert state carries a text label and icon |
| Contrast | WCAG AA minimum for all text; tested against the alert palette (§11.2) |
| Keyboard | Every operational action reachable and executable by keyboard alone |
| Screen readers | ARIA labels on all live regions; alert announcements routed to the aural UI |
| Text size | Base 14 px, resizable to 200 % without layout breakage |
| Dark mode | Supported (ops rooms run dark); both themes tested in CI |
| Offline/bandwidth | Progressive rendering; the dashboard must stay readable on a 1 Mbps ship link |
| Language | English UI; message catalogues prepared so localisation is a translation task, not a rework |
| Time zones | All times displayed in UTC with a user-timezone toggle; every log entry stored in UTC |
| Redundancy of format | Numbers shown with units and spelled-out thresholds on hover (e.g. "SOC 38 % — below winter floor 55 %") |

**Design rule.** If an operator cannot tell the mission's state correctly in the first ten seconds
of looking at the dashboard — in daylight, in a storm, on a laptop — the design is wrong, whatever
the specifications say (§11.8).

### 11.10 Dashboard smoke-test suite

A fast, scripted check-list run before every release (§18.12), in addition to the automated tests:

| # | Smoke test | Pass criterion |
|---|---|---|
| 1 | Log in as each role (§11.3) | Role sees exactly its permitted views and commands |
| 2 | Inject a synthetic alert | Alert appears within 2 s, announces in the aural UI, escalates correctly |
| 3 | Kill the WebSocket feed | Dashboard degrades to cached state with a visible staleness banner |
| 4 | Replay a real rendezvous from the simulator log | Timeline, map and scorecard (§6.8) match the recorded values |
| 5 | Export a profile as netCDF and CSV | Files open and match the displayed numbers |
| 6 | Override a checkpoint with a reviewer account | Command is rejected with an audit entry |
| 7 | Switch to dark mode at 200 % text | No clipped controls, contrast passes |
| 8 | Disconnect and reconnect a satellite feed mid-pass | No duplicated or lost frames in the audit log |
| 9 | Load a 30-day window of full-resolution data | Map and charts stay responsive |
| 10 | Simulate a lost vehicle | Dashboard shows the alert ladder, contact-age timer and §13.9 checklist entry points |

> The smoke suite is deliberately manual: it exercises *perception* — the part of the interface
> that automated tests cannot see.

---

### 11.11 Dashboard screen-by-screen walkthrough

Five screens cover the whole mission, plus one modal that can interrupt any of them. This
walkthrough is the operator-training syllabus in miniature (§14.7) and the checklist the smoke
suite (§11.10) verifies after every release.

<p align="center">
  <img src="assets/figures/fig_29_dashboard_flow.png" alt="Dashboard screen layout and flow" width="95%"/>
</p>

*Figure 29 — the dashboard layout (top) and the screen-to-screen flow (bottom). One glance at the
live map answers "where is everything and is anything wrong" — the question every screen exists
to answer (§11.1).*

```mermaid
stateDiagram-v2
    [*] --> LiveMap
    LiveMap --> Vehicle: click vehicle marker
    LiveMap --> Prediction: click zone
    LiveMap --> Planner: checkpoint review due
    LiveMap --> DataLab: export request
    Vehicle --> LiveMap: back
    Vehicle --> Planner: inspect plan for this vehicle
    Prediction --> LiveMap: back
    Prediction --> Planner: widen/accept zone
    Planner --> LiveMap: plan uplinked
    DataLab --> LiveMap: back
    LiveMap --> Alert: any L2+ alert fires
    Vehicle --> Alert: health anomaly
    Prediction --> Alert: contract breach detected
    Planner --> Alert: override needs operator ack
    DataLab --> Alert: QC batch failure
    Alert --> LiveMap: acknowledge + return
    Alert --> Vehicle: acknowledge + inspect vehicle
```

| Screen | Primary user | Answers the question | Key elements |
|---|---|---|---|
| **Live map** | Operator | Where is everything? | Vehicles, ice polygons, zones, checkpoints, alert banner, ≤ 2 s live refresh (§11.8) |
| **Vehicle** | Operator / engineer | Is this vehicle healthy? | SOC, position, last-pass age, sensor health, log tail, firmware version |
| **Prediction** | ML lead / operator | Where will the float surface, and how sure are we? | Zone + spread plot, coverage curve (§7.11), ensemble members, freshness stamps |
| **Planner** | Operator | What is the plan, and what would change it? | Checkpoint list, cost-function summary, hazard updates, accept/override actions |
| **Data lab** | Scientist | What did we measure, and can I trust it? | Profiles, met series, QC flags, exports (netCDF/CSV), data-mode badges |
| **Alert modal** | Whoever is on call | What just broke, and what do I do? | Level, contact-age timer, suggested response (§13.9), ack/reassign buttons |

**The 60-second operator glance** (drilled in training):

1. **Alert banner** — anything L2+? If yes, the mission is now about that.
2. **Two SOC numbers** — glider and float, both above their seasonal floors (§8.12)?
3. **Next surfacing window** — when is the next rendezvous, and is the glider on track (§6.8)?
4. **Ice** — has the ice chart moved anything since the last plan (§8.3)?
5. **Last pass age** — is the contact plan holding (§13.7), or is it time for plan B?

> 👁️ **Design rule.** The dashboard's success metric is not feature count — it is *time to
> correct understanding*. Every element on every screen is there to shorten it (§11.7), and the
> smoke suite exists to catch anything that lengthens it.

## 12. Optional Extension: The Iceberg Tracker

As an optional extension, NCPOR can integrate a small, autonomous iceberg/sea-ice tracker that is
physically deployed **onto an iceberg or ice floe**. It turns a moving hazard into a live, tracked
object that feeds the planner directly — rather than merely drawing ice on a map.

### 12.1 What it is and what it does

| Aspect | Specification |
|---|---|
| **Hardware** | Ruggedised, low-power beacon package: GPS/GNSS positioning, satellite modem, batteries (often with small solar assistance), hardened ice-rated enclosure |
| **Deployment** | Attached to or dropped onto a target iceberg or floe from a vessel, aircraft or the surface platform |
| **Reporting** | Position (and optionally temperature and motion) on a set schedule |
| **Product** | Each position becomes a **tracked hazard with a predicted drift path and exclusion zone** |

📷 **Plate 12.1 — Large tabular icebergs drift for years** along predictable-but-evolving paths.
A tracker mounted on such an iceberg provides a continuously updated hazard position for the
planner and scientists alike. *(Credited in §20.5.)*

<p align="center">
  <img src="assets/images/iceberg_a23a_2.jpg" alt="Tabular iceberg for tracker deployment" width="70%"/>
</p>

🖼️ **Figure 22 — From iceberg to planner input:**

<p align="center">
  <img src="assets/figures/fig_22_tracker.png" alt="Iceberg tracker data flow" width="90%"/>
</p>

### 12.2 Data flow into the planner

```mermaid
flowchart LR
    TRK["tracker on iceberg<br/>GPS/GNSS + sat modem"] -->
    SAT["satellite relay"] -->
    SH["shore: track + drift model"] -->
    POLY["hazard polygon<br/>+ predicted drift path"] -->
    PLN["planning engine<br/>no-go zones updated"] -->
    GL["glider re-routes<br/>before the hazard arrives"]
    POLY -. "also served to dashboard" .-> DASH["dashboard map"]
    SH -. "iceberg drift records<br/>for science" .-> SCI["science archive"]
```

> 🔁 **From visualisation to autonomy.** The key point is that the tracker data are *operational*,
> not cosmetic. A freshly reported iceberg position automatically reshapes the no-go polygons
> used by the planning engine, so the glider reroutes **before** the hazard ever reaches its
> path — the same data also produces scientifically valuable iceberg-drift records.

### 12.3 Deployment methods and scientific side-benefits

| Deployment method | Notes |
|---|---|
| From the research vessel (during the seasonal deployment cruise) | Simplest; requires being in range of the target berg |
| From aircraft (fly-over drop) | Reaches bergs beyond ship range |
| From the Wave Glider (where feasible) | Autonomous, opportunistic deployment |

| Side-benefit | Value |
|---|---|
| Iceberg drift records | Long, continuous Lagrangian observations of tabular iceberg motion |
| Hazard climatology | Improves future ice-avoidance models and planning margins |
| Validation | Ground truth for iceberg drift forecasts |

### 12.4 Tracker engineering and link budget (optional extension)

| Parameter | Value (illustrative) | Notes |
|---|---|---|
| Positioning | GPS/GNSS, cold-start < 60 s | Ephemeris caching for polar geometry |
| Reporting | 6-hourly standard; 15-min hazard-alert mode | Schedule configurable over satellite |
| Power | Solar-assisted battery; ≥ 12 months autonomy | Sized for polar winter |
| Enclosure | Ice-rated, crush- and melt-tolerant; anchor/ablation spike | Survives rollover and melt-out |
| Comms | Satellite modem, store-and-forward | Same constellation as mission (§4.3) |
| Payload (optional) | Air temperature, tilt/motion, barometer | Adds science value at negligible cost |

**Link budget sketch (per report):**

| Item | Value |
|---|---|
| Position fix | ~30 s GNSS on, ~10 mW·s |
| Report message | ~200 B, one satellite burst |
| Report energy | ~2 J total (fix + burst) |
| Daily budget (6-hourly) | ~8 J ≈ 3 mW average |
| Solar panel (small) | ≥ 20 mW average in summer — comfortable margin; battery bridges winter |

> The tracker's most important engineering requirement is **operational reliability at the
> planner interface**: a stale tracker is *worse than no tracker* if it makes the planner trust
> an outdated position. Every tracker report therefore carries a timestamp, a fix quality, and a
> staleness TTL; polygons older than their TTL are widened automatically (§8.7).

### 12.5 Tracker operations (optional extension)

| Task | Cadence | Owner |
|---|---|---|
| Battery & health review | Each report | Dashboard automation |
| Drift-path prediction refresh | 6-hourly | Shore tracker service |
| Polygon TTL enforcement | Continuous | Planning engine (§12.4) |
| Melt-out / loss detection | On report gap > 2× schedule | Ops alert |
| Science archive of drift record | Daily | Server pipeline |
| Tracker recovery (if possible) | At season closeout | Field team |

> A tracker that goes silent is automatically widowed: its last polygon grows and its TTL
> expires, then it reverts to chart-based handling — the planner never freezes on a dead asset.

---

## 13. Deployment and a Year in the Field

### 13.1 How the devices get into the water

Deployment is the one part of the mission that requires a ship, and it deliberately needs to
happen only **once per field season** (with recovery/service as an option at season's end).

```mermaid
flowchart LR
    SHIP["research vessel<br/>on station"] -->
    CHECK["hand-held debugger<br/>full checklist, both vehicles"] -->
    GATE{"all systems ready?"}
    GATE -- "no" --> FIX["fix on deck"] --> CHECK
    GATE -- "yes" --> LAUNCH["crane launch: float + glider"] -->
    AUTO["immediate autonomous<br/>operation begins"] -->
    DEPART["ship departs — system<br/>runs without ship present"]
```

📷 **Plate 13.1 — A polar research vessel operating in ice.** Such an expedition is used to
deploy the devices; after that the vehicles operate without the ship present. *(Representative
photograph, credited in §20.5.)*

<p align="center">
  <img src="assets/images/polar_vessel.jpg" alt="Polar research vessel in ice" width="80%"/>
</p>

📷 **Plate 13.2 — A profiling float being readied and craned over the side** of a research
vessel. Deployment is quick and routine once the on-board health checks pass. *(Representative
photograph, credited in §20.5.)*

<p align="center">
  <img src="assets/images/argo_float_deployment_2.jpg" alt="Float crane deployment" width="80%"/>
</p>

### 13.2 The year ahead

Once deployed, the glider remains on station continuously while the float repeats roughly 30–35
ten-day cycles over a year. Each cycle ends in a rendezvous, data delivery and recharge. The exact
operating window and ice limits are set by NCPOR around sea-ice conditions; the system
automatically re-plans or holds as ice advances.

🖼️ **Figure 15 — One year of autonomous operation (illustrative plan):**

<p align="center">
  <img src="assets/figures/fig_15_year_plan.png" alt="Year-in-the-field plan" width="95%"/>
</p>

| Season | Conditions (illustrative) | System behaviour |
|---|---|---|
| **Late spring / early summer** | Long daylight, retreating ice | Deployment; commissioning cycles; highest solar yield |
| **Summer–autumn** | Open water, storms | Continuous station-keeping; ~1 rendezvous per 10 days; storm-riding |
| **Winter** | Expanding sea ice, long darkness | Re-plan or hold as ice advances; load-shedding; reduced transmission cadence |
| **Following spring** | Ice retreat, daylight returns | Optional recovery/servicing; or mission continuation |

### 13.3 Seasonal operations logic

```mermaid
flowchart TB
    DAY{"daylight & ice state?"}
    DAY -- "summer · open water" --> FULL["full operations:<br/>normal cycles + rendezvous"]
    DAY -- "autumn · advancing ice" --> CAUT["ice-aware mode:<br/>re-plan / hold as ice advances<br/>float surfacing logic ice-informed"]
    DAY -- "winter · dark · ice" --> RED["reduced mode:<br/>load-shedding · fewer transmissions<br/>data stored on board"]
    RED --> SPR["spring: recovery / servicing<br/>or continuation"]
    FULL --> CAUT --> RED
```

### 13.4 Roles and responsibilities

| Role | Responsibilities | Location |
|---|---|---|
| **On-site engineer** | Pre-deployment assembly and hand-held debugger verification on the vessel; deployment (and recovery); first-line hardware support | Research vessel (seasonal) |
| **Mission operators** | Monitor the dashboard, respond to alerts, approve autonomous plans or issue overrides, manage checkpoints and mission configuration | NCPOR mission room |
| **NCPOR scientists** | Use the ocean and atmospheric data; set sampling priorities and operating regions; review data quality and prediction performance | NCPOR / remote |
| **Engineering / ML team (ashore)** | Maintain the mission server and models, retrain ML with new data, manage software updates, analyse fault logs, plan servicing | NCPOR / partner labs |
| **The vehicles themselves** | Execute the mission autonomously: profile, predict, navigate checkpoints, rendezvous, offload/recharge, avoid ice, monitor health, report | Southern Ocean |

### 13.5 Operational contingencies playbook

| Event | Operator action | System action (already automatic) |
|---|---|---|
| Alert: ice within stand-off | Review proposed re-route; accept or adjust | Planner re-routes or holds |
| Alert: glider battery low | Verify load-shedding engaged; consider reducing transmission cadence | Priority load-shedding |
| Contact lost > threshold | Confirm via alternate data path (float burst); wait for scheduled window | Vehicles run last validated plan; store-and-forward |
| Rendezvous at risk | Choose: extend surface window / accept satellite fallback / retry next cycle | Planner re-optimises; fallback chain (§6.5) |
| Sensor fault | Decide graceful degradation vs servicing at season end | Fault isolated; remaining sensors continue |
| Major storm forecast | Approve loiter/hold pattern away from hazards | Planner biases checkpoints with sea state |

### 13.6 Deployment runbook (on-vessel)

| Step | Who | Action | Done when |
|---|---|---|---|
| 1 | Engineer | Unpack and visually inspect both vehicles (hull, seals, antennas, fins, bladder) | No visible damage; photos logged |
| 2 | Engineer | Power on glider; run debugger **power + sensors** checks (§9.4) | All pass entries logged |
| 3 | Engineer | Power on float; run debugger **sensors + actuators** checks (pump self-test) | Pump cycles; bladder confirmed |
| 4 | Engineer | Pairwise comms test: float ↔ glider short-range link on deck (2 m separation) | Handshake + test transfer OK |
| 5 | Engineer | Satellite registration test: glider and float modems pass a test message to shore | Shore confirms receipt both IDs |
| 6 | Engineer | Clock sync: both vehicles synchronised to GNSS time | Offset < 1 s, logged |
| 7 | Engineer | Mission plan load: cycle parameters, checkpoints, safe-hold configs | Checksums verified, reviewed |
| 8 | Engineer | **Gate: ALL SYSTEMS READY** — sign off in the debugger app | Signed record stored in app + shore |
| 9 | Deck crew | Crane deployment: float first, then glider, ~200 m apart | Both report GPS fix + first telemetry |
| 10 | Operator (ashore) | Confirm both vehicles on dashboard; planner produces first plan | Dashboard state = OPERATIONAL |
| 11 | Captain | Ship departs station | Vehicles on their own |

> ⚠️ The debugger record from step 8 is the mission's *birth certificate* — it is the auditable
> proof that deployment was a signed-off quality gate, not an act of faith.

### 13.7 Contact plan and communications schedule (illustrative)

| Pass | Local time window | Content down | Content up | Budget |
|---|---|---|---|---|
| A (morning) | 06:00–07:30 | Overnight met data, telemetry, logs | Checkpoints, time sync | ~40 Wh |
| B (midday) | 12:30–14:00 | Telemetry; ACKs; alerts | Plan updates, configs | ~40 Wh |
| C (evening) | 21:00–22:30 | Relay of rendezvous data if held; health summary | Checkpoint deltas | ~40 Wh |
| D (rendezvous) | On event | Full profile relay + health burst | Rendezvous parameters | ~60 Wh |

- Winter: passes A–C reduce to **one pass/day**; rendezvous pass D is never skipped.
- Every pass ends with a **queue-drain check**: pending bytes and estimated drain time logged;
  if the queue grows beyond the daily budget, the planner sheds non-essential traffic (§8.4).
- Float direct bursts use their own minimal pass plan, only on surfacing windows.

### 13.8 Season closeout checklist

Run at season end — whether the mission continues or is recovered.

| # | Task | Owner | Evidence |
|---|---|---|---|
| 1 | Confirm all profiles archived and QC'd | Science | Archive completeness report |
| 2 | Pull complete event logs from both vehicles | Engineering | Logs merged on shore (§9.5) |
| 3 | Freeze the season dataset version | Engineering | Tagged release of `data/` |
| 4 | Retrain models with the season's labelled cycles | ML | Validation report (§7.5) |
| 5 | Compile prediction-skill report | ML + Science | Coverage/radius vs advertised |
| 6 | Compile rendezvous scorecard (§6.8) | Ops | Scorecard + tuning decisions |
| 7 | Energy audit: solar yield vs model, SOC history | Engineering | Fig. 18-style comparison |
| 8 | RAM review vs targets (§5.7) | Engineering | Fault catalogue update |
| 9 | Decide: continue / recover / service | Programme board | Decision record |
| 10 | If continuing: recharge float to full, clear storage, update software via validated uplink | Engineering | Signed change record |
| 11 | Publish season report + archive deliverables | All | Report + data centre receipts |
| 12 | Update this README's illustrative numbers with measured ones | Engineering | Versioned diff (Appendix E) |

### 13.9 Emergency procedures

Emergencies are rare by design, but the responses are scripted so nobody improvises.

**E-1 · Vehicle contact lost**

1. Confirm via the alternate path (glider silent → check float bursts; both silent → satellite
   provider status).
2. Mark expected behaviour on the dashboard: vehicles run their last validated plan (§4.4).
3. Escalate at 48 h (L3 §15.3); convene engineering + ML to model likely states.
4. On contact return: pull logs first, assess, then command.

**E-2 · Ice stand-off intrusion (predicted or actual)**

1. Alert fires automatically; planner holds the glider.
2. Operator verifies the hazard source and freshness; accepts hold or adjusts.
3. Engineering reviews why detection/prediction lagged; stand-off policy revisited (§8.7).
4. Float surfacing logic set to ice-cautious until the hazard clears.

**E-3 · Battery critical on either vehicle**

1. Load-shedding engages automatically (§8.4); all non-essential traffic stops.
2. Operator confirms the shedding ladder and reduces the contact plan (§13.7).
3. Float: skip the next deep profile (surface early) if SOC < floor.
4. Recovery or solar-recovery plan agreed at the next engineering sync.

**E-4 · Unauthorised or malformed command detected**

1. Command rejected by the vehicle (§4.5); alert raised with the rejected payload hash.
2. Security review: key status, source, scope of exposure.
3. Key rotation if indicated; full audit preserved.

**E-5 · Docking incident (pair coupled but unable to release)**

1. Both controllers run the abort sequence independently (§6.6).
2. If still coupled: command both to safe-hold; surface the situation at L3.
3. Engineering runs the failure scenario in the simulator before any further rendezvous.

**Drill schedule.** E-1 and E-3 are drilled quarterly on the simulator with operators; E-5 is
drilled with the hardware bench at every phase gate.

### 13.10 Winter operations runbook

Winter is the mission's hardest quarter. The rules below are the operating doctrine from
April to September (illustrative dates, Southern Hemisphere).

**Standing orders**

1. **One pass per day**, at the highest-elevation window (§13.7 winter plan).
2. **Load-shedding ladder pre-approved** — operators do not approve each step, they monitor it
   (§8.4).
3. **Ice charts daily**; the planner runs in conservative mode (widened stand-offs §8.7).
4. **No rendezvous is forced** — a missed window in winter is routine; the burst path carries
   the essentials (§6.5).
5. **Glider rides the ice edge**, never the interior: box boundaries re-planned to the MIZ
   position each week.

**Winter-only thresholds**

| Signal | Summer limit | Winter limit |
|---|---|---|
| Glider SOC floor (shed more) | 40 % | 55 % |
| Passes/day | 3 | 1 |
| Met sampling interval | 1 min | 10 min |
| Rendezvous attempt sea-state gate (`SELL`) | 2.5 m Hₛ | 2.0 m Hₛ (less capable seas) |
| Alert contact-age | 24 h | 72 h (fewer passes, longer silence is normal) |

**Winter exit.** When 10-day mean solar yield exceeds the summer threshold for 14 consecutive
days, winter orders lift automatically and the dashboard announces "WINTER MODE ENDED".

### 13.11 Ship-visit choreography and handover

Once per season, a ship visits the operating area (§13 assumptions A-08). The visit is planned
months ahead and choreographed like a rendezvous — with the ship as the third vehicle.

| Step | Action | Lead |
|---|---|---|
| V-1 (D−30) | Visit window agreed with the ship's cruise plan; operating box steered near the track | Ops + ship PI |
| V-2 (D−7) | Vehicles commanded toward the meeting box; float cycle timed to surface inside the window | Ops |
| V-3 (D−2) | Weather, ice and vehicle-state review; go/no-go call | Ops + captain |
| V-4 (D−1) | Final box broadcast; recovery gear staged; team briefed | Field team |
| V-5 (visit day) | Small-boat ops for glider recovery (weather permitting); float recalled and recovered separately if needed | Field team |
| V-6 (same day) | On-deck checks: data verified (§9.4 debugger against the real vehicles), batteries topped, sensors cleaned | Field team |
| V-7 (D+1) | Redeploy or pack for return; debrief; report uploaded to the mission server | Field team + ops |

**Rules**

1. The ship visit is the **only** sanctioned time for hands-on vehicle work at sea — everything
   else happens through the dashboard.
2. A missed ship visit must never strand the mission: every recovery step has a "leave it and
   continue" alternative (the vehicles can simply keep working).
3. Ship time is expensive: the visit checklist is rehearsed on the simulator the week before,
   and every tool on the deck list has an owner and a spare.

### 13.12 Season wrap-up report template

Every season ends with one report, written against this fixed outline (mirrors §13.8 closeout):

```text
SEASON WRAP-UP REPORT — [SEASON]
================================
1.  Mission summary          vehicles, dates, operating box, headline numbers
2.  Science delivery         profiles, met-hours, QC levels, notable events (Q1–Q5 vs §2.5)
3.  Rendezvous record        attempts, successes, scorecard trends (§6.8), energy ledgers
4.  Prediction skill         coverage vs confidence curves, drift errors, model changes (§7.10)
5.  Incidents and lessons    E-procedures used (§13.9), LL register summary (§15.5)
6.  Risk register close-out  every §15.1 row: retired / changed / accepted
7.  Data handover            archive manifest, provenance, access levels (§10.10)
8.  Publications & outreach  status against §14.8
9.  Recommendations          for the next season: design, ops, ML, training
10. Signatures               ops lead, science lead, programme lead
================================
```

> The wrap-up is the input to the next season's planning — the programme's memory lives in
> writing, not in anyone's head (§15.5, §17.11).

---

### 13.13 A day in the life — three operational vignettes

The runbooks of §13.1–13.12 are abstract; here is what they look like as days. Each vignette is
written from the operator's console and is used verbatim in training drills (§14.7) and the
simulator scenario bank (Appendix H).

```mermaid
sequenceDiagram
    autonumber
    participant OPS as Operator (NCPOR)
    participant SRV as Mission server
    participant SAT as Satellite link
    participant WG as Wave Glider
    participant FL as Argo Float

    loop 3 scheduled passes/day (summer)
        WG->>SAT: telemetry + science backlog (M05/M06)
        SAT->>SRV: frame relay
        SRV->>OPS: dashboard update ≤ 2 s
    end
    Note over OPS,SRV: planner runs on new forecasts
    SRV-->>WG: checkpoint list + config (M07/M08)
    WG-->>FL: (rendezvous day only) announce + lock (§6.2)
    FL-->>WG: profile packets + health (M01)
    WG->>SAT: combined dataset relay
    SAT->>SRV: ingest → QC → archive (§10.11)
    SRV->>OPS: scorecard posted (§6.8)
    OPS->>SRV: acknowledge / adjust / override (audited, §11.2)
```

**Vignette 1 — a storm day.** 03:10 UTC, mid-season. The barometer on the glider has fallen
8 hPa in six hours; wind gusts are 28 m/s. The wave sensor reports 6.2 m significant height —
above the 2.5 m docking gate (§6.6 `SELL`). The float is scheduled to surface at 04:00.

| Time (UTC) | Event | Decision |
|---|---|---|
| 03:10 | Storm signature detected | Alert L3 raised automatically; operator on console within 5 min |
| 03:20 | Forecast confirms 18 h of storm | Surfacing window extended 12 h (M08); glider stands off 30 km |
| 04:00 | Float surfaces in the (now wider) zone | No dock attempt — `SELL` exceeded; float waits on burst + short window |
| 04:15 | Essentials burst received via satellite | Science safe; full profile remains on float |
| 16:00 | Storm passes | Planner re-optimises; rendezvous succeeds at the extended window |
| 17:30 | Full offload + partial charge | Scorecard: window-2 success, one dock attempt (§6.8) |

<p align="center">
  <img src="assets/images/southern_ocean_storm.jpg" alt="A ship crashing through Southern Ocean waves" width="65%"/>
</p>

*The Southern Ocean earns its reputation — a ship crashing through storm waves (Getty Images
photo, illustrative). The mission's answer to this sea is not courage but margins: stand-off
distance, extended windows, and a burst path that needs no meeting at all (§6.5).*

**Vignette 2 — an ice day.** Winter, 09:00. The daily ice chart shows a tongue of the marginal
ice zone extending 15 km further north than yesterday's chart — directly across checkpoint 3.

| Time (UTC) | Event | Decision |
|---|---|---|
| 09:00 | Ice chart ingest; polygon updated | Planner replans within 15 s (§8.11 emergency budget) |
| 09:20 | New route skirts the tongue, +40 Wh, +6 h | Accepted automatically (within policy envelope, §8.7) |
| 11:00 | Glider executes the detour | Winter doctrine: ride the edge, never the interior (§13.10) |
| 14:00 | Optional tracker (if deployed) confirms tongue drift | Hazard model updated; no further action needed |
| 23:00 | Day closes without alerts | Winter's quiet days are logged, not forgotten (§15.5) |

<p align="center">
  <img src="assets/images/weddell_ice_seals.jpg" alt="Crabeater seals resting on ice floes in the Weddell Sea" width="65%"/>
</p>

*Weddell Sea ice floes with crabeater seals (Britannica photo, illustrative). Ice is not just a
hazard to the mission — it is habitat. The mission's avoidance policy (§8.3) protects both the
vehicles and the environment they measure (§14.6).*

**Vignette 3 — a ship-visit day.** The season's one ship call (§13.11), 14:00 local. ORV
*Sagar Nidhi*-class logistics are in the box; the glider is commanded to the meeting point two
days earlier.

| Time (UTC) | Event | Decision |
|---|---|---|
| D−2 06:00 | Glider receives visit waypoints | Transit in transit-only mode to save power (§8.12 rung 5) |
| D−1 12:00 | Go/no-go review: weather OK | Visit confirmed; recovery gear staged (§13.11 V-3) |
| Visit 14:00 | Glider recovered to deck | On-deck checks against the debugger (§9.4); batteries topped |
| Visit 16:00 | Data verified on deck, not just on the server | Byte-level check of the backlog against the relay copy |
| Visit 18:00 | Glider redeployed, float recalled and serviced | Both vehicles back on station by nightfall |
| D+1 09:00 | Debrief + report uploaded | One lessons-learned entry: deck checklist took 90 min vs 60 planned |

<p align="center">
  <img src="assets/images/sagar_nidhi.jpg" alt="ORV Sagar Nidhi at sea" width="60%"/>
</p>

*ORV Sagar Nidhi, India's ice-strengthened research vessel (Wikipedia photo, illustrative) —
the ship class that would deliver and recover the vehicles. The choreography of §13.11 treats
the ship as a third, very capable, very expensive vehicle.*

> 📖 **Why vignettes matter.** A runbook tells you the rule; a vignette tells you the *feel* —
> what time the alert wakes you, which decision is automatic and which is yours, and what the
> day's log line looks like afterwards. Training drills run these three days in the simulator
> until every operator can call the next move before the log shows it.

## 14. Expected Benefits and Impact

### 14.1 Scientific benefits

| Benefit | Detail |
|---|---|
| **All-year data through storm & darkness** | Not just summer expeditions — winter, under-sampled and seasonal-ice regions included |
| **Two depths at once** | Atmosphere + full 2,000 m water column from the same patch of ocean |
| **Sustained 4-D time-series** | Repeated T/S (and optional biogeochemical) profiles + co-located atmospheric and wave measurements |
| **Novel co-located datasets** | Air–sea forcing, currents, ice and the underlying water column together |
| **Iceberg drift records** | From the optional tracker — valuable to glaciology and hazard science |

### 14.2 Operational and economic benefits

| Benefit | Detail |
|---|---|
| **Ships ↓** | One deployment per season, not repeated voyages |
| **Cost per observation ↓** | Dramatic reduction in ship time and fuel per profile |
| **Safety ↑** | People no longer need to remain in hazardous seas |
| **Long life** | In-situ recharge extends float endurance; fewer losses to depleted batteries |
| **Resilient data return** | Two communication paths + store-and-forward buffering |
| **Reusable architecture** | The paired-vehicle concept, embedded software, ML models and dashboard can be replicated for other regions, fleets of pairs, or other hazard types |

### 14.3 Capability and strategic benefits

| Benefit | Detail |
|---|---|
| **Domestic expertise** | Autonomous marine robotics, ocean ML, polar operations, mission software — built and retained in-country |
| **International alignment** | Argo-standard data contribute to global observing programmes |
| **National polar science** | Serves national polar-science and strategic objectives in the Southern Ocean |
| **Platform for scale** | One proven pair is a template for a fleet |

### 14.4 Key performance indicators

| KPI | Target (illustrative) | Measured by |
|---|---|---|
| Profiles delivered per year | ≥ 30 | Mission server archive |
| QC pass rate (real-time) | ≥ 95 % good/probably-good | QC flags vs Argo convention |
| Rendezvous success rate | ≥ 80 % | Rendezvous log |
| Data loss on failed rendezvous | 0 % (essentials always delivered) | Burst receipts |
| Prediction coverage | ≈ advertised confidence | Skill diagnostics |
| Ice stand-off violations | 0 | Planner + vehicle logs |
| Vehicle losses | 0 | Mission log |
| Manual overrides exercised | ≥ 1 (as a drill) | Audit trail |

### 14.5 Impact monitoring and evaluation plan

Benefits (§14.1–14.3) are claims until measured. Each claim has a metric and a review point:

| Claim | Metric | Data source | Review point |
|---|---|---|---|
| Year-round data delivery | Profiles/month through winter vs summer | Archive (§10.3) | End of season |
| Better regional coverage | New profiles in previously-unsampled cells vs climatology of sampling | Archive vs historical map | Post-season science review |
| Reduced ship dependence | Ship days used vs a ship-based equivalent survey | Ops records | Programme review |
| Extended asset life | Float operational days vs standard float lifetime expectation | Mission log | End of mission |
| Indigenous capability built | Personnel trained, code/models reusable, publications | Team records, repo | Phase gates + annual review |
| International contribution | Data accepted by national archive / Argo GDAC | Archive acknowledgements | Post-season |

**Evaluation instruments**

1. **Seasonal science report** — data quality, coverage, key events (storms, ice, rendezvous
   statistics).
2. **Engineering post-mortem** — every fault, fallback and anomaly, with the event log as
   evidence (§9.5).
3. **Prediction-skill report** — coverage vs confidence, radius by horizon, per §7.5.
4. **Cost-per-observation study** — total programme cost vs profiles and met-hours delivered.

### 14.6 Sustainability and environmental responsibility

| Commitment | How |
|---|---|
| Zero fuel, zero emissions in operation | Wave + solar propulsion (§5.1) |
| No single-use deployment hardware | Reusable/recoverable vehicles; minimal packaging |
| No chemical release | Mineral-oil buoyancy engine, sealed; no batteries disposed at sea |
| Minimal marine impact | Low-speed, low-signature vehicles; no anchoring; no antifouling biocides beyond marine-grade practice |
| Hazard documentation | The mission itself maps ice hazards — a public-safety side-benefit |
| End-of-life plan | Recovery or, if loss occurs, tracked final position reported; hulls are inert and pressure-tested |

> The mission's entire design philosophy — renewable energy, endurance over replacement, data
> over presence — is itself a sustainability statement for ocean science.

### 14.7 Capacity building and training plan

The programme's lasting output is not only data — it is people who can build and run such
systems. Every phase (§16) therefore has explicit training objectives.

| Audience | Programme | Delivered by | Phase |
|---|---|---|---|
| NCPOR engineers | Robotics integration, comms, power, docking mechanics | Core team + vendor schools | P1–P2 |
| NCPOR operators | Mission control, contact planning, emergency drills (§13.9) | Core team; simulator hours required | P2–P3 |
| ML scientists | Drift prediction, calibration, monitoring (§7) | Core ML team; held-out-float exercises | P1–P3 |
| Data managers | Argo GDAC formats, QC, provenance (§10) | Data team + international Argo guidance | P1–P2 |
| Students/interns | Co-located projects on public dataset subsets | Academic partners | All phases |
| Reviewers & leadership | System-level reading course: this README + simulator | Core team | P1 gate |

**Training rules**

1. **Simulator hours are currency.** No operator touches the real console without N logged
   simulator sessions, including at least two emergencies (§8.9 scenario bank).
2. **Drills are scheduled, not optional** — the E-drill calendar (§13.9) is part of the ops plan.
3. Every training session itself produces one lessons-learned entry (§15.5) — training the
   trainers is how the programme compounds.

**Success measure.** At season end, NCPOR should be able to run a second pair of vehicles with
the original core team in an advisory role only — *the programme is complete when it has made
itself unnecessary*.

### 14.8 Publication and outreach plan

| Output | Target venue / audience | Timing |
|---|---|---|
| System architecture paper | Peer-reviewed ocean-engineering journal | After P3 pilot |
| Winter flux dataset (Q1–Q5, §2.5) | Argo GDAC + national data centre | Season end + 6 months |
| Prediction-skill study | ML-for-oceanography venue | After one full season |
| Open architecture docs | Public repository (from §17.11 openness policy) | Rolling |
| Public science communication | NCPOR outreach; school material built on the dashboard's public views | P3 onward |
| Policy briefing | Ocean observing and climate policy stakeholders | Season end |

**Ground rules.** No publication claims performance the scorecards (§6.8, §7.10) do not support;
every paper cites the vehicles' real, QC'd data; negative results (what failed and why) are
published with the same standing as positive ones.

---

### 14.9 Impact pathways

Benefits (§14.1–14.3) do not happen automatically; they happen along *pathways* — each one a
chain of funded, owned, measured steps from observation to outcome. This section draws those
chains explicitly so that impact is auditable, not asserted.

<p align="center">
  <img src="assets/figures/fig_30_impact_pathways.png" alt="Impact pathways" width="95%"/>
</p>

*Figure 30 — the three pathways from mission outputs to societal benefit. Each arrow is a
deliverable with an owner; each box has a KPI from §14.4 and a review cadence from Appendix O.*

```mermaid
flowchart LR
    A["🌡️ winter observations<br/>profiles + fluxes"] --> B["🧪 quality-controlled archive<br/>netCDF/CF · provenance"]
    B --> C["📄 science analyses<br/>Q1–Q5 (§2.5)"]
    C --> D["🗣️ publications + briefings<br/>(§14.8)"]
    D --> E["🏛️ monsoon & climate policy<br/>better seasonal prediction"]
    A --> F["📊 data centre delivery<br/>Argo GDAC + national"]
    F --> G["🌐 community reuse<br/>models, reanalyses, students"]
    G --> E
    A --> H["🛠️ platform & ops capability<br/>rendezvous, ML, winter ops"]
    H --> I["🚢 follow-on missions<br/>fleet scaling (§16.6)"]
    I --> J["🏭 industry & academia<br/>jobs, spinoffs, standards"]
    J --> E
```

| Pathway | Mechanism | KPI (from §14.4) | Owner | Time horizon |
|---|---|---|---|---|
| **Climate science** | Winter flux + profile dataset → peer-reviewed analyses → policy briefings | Publications; profiles in data centre; citation count | Science lead | 2–5 years |
| **Operational oceanography** | Real-time T/S + met into forecasting centres' pipelines | Skill-score feedback; latency (§10.11) | Ops + partner centres | 1–3 years |
| **Capability building** | Trained operators, reusable architecture, open docs | Trained staff count (§14.7); follow-on mission approved | Programme lead | 3–7 years |
| **International standing** | India's Argo contribution extended with recharge/ice ops | GDAC deliveries; joint-mission invitations | NCPOR | 2–6 years |

**Honesty rules for impact reporting** (carried over from §14.5)

1. Impact claims cite the pathway, the KPI, and the number — never adjectives alone.
2. A pathway that stalls is reported with the same prominence as one that succeeds.
3. The impact review (§14.5) scores each pathway annually against its baseline, and the
   lessons-learned register (§15.5) records *why* — the pathway map is a living document, not a
   poster.

> 🔭 The mission's lasting value is decided less by the two vehicles than by these pathways:
> the same winter profile can change a forecast, a paper, a student, or a policy — the
> difference is which arrows the programme actually staffs and funds.

## 15. Risk Register and Mitigations

### 15.1 The register

| ID | Risk / challenge | Why it matters | Likelihood | Impact | Response in the design | Residual risk |
|---|---|---|---|---|---|---|
| R1 | **Missed rendezvous** | The glider cannot reach the float in time under the currents/sea state. | Possible | Moderate | Probabilistic zones, arrive-early loiter, continuous replanning; float extends surface window where possible; direct satellite burst and retry next cycle guarantee no data loss. | Data delivered late, not lost |
| R2 | **Prediction error** | Current/forecast uncertainty grows over 10 days; the float could surface outside the zone. | Likely | Moderate | Ensemble/uncertainty-aware models sized against historical accuracy; confidence-scored routing; the float's own GPS burst at surfacing corrects the estimate; models retrain on every outcome. | Zone may be large early on |
| R3 | **Iceberg / sea-ice encounter** | Collision or entrapment can destroy a vehicle; a float can surface under ice. | Possible | Severe | On-board sensing + ice charts + optional tracker; no-go polygons with stand-off; automatic rerouting; safe-hold/abort; float surfacing logic informed by ice conditions. | Controlled, fail-safe behaviour |
| R4 | **Insufficient solar power** | Cloud, winter darkness and high comms load can deplete the glider battery. | Likely | Moderate | Wave propulsion needs no electricity; conservative battery sizing; prioritised load-shedding; charge targets per rendezvous; fallback to the float's direct burst. | Reduced winter cadence |
| R5 | **Docking in rough seas** | Locking two small vessels together in swell is mechanically demanding. | Likely | Moderate | Compliant, fault-tolerant capture mechanism; quick data-first transfer; abort-and-retry; no dependency of science delivery on a successful dock. | Satellite fallback |
| R6 | **Communication outages / high latitude** | Satellite geometry and weather can interrupt links. | Likely | Minor | Polar-capable constellation; on-board buffering and store-and-forward; two independent data paths; autonomous safe behaviour when shore is unreachable. | Latency, not loss |
| R7 | **Sensor & hardware faults** | A failed instrument at sea is invisible until recovery without monitoring. | Possible | Moderate | Continuous health monitoring, watchdogs and time-stamped logs; pre-deployment debugger gate; shore QC; graceful degradation (other sensors keep working). | Partial science |
| R8 | **Biofouling & harsh environment** | Marine growth and cold affect sensors, mechanisms and seals over months. | Unlikely | Moderate | Proven marine-grade hardware and antifouling practice; periodic self-checks; service/recovery in the operating plan; conservative material and seal specifications. | Seasonal servicing |
| R9 | **Cyber / command safety** | Mission commands and data links must be trustworthy. | Unlikely | Severe | Authenticated, encrypted command channel; validated and versioned mission plans; human approval for major actions; full audit logging. | Defence in depth |

### 15.2 The risk matrix

🖼️ **Figure 16 — Risk matrix** — every major risk is mitigated in the design:

<p align="center">
  <img src="assets/figures/fig_16_risk_matrix.png" alt="Risk matrix" width="80%"/>
</p>

### 15.3 Escalation and reporting

| Level | Trigger | Response |
|---|---|---|
| **L1 — advisory** | Routine alert, plan accepted | Logged; dashboard notification |
| **L2 — attention** | Alert requiring operator review | Operator acknowledges within shift; decision logged |
| **L3 — critical** | Vehicle at risk (ice, battery, contact lost) | Immediate page to operator on call; ML/engineering consulted |
| **L4 — emergency** | Loss or imminent loss of vehicle | Mission lead + NCPOR management; contingency playbook §13.5 |

> 📊 Risk posture reviews happen at every phase gate (§16) — the register above is living
> documentation, updated as evidence arrives from testing.

### 15.4 Subsystem failure catalog

The register (§15.1) is mission-level; this catalog drills into each subsystem so that fault
localisation (§9.2) has a defined response for every known failure mode.

| Subsystem | Failure mode | Detection | Automatic response | Worst-case outcome |
|---|---|---|---|---|
| Solar array | Panel shading/fouling, cell degradation | MPPT yield vs irradiance model | Load-shedding; re-plan energy | Reduced winter cadence (R4) |
| Glider battery | Capacity fade, cold soak | SOC vs coulomb-count drift | Conservative SOC floor; charge management | Reduced rendezvous capability |
| Rudder actuator | Jam, high current | Current sense + position feedback | Freeze steering; wave-steer only; alert | Degraded transit (R1) |
| Satcom modem | No registration, RX desense | Pass failures, RSSI | Retry ladder; store-and-forward; antenna switch if fitted | Data latency, no loss (R6) |
| Short-range link | No handshake at rendezvous | Pairwise test + telemetry | Retry once; float falls back to burst | Burst-only data path (R1) |
| Dock mechanism | Lock sensor fault, release jam | Sensor disagreement, actuator current | Abort sequence; never leave coupled | Satellite fallback (R5) |
| Charge path | Over-temperature, wet connector fault | Charge curve monitoring | Terminate charge; retry next cycle | Float keeps primary battery (R5) |
| Float pump | Stall, high current, slow cycle | Pump telemetry + cycle timing | Shortened cycle; early surface; alert | Fewer profiles (R7) |
| CTD | Stuck value, drift | QC-05/QC-10 ashore + on-board ranges | Flag bad; continue other sensors | Partial profile (R7) |
| Float bladder | Leak, slow rise | Buoyancy model vs ascent rate | Early surface; safe-hold; alert | Recovery decision (R3/R7) |
| On-board storage | Bad blocks, write errors | Integrity checks, wear counters | Remap; prioritise offload | Managed within margin (R7) |
| Watchdog system | Reset storms | Reset counters (§9.6) | Safe-hold after 3 in 24 h | Vehicle safe, degraded |

> 🔧 This table feeds the hand-held debugger's pass/fail definitions (§9.4): every row that can
> be tested on deck **is** tested on deck.

### 15.5 Lessons-learned register (template)

Every significant event (success or failure) produces one entry. The register is reviewed at the
risk board (§Appendix O).

| Field | Example |
|---|---|
| ID | LL-2026-017 |
| Date / phase | 2026-08-14 · P4 winter |
| Event | Rendezvous after 40-day gap in polynya |
| What happened | Full backlog offloaded in one window |
| What worked | Planner's polynya routing; data-first ordering |
| What surprised us | Charge target reached 12 min early |
| Action | Raise default target SOC for polynya windows (+5 %) |
| Linked risk | R1, R4 (§15.1) |
| Owner / status | Ops · done in config v4 |

---

## 16. Phased Development Roadmap

Risk is retired **progressively**. No phase proceeds until its gate is passed.

🖼️ **Figure 17 — Phased development Gantt (illustrative):**

<p align="center">
  <img src="assets/figures/fig_17_gantt.png" alt="Development roadmap Gantt" width="95%"/>
</p>

```mermaid
timeline
    title Phased development roadmap
    Phase 1 · Shore build : dock & charge link build : tank and captive tests : embedded software + health monitor
    Phase 2 · Coastal trials : glider–float rendezvous trials : failure-mode exercises : data pipeline shakeout
    Phase 3 · Polar pilot : handful of cycles with close ship support : ML validation at sea : dashboard & ops drill
    Phase 4 · Full season : unattended seasonal autonomy : 30–35 cycles delivered : handover & scale-up review
```

### 16.1 Timeline at a glance

| Phase | Duration (illustrative) | Focus |
|---|---|---|
| P1 Shore-based build | ~9 months | Dock/charge link, embedded software, tank testing |
| P2 Coastal trials | ~6 months | Rendezvous in real sea state, easy recovery |
| P3 Short polar deployment | ~9 months | Pilot cycles with close ship support |
| P4 Full seasonal autonomy | ~12 months | Unattended mission, science delivery |

### 16.2 Phase details and exit gates

| Phase | Work items | Exit gate (all must hold) |
|---|---|---|
| **P1 · Shore-based build** | Dock/charge link build; tank/captive tests; embedded software & health monitor; ML models trained on historical data | Dock, data-transfer and charge cycle verified end-to-end in controlled conditions; health monitor and logger operational; ML passes held-out validation |
| **P2 · Coastal trials** | Glider–float rendezvous trials; failure-mode exercises (abort, retry, satellite fallback); data pipeline shakeout | Repeated successful autonomous rendezvous in real sea state; every fallback in §6.5 exercised and verified |
| **P3 · Short polar deployment** | Handful of cycles with close ship support; ML validation at sea; dashboard and ops drill | Predicted surfacing zones contain the actual surfacing point at advertised confidence; sea→satellite→shore→dashboard pipeline verified; one manual-override drill completed |
| **P4 · Full seasonal autonomy** | Full season unattended, alerts and human-in-the-loop oversight; science products delivery | 30–35-cycle year achieved; ≥ 80 % rendezvous success; zero ice violations; zero vehicle losses; data accepted by national archive |

> 🧪 **ML validation precedes deployment.** Models are validated against existing Argo and glider
> datasets before ever guiding a vehicle (§7.5). Each phase gate includes a re-run of the full
> validation suite.

### 16.3 Dependencies between phases

```mermaid
flowchart LR
    P1["P1 · shore build"] --> P2["P2 · coastal trials"]
    P2 --> P3["P3 · polar pilot"]
    P3 --> P4["P4 · full season"]
    M1["ML training & validation"] --> P3
    M2["ML at-sea validation"] --> P4
    DB["dashboard + server MVP"] --> P2
    DB2["dashboard + override complete"] --> P3
    SHP["ship slot + polar logistics"] --> P3
    SHP2["seasonal ship visit"] --> P4
```

### 16.4 Work breakdown structure and deliverables

The roadmap (§16.1–16.3) decomposes into work packages with named deliverables. This is the
basis for the project plan, resourcing and the review agenda.

| ID | Work package | Phase | Deliverables |
|---|---|---|---|
| WP-01 | Concept & system architecture | P1 | This blueprint; architecture ADRs; requirements register |
| WP-02 | Dock & charge engineering | P1 | Dock mechanism prototype; charge path; tank-test report |
| WP-03 | Embedded software core | P1 | Shared OS, health monitor, event logger; HITL report |
| WP-04 | Wave Glider integration | P1–P2 | Sensor suite, nav, comms; integration test report |
| WP-05 | Argo Float integration | P1–P2 | CTD, buoyancy control, ice-aware surfacing; test report |
| WP-06 | Hand-held debugger | P1 | Debugger app + vehicle-side test harness; checklist spec |
| WP-07 | ML — glider trajectory model | P1–P3 | Model v1..vN; validation reports (coverage, radius) |
| WP-08 | ML — surfacing zone model | P1–P3 | Model v1..vN; validation reports; calibration artefacts |
| WP-09 | Planning engine | P1–P3 | Checkpoint generation, rendezvous optimisation, ice avoidance; simulator regression suite |
| WP-10 | Mission server | P1–P2 | Ingest, decode, QC, archive, alerting, command queue; API spec |
| WP-11 | Dashboard | P1–P3 | Live map, data views, alerts, RBAC, override UI |
| WP-12 | Simulator / digital twin | P1–P3 | Scenario catalog; SITL/HITL harness; replay tools |
| WP-13 | Iceberg tracker (optional) | P2–P3 | Tracker hardware, shore ingestion, planner integration |
| WP-14 | Coastal trials campaign | P2 | Trial plan, safety case, trial reports, fallback drills |
| WP-15 | Polar pilot campaign | P3 | Pilot plan, deployment runbook, ship coordination, pilot report |
| WP-16 | Full-season operations | P4 | Seasonal ops plan, playbooks, contingency procedures |
| WP-17 | Science data management | P2–P4 | Formats, QC, archival pipeline, publication support |
| WP-18 | Training & handover | P4 | Operator training, developer onboarding, knowledge base |

**Milestone schedule (illustrative, aligned to §16.1)**

| Milestone | When | Criterion |
|---|---|---|
| M1 · Architecture approved | P1 start + 2 mo | ADRs ratified; requirements baselined |
| M2 · Dock bench demo | P1 end | Dock + charge verified in tank |
| M3 · Coastal rendezvous achieved | P2 end | Repeated autonomous rendezvous at sea |
| M4 · Polar pilot completed | P3 end | Zones validated at sea; pipeline verified |
| M5 · Full season completed | P4 end | KPI targets (§14.4) met; data archived |

### 16.5 Resourcing sketch (illustrative)

| Role | P1 | P2 | P3 | P4 |
|---|---|---|---|---|
| Systems engineer | 1.0 | 1.0 | 0.5 | 0.3 |
| Embedded/firmware | 2 | 2 | 1 | 0.5 |
| Mechanical (dock) | 1 | 1 | 0.5 | 0.2 |
| ML engineers | 2 | 2 | 1 | 0.5 |
| Backend/frontend | 2 | 2 | 1 | 0.5 |
| Ops & field engineer | 0.5 | 1 | 2 | 1 |
| NCPOR science | 0.5 | 0.5 | 1 | 1 |
| **Total FTE (approx.)** | **9** | **9.5** | **7** | **4** |

> Numbers are planning figures for budgeting conversations only — not commitments.

### 16.6 Risk-retirement curve

The roadmap is explicitly a risk-retirement sequence. The curve below is reviewed at each gate:

| Phase | Top risks being retired | Expected remaining exposure |
|---|---|---|
| P1 (shore build) | R5 docking feasibility, R7 hardware quality, R9 security | R1–R4 untested but designed |
| P2 (coastal trials) | R5 at sea, R1 meeting mechanics, R6 comm fallbacks | R3 ice still theoretical |
| P3 (polar pilot) | R2 prediction at sea, R3 ice handling, R4 winter energy | Confidence replaces theory |
| P4 (full season) | Residual risk accepted explicitly, monitored continuously | Managed risk, not zero risk |

> 🛡️ The honest position: autonomy risk is never eliminated, it is **retired to a known,
> monitored, and accepted level** — and that level is documented at every gate.

---

# PART V — THE ENGINEERING BLUEPRINT

---

## 17. Repository Structure (Blueprint)

This repository is organised as a **monorepo**: firmware, ML, planning, shore server, dashboard
and tooling evolve together against one shared contract (the mission cycle and data formats).

### 17.1 Directory layout

```text
cooperative-polar-observation/
├── README.md                        ← this file — the canonical blueprint
├── LICENSE                          ← TBD until publication strategy approved
├── .github/
│   ├── workflows/                   ← CI/CD pipelines (§18.6)
│   └── ISSUE_TEMPLATE/              ← issue + design-review templates
├── docs/                            ← detailed design & operations documents
│   ├── concept-overview.md            (mirror of the v1.0 overview PDF, text only)
│   ├── architecture.md                (system + comms + resilience + security)
│   ├── mission-cycle.md               (rendezvous sequence, docking, failure modes)
│   ├── ml-models.md                   (Model 1 & 2 specs, validation protocol)
│   ├── planning-engine.md             (checkpoints, objective, ice handling, energy)
│   ├── data-format.md                 (formats, QC conventions, Argo alignment)
│   ├── dashboard-spec.md              (views, alerts, RBAC, override workflow)
│   ├── decisions/                     (ADRs — architecture decision records)
│   └── operations/
│       ├── deployment-checklist.md    (hand-held debugger checklist, run book)
│       └── year-in-the-field.md       (seasonal ops plan, hold/re-plan logic)
├── firmware/
│   ├── common/                      ← shared embedded OS, health monitor, event
│   │                                  logger, mission state machine, watchdogs
│   ├── wave-glider/                 ← navigation, communications, dock/charge
│   │                                  controller
│   └── argo-float/                  ← dive/profile control, ice-aware surfacing
│                                      logic, buoyancy engine driver
├── ml/
│   ├── glider-trajectory/           ← Model 1: trajectory + reachable set +
│   │                                  uncertainty envelope
│   ├── float-surfacing/             ← Model 2: surfacing ellipse + confidence
│   ├── ensembling/                  ← Monte-Carlo / probabilistic forecasting
│   ├── validation/                  ← held-out cycle scoring (coverage, radius)
│   └── notebooks/                   ← experiments, retraining, skill reports
├── planner/                         ← checkpoint generation, rendezvous
│                                      optimisation, ice avoidance, energy budgets
├── server/                          ← shore mission server: ingest, decode, QC,
│                                      archive, alert engine, command queue, API
├── dashboard/                       ← live web mission platform (map, tracks,
│                                      zones, alerts, data views, override UI)
├── tools/
│   ├── handheld-debugger/           ← pre-deployment verification app
│   │                                  (rugged tablet)
│   └── simulator/                   ← mission digital twin: replay, scenario
│                                      tests, model training support
├── data/
│   ├── schemas/                     ← versioned payload schemas (vehicles ⇄ shore)
│   ├── samples/                     ← sample profiles, telemetry, alerts
│   └── qc/                          ← QC rule sets and reference outputs
├── assets/
│   ├── images/                      ← Google-sourced illustrative photographs
│   └── figures/                     ← Python-generated blueprint figures
├── scripts/
│   ├── gen_figures.py               ← regenerates all README figures
│   ├── gen_figures_part1.py
│   └── gen_figures_part2.py
└── tests/                           ← integration & system tests (SITL/HITL
                                       where applicable)
```

### 17.2 Module contracts

Each module exposes a narrow, versioned interface. Anything crossing a module boundary must use
it.

| Module | Consumes | Produces | Contract artefact |
|---|---|---|---|
| `firmware/common` | hardware drivers | health packets, event log | `data/schemas/health.json` |
| `firmware/wave-glider` | planner checkpoints (uplink) | telemetry, met data, relay frames | `data/schemas/telemetry.json` |
| `firmware/argo-float` | cycle config, ice surfacing rules | profiles, float health | `data/schemas/profile.json` |
| `ml/glider-trajectory` | telemetry history, forecasts | trajectory artefact (netCDF/JSON) | `ml/contracts/trajectory.md` |
| `ml/float-surfacing` | last fix, forecasts, climatology | surfacing-zone artefact | `ml/contracts/surfacing.md` |
| `planner` | ML artefacts, hazards, constraints | checkpoint lists, decisions | `data/schemas/plan.json` |
| `server` | satellite gateway frames | archived data, products, events | `data/schemas/`, REST API spec |
| `dashboard` | server API + WebSocket | UI, operator commands | OpenAPI spec in `server/` |
| `tools/simulator` | same schemas as real vehicles | synthetic mission scenarios | schema parity tests |

### 17.3 Technology stack

> These are **proposed defaults for ratification** by the engineering team; they are not
> specified in the concept document. Ratification happens as ADRs in `docs/decisions/`.

| Component | Proposed stack | Notes |
|---|---|---|
| Embedded firmware | C on a real-time OS (FreeRTOS/Zephyr-class) | Both vehicles share the same layered architecture; HAL per vehicle |
| ML models | Python, PyTorch or JAX | Sequence/time-series models + ensemble/Monte-Carlo forecasting; physics-informed blending |
| Planning engine | Python (fast prototyping) → Rust/C++ (deploy) | Optimisation over probability zones, not points |
| Mission server | Python (FastAPI), PostgreSQL, MQTT/NATS for telemetry | Argo-standard formats in, standard products out |
| Dashboard | TypeScript/React, MapLibre-class map, WebSockets | Live digital twin; role-based access |
| Hand-held debugger | Cross-platform app (Flutter-class) on rugged tablet | Wired plugin to each vehicle; pass/fail checklist |
| Simulator | Python digital twin | Same message contracts as real vehicles |
| CI/CD | GitHub Actions (server, dashboard, ML) + self-hosted runners (firmware HITL) | See §18.6 |

### 17.4 Coding standards

| Area | Standard |
|---|---|
| Python | PEP 8, type hints, `ruff` + `mypy`; ≥ 80 % test coverage on new code |
| TypeScript | ESLint + Prettier, strict mode |
| C (firmware) | MISRA-C-inspired subset, static analysis (clang-tidy/cppcheck), watchdog-aware code |
| Docs | This README is the spec; ADRs for every design decision |
| Commits | Conventional Commits (`feat(planner): …`, `fix(server): …`) |
| Data | Schema-first; every schema change is a reviewed migration |
| ML | Every model change ships with a validation report (§7.5) |

### 17.5 The Git workflow

```mermaid
gitGraph
    commit id: "init: blueprint v2"
    branch feature/planner-ice-avoid
    commit id: "feat(planner): no-go polygons"
    commit id: "test(planner): hazard regression"
    checkout main
    merge feature/planner-ice-avoid tag: "v0.2.0"
    branch feature/ml-ensemble
    commit id: "feat(ml): monte-carlo spread"
    commit id: "docs(ml): validation report"
    checkout main
    merge feature/ml-ensemble tag: "v0.3.0"
    branch release/polar-pilot
    commit id: "chore: freeze configs"
    checkout main
    merge release/polar-pilot tag: "v1.0.0-pilot"
```

| Branch | Purpose | Rules |
|---|---|---|
| `main` | Always-deployable, validated state | Protected; PR + review + CI green required |
| `feature/*` | One concern per branch | Conventional commits; short-lived |
| `release/*` | Mission-configuration freezes (pilot, season) | No feature work; config & docs only |
| `hotfix/*` | Emergency fixes at sea | Fast-track review; mandatory post-mortem |

### 17.6 Environments, configuration and secrets

| Environment | Purpose | Data | Deploy |
|---|---|---|---|
| `dev` | Local development | Synthetic/sample data only | Manual |
| `staging` | Pre-deploy validation; replay of real mission segments | Anonymised replays | CI-managed |
| `prod` | Live mission operations | Real telemetry, profiles | Change-controlled, operator-approved |

| Secret | Scope | Handling |
|---|---|---|
| Satellite gateway credentials | Mission server | Vault / sealed secrets; rotated per season |
| Vehicle command keys | Firmware + server | Provisioned at the debugger gate (§9.4); per-vehicle identity |
| Dashboard OIDC credentials | Dashboard | Standard IdP integration (SSO) |
| API tokens (data exports) | Consumers | Scoped, expiring, revocable |

**Rules:** secrets never in the repo, never in logs, never in issue trackers; configuration is
code (reviewed), secrets are not.

### 17.7 Dependency management and supply chain

| Area | Policy |
|---|---|
| Pinning | Lockfiles committed for all ecosystems (`requirements.lock`, `package-lock.json`, firmware manifest) |
| Review | Dependency changes reviewed like code; minimal-transitivity preferred |
| SBOM | Software bill of materials generated per release (firmware, server, dashboard) |
| CVE scanning | Automated advisory scan on every PR to a manifest |
| Firmware toolchain | Pinned compiler versions; reproducible builds as a Phase 1 goal (§16.2) |
| Data dependencies | Forecast/ice feeds versioned with fetch timestamps (planner inputs are reproducible) |

### 17.8 Test strategy

| Level | What | Where | Gate |
|---|---|---|---|
| Unit | Functions, state transitions, QC rules, schema validation | `tests/`, per-module CI | Merge |
| Contract | Message schemas across modules; API responses | CI (`schemas` pipeline) | Merge |
| Integration | Server + planner + simulator with real message traffic | CI nightly | Release |
| SITL | Firmware compiled and run against the digital twin | Self-hosted runner | Firmware merge |
| HITL | Real hardware rigs (dock bench, charge path, sensors) against simulator | Lab | Phase gates P1–P2 |
| Field trials | Coastal rendezvous, fallback drills | Coastal waters | Gate P2 → P3 |
| Polar pilot | Real mission subset with ship support | Southern Ocean | Gate P3 → P4 |
| Chaos drills | Kill links, inject hazards, corrupt frames — on staging | Staging | Pre-season |

| Scenario type | Examples (simulator catalog) | Must exercise |
|---|---|---|
| Nominal | Full 10-day cycle, calm-to-storm weather | Complete rendezvous chain |
| Degraded | Comm outage 72 h; satellite loss at rendezvous | Store-and-forward + burst fallback |
| Hazard | Iceberg on route; MIZ advance; tracker update mid-plan | Re-route / hold / abort ladder |
| Fault | Lock fault; charge over-temp; watchdog storm | Abort logic + alerts + logs |
| Seasonal | Winter darkness + ice; spring recovery | Load-shedding, safe-hold, recovery |

### 17.9 Observability: logging, metrics and tracing

The mission runs for months with humans only watching — the system must be diagnosable from the
shore alone.

| Pillar | Implementation | Example |
|---|---|---|
| **Logs** | Vehicle event logs (§9.5) + server structured logs, merged on `seq`/`msg_id` | `{"seq":41833, "event":"release_confirmed", ...}` |
| **Metrics** | Server and dashboard counters/gauges: pass counts, queue depth, battery SOC, prediction error | `rendezvous_success_total{vehicle="wg1"}` |
| **Tracing** | Correlation IDs on every message: `mission_id + msg_id` end-to-end | Trace a profile from CTD to archive |
| **Alerts** | Thresholds on metrics + log patterns (§11.2) | "satellite queue > 80 % for 2 consecutive passes" |

**Golden signals for operations**

| Signal | Healthy | Investigate | Critical |
|---|---|---|---|
| Last-contact age | < 12 h | 12–48 h | > 48 h |
| Glider SOC | > 60 % (season-adjusted) | 40–60 % | < 40 % |
| Float SOC at surfacing | > 50 % | 30–50 % | < 30 % |
| Pending satellite bytes | draining each pass | growing slowly | > 1 pass capacity |
| Prediction coverage (10-cycle) | ≈ advertised | −5 % drift | −10 % drift |
| Stand-off violations | 0 | 1 (investigate) | ≥ 2 |
| Watchdog resets / 24 h | 0 | 1–2 | ≥ 3 |

### 17.10 Configuration management

| Item | Managed by | Change process |
|---|---|---|
| Vehicle firmware | Git + reproducible builds | PR → HITL → phased rollout (glider first) |
| Mission configs (cycles, depths, windows) | Versioned YAML in repo | Reviewed change; validated in simulator before uplink |
| Model versions | Model registry (§7.9) | Validation gate → shadow mode → live |
| Dashboard & server | Git + CI/CD | Standard PR flow, zero-downtime deploys |
| Planner parameters | Config-as-code | Operator-approved changes only during ops |
| Keys & secrets | Vault | Rotation schedule; debugger-gate provisioning |

> 🔁 **Config drift is a mission risk.** Every uplinked configuration is checksummed and echoed
> back in telemetry; the dashboard diffs the intended vs actual config and raises an alert on
> any mismatch.

### 17.11 Repository governance

| Policy | Rule |
|---|---|
| Branch protection | `main` requires PR, review, CI green (§17.5) |
| Code ownership | `CODEOWNERS` maps every directory to a responsible team |
| ADR process | Design changes of consequence land as ADRs in `docs/decisions/` before code |
| Doc-code parity | README/`docs/` and code change in the same PR (§20.1) |
| Issue hygiene | Issues tagged by phase (P1–P4) and element (E1–E12); no untriaged backlog |
| Release cadence | Server/dashboard: continuous. Planner/models: gated. Firmware: mission-window releases |
| Archive | Quarterly snapshot of the repo + artefacts to the NCPOR archive |
| Openness | Internal to NCPOR now; a publication strategy for the architecture docs is planned |

**Review norms.** Reviews verify three things in order: (1) does it break the mission contract
(§4.7 schemas, §7.11 prediction contract)? (2) is it safe under §15's risk register? (3) is it
maintainable by the next engineer? Style is last.

---

## 18. Getting Started (Developers)

> ⚠️ Scaffold commands are **illustrative** until the stack in §17.3 is ratified and the
> repositories are created.

### 18.1 Prerequisites

| Tool | Version | Used for |
|---|---|---|
| Git | 2.30+ | Version control |
| Python | 3.11+ | Server, ML, planner, simulator, figure generation |
| Node.js | 20+ | Dashboard |
| Docker | 24+ | Local services (PostgreSQL, message bus) |
| Rust/C toolchain | per firmware team | Firmware (per-team setup) |

### 18.2 Quick start

```bash
git clone <repository-url>
cd cooperative-polar-observation

# 1. Shore mission server + API
cd server
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env                # set DB, keys, feeds
docker compose up -d postgres mqtt  # local backing services
uvicorn app.main:app --reload       # http://localhost:8000/docs

# 2. Live dashboard (separate terminal)
cd ../dashboard
npm install
npm run dev                         # http://localhost:5173

# 3. ML models — run held-out cycle validation
cd ../ml
pip install -r requirements.txt
make validate                       # writes ml/validation/report-<date>.md

# 4. Mission simulator (digital twin)
cd ../tools/simulator
python run_mission.py --scenario southern-ocean-summer --days 10
```

### 18.3 Running the ML models

```bash
cd ml

# Train Model 1 (glider trajectory) on historical tracks
python -m glider_trajectory.train --config configs/glider_v1.yaml

# Train Model 2 (float surfacing) on Argo history
python -m float_surfacing.train --config configs/float_v1.yaml

# Held-out validation: predict real past surfacings, score coverage & radius
python -m validation.run --models glider_v1 float_v1

# Serve predictions to the planner
python -m serving.api --port 8001
```

**Validation contract:** a model may only be promoted if coverage ≈ advertised confidence and
the zone radius does not regress versus the previous version (§7.5).

### 18.4 Running the simulator

```bash
cd tools/simulator

# List scenarios
python run_mission.py --list-scenarios
#   southern-ocean-summer     10 days, open water, calm→storm
#   winter-ice-advance        20 days, advancing ice, low solar
#   rendezvous-failures        failure injection: lock fault, comm outage
#   full-season               365 days, seasonal ice + daylight cycles

# Run a scenario and produce the same artefacts the real system would
python run_mission.py --scenario southern-ocean-summer --days 10 --out out/
```

The simulator speaks the **same message contracts** as the real vehicles — anything that passes
in simulation has a fighting chance at sea, and any dashboard/planner bug can be reproduced on
shore.

### 18.5 Firmware development

```bash
cd firmware/common

# Build + unit tests (host)
cmake -B build -DENABLE_TESTS=ON && cmake --build build
ctest --test-dir build

# HITL against the simulator (self-hosted runner with hardware rigs)
python ../../tools/simulator/hitl.py --vehicle wave-glider
```

Firmware merges require: static analysis clean, watchdog-aware review, and the simulated
pre-deployment checklist (§9.4) passing in CI.

### 18.6 CI/CD pipelines

| Pipeline | Triggers | Checks |
|---|---|---|
| `server` / `dashboard` | PRs to `server/`, `dashboard/` | Lint, unit tests, build, contract tests against sample telemetry |
| `ml` | PRs to `ml/`, scheduled nightly | Re-run held-out-cycle validation; **block merge on coverage or radius regression** |
| `firmware` | PRs to `firmware/` | Static analysis, HITL simulation against the digital twin |
| `docs` | PRs to `docs/`, `README.md` | Link check, consistency linter, figure-regeneration check |
| `schemas` | PRs touching `data/schemas/` | Backward-compatibility lint; downstream contract tests |

```mermaid
flowchart LR
    PR["pull request"] --> LINT["lint + static analysis"]
    LINT --> UNIT["unit tests"]
    UNIT --> CONTRACT["schema contract tests"]
    CONTRACT --> HITL{"HITL / simulation<br/>(firmware & ml)"}
    HITL --> REVIEW["human review + ADR check"]
    REVIEW --> MERGE["merge to main"]
    MERGE --> DEPLOY["deploy server/dashboard ·<br/>register model version"]
    MERGE --> TAG["tag release for<br/>mission configuration freeze"]
```

### 18.7 Troubleshooting and FAQ

| Problem | Likely cause | Fix |
|---|---|---|
| Dashboard shows stale data | WebSocket disconnected or satellite gap | Check server logs for gateway frames; dashboard shows "last contact" age by design |
| `make validate` fails on coverage | Model under- or over-confident | Re-tune ensemble spread; check calibration plot in report |
| Simulator diverges from real telemetry | Physics constants differ from real vehicle | Compare `tools/simulator/configs/vehicle.yaml` against as-built measurements |
| Figure PNGs out of date | README edited without regenerating | `python3 scripts/gen_figures.py` and commit `assets/figures/` |
| Mermaid diagram not rendering | Syntax error (unbalanced quotes/parens) | Render locally with `npx @mermaid-js/mermaid-cli` or preview in Typora/VS Code |
| Schema lint fails | A change breaks older vehicle firmware | Version the schema; add a compatibility shim in `server/` |

### 18.8 API usage examples

```bash
# Mission summary (any authenticated user)
curl -s https://mission.ncpor.example/api/v1/mission \
  -H "Authorization: Bearer $TOKEN" | jq .

# Latest surfacing-zone prediction
curl -s "https://mission.ncpor.example/api/v1/predictions?type=surfacing" \
  -H "Authorization: Bearer $TOKEN" | jq '{zone: .geometry, confidence: .confidence,
                                             radius_km: .radius_km, model: .model_version}'

# Acknowledge an alert (operator role)
curl -s -X POST "https://mission.ncporexample/api/v1/alerts/AL-2026-1142/ack" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"decision": "accept_autonomous", "note": "plan looks correct"}'

# Upload revised checkpoints (operator role)
curl -s -X POST "https://mission.ncporexample/api/v1/plan/checkpoints" \
  -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d @revised_plan.json

# Download a profile as netCDF (scientist role)
curl -s "https://mission.ncporexample/api/v1/profiles?cycle=17&format=netcdf" \
  -H "Authorization: Bearer $TOKEN" -o profile_cycle17.nc

# Audit trail excerpt (admin role)
curl -s "https://mission.ncporexample/api/v1/audit?from=2026-11-01&to=2026-11-12" \
  -H "Authorization: Bearer $TOKEN" | jq '.[] | {ts: .ts_utc, actor: .actor, action: .action}'
```

### 18.9 Local full-stack with Docker Compose

```yaml
# docker-compose.yml (illustrative — mirror of server/.env.example)
services:
  postgres:
    image: postgres:16
    environment: { POSTGRES_DB: mission, POSTGRES_USER: mission, POSTGRES_PASSWORD: mission }
    volumes: [pgdata:/var/lib/postgresql/data]
    ports: ["5432:5432"]

  mqtt:
    image: eclipse-mosquitto:2
    ports: ["1883:1883", "9001:9001"]

  server:
    build: ./server
    environment:
      DATABASE_URL: postgresql://mission:mission@postgres/mission
      MQTT_URL: mqtt://mqtt:1883
    ports: ["8000:8000"]
    depends_on: [postgres, mqtt]

  dashboard:
    build: ./dashboard
    environment: { API_BASE_URL: http://server:8000 }
    ports: ["5173:5173"]
    depends_on: [server]

  simulator:
    build: ./tools/simulator
    command: ["run_mission.py", "--scenario", "southern-ocean-summer", "--days", "10"]
    environment: { MQTT_URL: mqtt://mqtt:1883 }
    depends_on: [mqtt, server]

volumes:
  pgdata:
```

```bash
docker compose up --build        # everything: db + bus + server + dashboard + simulator
# dashboard → http://localhost:5173 · API docs → http://localhost:8000/docs
```

### 18.10 Extended FAQ

| # | Question | Answer |
|---|---|---|
| 1 | Why not just use existing Argo floats and a normal satellite link? | Because energy and airtime are the float's limits (§2.4). The pairing removes both: local offload is orders of magnitude cheaper than satellite per byte, and recharge removes the end-of-life cliff. |
| 2 | Why is the objective "P(rendezvous)" and not "minimum distance"? | A distance-minimising plan can be impossible (currents, ice) or expensive (headwind steering). A probability objective admits "arrive early and loiter inside the zone", which is the physically honest goal (§8.2). |
| 3 | What if the ML is wrong for several cycles in a row? | Coverage is monitored in production (§7.10). Sustained under-coverage triggers a fallback to climatology-based priors, wider zones and eventually human replanning — never blind trust. |
| 4 | Can the float recharge anywhere else? | No — the glider is the only charging station in the design. The float's own battery is the reserve that bridges any missed rendezvous (§6.5). |
| 5 | What happens during polar winter darkness? | Load-shedding keeps the vehicle alive at reduced cadence (§8.4, Fig. 18). Science continues at lower rate; profiles are stored on board and delivered when power allows. |
| 6 | How is the dock safe in rough seas? | The capture mechanism is compliant and fault-tolerant; docking is gated by a sea-state envelope (`SELL`, §6.6). If conditions exceed it, the pair separates and uses satellite fallback. |
| 7 | Who is liable for autonomous decisions? | The audit trail records whether each decision was made by the system or by a named operator (§11.2). The design principle is that major actions require human approval (§4.5). |
| 8 | Can this scale to many pairs? | Yes — the mission server, models and dashboard are multi-vehicle by design (`Mission 1 → * Vehicle` in §11.6). A second pair is a configuration change, not a rewrite. |
| 9 | What is the single most important gate in the project? | The Phase 2 exit gate: repeated successful rendezvous in real sea state with every fallback exercised (§16.2). Everything before it is preparation; everything after it inherits its confidence. |
| 10 | Where do I start as a new developer? | Read §1, §4, §6 first; then set up per §18.2; then run the simulator (§18.4) and watch a full 10-day cycle in fast time. |

### 18.11 Developer onboarding checklist

New engineers become productive in ~2 days following this path:

| Step | Task | Done when |
|---|---|---|
| 1 | Read §1, §4, §6, §7 | Can explain the rendezvous loop in your own words |
| 2 | Set up the dev environment (§18.2) | Dashboard + server running locally |
| 3 | Run the summer scenario in the simulator (§18.4) | Watched a full 10-day cycle in fast time |
| 4 | Read the schemas in `data/schemas/` | Can name every message in §4.7 |
| 5 | Run `make validate` in `ml/` | Understood coverage vs confidence |
| 6 | Pick a "good first issue" from the tracker | First PR merged (docs fixes count!) |
| 7 | Pair with an operator for a dashboard shift (if in season) | Completed one alert response drill |

**Onboarding rules**

- Ask questions in the project channel before inventing conventions — this README is the
  contract, but the team owns the interpretation.
- Never modify generated assets by hand: figures come from `scripts/gen_figures.py`, the README
  comes from `build/` via `scripts/splice_readme.py`.
- If a document and the code disagree, fix both in the same PR and flag the discrepancy in the
  review.

### 18.12 Release process

```mermaid
flowchart LR
    CODE["merged to main"] --> BUILD["CI build + full test suite"]
    BUILD --> TAG["version tag + SBOM + release notes"]
    TAG --> STAGE["deploy to staging · replay recent mission segment"]
    STAGE --> GATE{"staging verification<br/>+ operator sign-off"}
    GATE -- "no" --> FIX["fix + re-tag"] --> BUILD
    GATE -- "yes" --> PROD["deploy to production"]
    PROD --> ANNOUNCE["release announcement to ops channel<br/>+ update README version history"]
```

| Release type | Cadence | Approval |
|---|---|---|
| Server/dashboard patch | Continuous | Engineering lead |
| Model promotion | After validation gate (§7.5) | ML lead + science sign-off |
| Planner parameter change | Pre-planned windows | Operator + engineering lead |
| Firmware (vehicle) | Mission windows (rendezvous day, post-offload) | Engineering lead + ops on console |
| Mission config change | On planning reviews | Operator-approved, simulator-validated (§17.10) |

**Rollback rules.** Every production release has a tested rollback path; firmware rollback is
automatic on failed boot (§9.8); model rollback is a registry pointer flip (§7.9).

### 18.13 Development environment reference

A shared, reproducible development environment for every component:

| Component | Toolchain (proposed) | Editor/IDE notes |
|---|---|---|
| Server / ML / planner | Python 3.11 + venv; ruff + mypy | VS Code with Python, Pylance |
| Dashboard | Node 20, pnpm, Vite, ESLint + Prettier | VS Code with ESLint extension |
| Firmware | GCC cross-toolchain, CMake, OpenOCD, clang-tidy | VS Code + Cortex-Debug |
| Simulator | Python 3.11 + optional container | Jupyter for scenario notebooks |
| Docs & figures | Python/matplotlib; Mermaid CLI for diagram linting | Typora / VS Code markdown preview |

**Environment parity rules**

1. Every repo documents its toolchain versions in `CONTRIBUTING.md` + lockfiles (§17.7).
2. CI runs the same pinned versions as local development (no "works on my machine").
3. The simulator container doubles as the reference environment for the server and planner.
4. `scripts/gen_figures.py` and `scripts/splice_readme.py` are the only ways to change
   `assets/figures/` and `README.md` — hand-edits are rejected by the docs CI check.

---

## 19. Glossary

| Term | Meaning in this project |
|---|---|
| **Argo Float** | An autonomous profiling float that descends, drifts at depth, and rises while measuring the ocean, reporting at the surface on roughly a 10-day cycle. |
| **Wave Glider** | A surface autonomous vehicle propelled by wave motion via a tethered submerged fin rack, with solar-powered electronics. |
| **CTD** | Conductivity–Temperature–Depth sensor; conductivity is converted to salinity and pressure to depth. |
| **Buoyancy engine** | Pump and oil bladder that make a float rise or sink by changing its volume (and thus density). |
| **Rendezvous** | The planned meeting of glider and float at the surface to dock, transfer data and recharge. |
| **Checkpoint / waypoint** | A feasible intermediate location the glider is routed through on its way to the rendezvous zone. |
| **Reachable set / region** | The area the glider can realistically get to by a given time given waves, currents and its limited steering. |
| **Surfacing zone** | The probability ellipse (with uncertainty radius and confidence) where the float is expected to surface. |
| **Confidence score** | The estimated probability that the vehicle ends up within the predicted region. |
| **Ensemble forecast** | Running many simulations with slightly different conditions; the spread of outcomes quantifies uncertainty. |
| **Nowcast / forecast** | Best-estimate current state (nowcast) and predicted future state (forecast) of currents, weather and waves. |
| **Marginal ice zone** | The dynamic boundary region between open ocean and the pack ice, made of broken floes. |
| **Stand-off / exclusion zone** | A safety buffer around a detected hazard that routes are kept out of. |
| **Store-and-forward** | Buffering data on board until a communication link becomes available, then sending it. |
| **Manual override** | A human operator taking control or changing the plan, with commands uplinked via satellite. |
| **Load shedding** | Automatically reducing non-essential power consumers in a defined priority order when energy is short. |
| **Safe-hold** | A conservative vehicle state (minimal motion, protected data) entered on anomaly until resolved. |
| **HITL / SITL** | Hardware/Software In The Loop testing — running real code against simulated environments or rigs. |
| **Digital twin** | The simulator + dashboard's live model of the mission, mirroring the real vehicles' state. |
| **Coverage (validation)** | Fraction of held-out true surfacing points falling inside the predicted zone at the advertised confidence. |
| **QC flags** | Per-variable quality indicators following the Argo real-time QC convention. |
| **netCDF / CF** | Standard self-describing data format and Climate-Forecast metadata conventions used for ocean data. |
| **ADR** | Architecture Decision Record — a short document capturing a design decision and its rationale. |
| **SOC** | State of charge of a battery, in percent. |
| **AIS** | Automatic Identification System — ship transponder signals usable for hazard awareness. |
| **NCPOR** | National Centre for Polar and Ocean Research — the operating scientific organisation. |

---

## 20. Contributing, Team and References

### 20.1 Contributing

1. Read this README end-to-end — it is the contract every change must respect.
2. Open an issue or discussion before large changes; small fixes can go straight to a PR.
3. Every PR links to the blueprint section (and, where relevant, the concept-document section) it
   implements or changes.
4. Design changes update `docs/` (including ADRs) **in the same PR** as the code.
5. If you change the README's figures, regenerate them: `python3 scripts/gen_figures.py`.
6. Never commit credentials; use the secrets management agreed by the engineering team.

**PR checklist**

- [ ] Conventional-commit title
- [ ] Blueprint section(s) updated where the design changed
- [ ] Tests added/updated; CI green
- [ ] ML changes include a validation report
- [ ] Schema changes include a migration + compatibility note
- [ ] Figures regenerated if affected

### 20.2 Project team

| Role | Team |
|---|---|
| Mission owners & science direction | NCPOR scientists |
| Embedded, vehicles & deployment | Engineering team + on-site engineer |
| ML, planning & shore systems | Engineering / ML team (ashore) |
| Operations & oversight | Mission operators |

> *(Add named owners once the team roster is confirmed.)*

### 20.3 License and distribution

- License: **TBD** — internal to NCPOR at this stage; do not redistribute outside the project
  until a publication strategy is approved.
- Data: delivery to national archives and (by agreement) the Argo GDAC follows Argo data-policy
  conventions.

### 20.4 References

- Primary source: *Cooperative Polar-Ocean Observation System — Project Overview*, v1.0, 2026
  (`Polar_Ocean_Observation_System_Overview.pdf`).
- Diagrams (Figures 1–13) in the concept document are original schematics generated with Python;
  data curves and schedules are **illustrative placeholders**, not measured values.
- Concept illustrations (cover, docked rendezvous, hand-held debugger, dashboard mock-up) are
  **AI-generated visualisations** of the proposed system, not photographs of an existing product.
- The international Argo programme data and QC conventions inform §10.4.

### 20.5 Image credits

The photographs in `assets/images/` are **representative images** of the vehicle types,
instruments, vessels and environments described, used for educational and explanatory purposes.
Copyright remains with the original rights-holders. Sourced via web search; where a source could
be identified it is listed below.

| File | Subject | Source (as identified by search) |
|---|---|---|
| `wave_glider_at_sea.jpg` | Next-generation Wave Glider heading out to sea | Marine Technology News — photo: Liquid Robotics, a Boeing Company |
| `argo_float_deployment.jpg` | Argo float about to be deployed from a research vessel | Woods Hole Oceanographic Institution (WHOI) — floats & drifters page |
| `argo_float_deployment_2.jpg` | Researchers lowering a profiling float from a research ship | MBARI — APEX profiling floats page |
| `ctd_sensor_2.jpg` | Conductivity & temperature (CTD-class) sensor products | Ocean Science Technology supplier catalogue |
| `iceberg_a23a.jpg` | Iceberg A-23a drifting in the Southern Ocean | Live Science (Futurism/CDN imagery) |
| `iceberg_a23a_2.jpg` | A-23a rotating in the Southern Ocean | CNN / British Antarctic Survey (Emily Broadwell) editorial |
| `acc_currents.png` | Simplified schematic map of Southern Ocean currents | AntarcticGlaciers.org |
| `global_currents_map.jpg` | Global ocean-current map | Ocean Blue Project |
| `marginal_ice_zone.jpg` | Sea-ice marginal zone in front of the West Antarctic Ice Sheet | EurekAlert!/AAAS multimedia |
| `polar_vessel.jpg` | SA Agulhas II manoeuvring in icy waters | CNN / Frontiers in Marine Science editorial |
| `ocean_glider_launch.jpg` | NOAA ocean glider being prepared for launch | NOAA AOML (Atlantic Oceanographic & Meteorological Laboratory) |
| `weddell_ice_seals.jpg` | Crabeater seals on ice floes, Weddell Sea | Britannica — Weddell Sea entry |
| `southern_ocean_storm.jpg` | Ship crashing through Southern Ocean storm waves | Getty Images — Southern Ocean storm collection |
| `satellite_ground_station.jpg` | Large satellite ground-station antenna | Antesky earth-station antenna catalogue |
| `sagar_nidhi.jpg` | ORV Sagar Nidhi research vessel at sea | Wikipedia — ORV Sagar Nidhi |

> ⚠️ **For publication:** replace each plate with licensed or project-owned photography and record
> exact attributions and licences, per the concept document's own note (§17 of the PDF). All
> third-party images here are reproduced for illustrative, non-commercial concept communication
> within this project document.

---

## 21. Appendices

### Appendix A. Traceability to the concept document

Every section of this README maps to the v1.0 concept document as follows:

| README section | Concept document (PDF v1.0) | Primary figures |
|---|---|---|
| 1. Executive Summary | §1 Executive summary | — |
| 2. Why: The Southern Ocean Challenge | §2 Why we are doing this | Plates 2.1–2.4 |
| 3. The Concept at a Glance | §3 The concept at a glance | — |
| 4. System Architecture and End-to-End Flow | §5 System architecture & end-to-end flow | Figure 1 (PDF) ↔ 🖼️ 1, Mermaid #4 (data-path loop) |
| 5. The Two Vehicles | §4 The two vehicles | Figures 1–2 (PDF) ↔ 🖼️ 4–6 |
| 6. One Mission Cycle | §6 One mission cycle | Figure 5 (PDF) ↔ 🖼️ 3, Mermaid sequence |
| 7. The ML Brain | §7 Machine-learning brain | Figure 6 (PDF) ↔ 🖼️ 7–8 |
| 8. Planning, Ice Avoidance and Energy | §8 Checkpoints, rendezvous planning, ice avoidance & energy | Figures 7–9 (PDF) ↔ 🖼️ 9–12, 18 |
| 9. Embedded Software & Debugger | §9 Embedded software, health monitoring & debugger | Figure 10 (PDF) ↔ 🖼️ 13, 20 |
| 10. Data Streams and Products | §10 What we measure | Figures 11–12 (PDF) ↔ 🖼️ 14, 24 |
| 11. Web Mission Dashboard | §11 Web-based mission monitoring platform | Plate 11.1 (PDF) ↔ 🖼️ 21 |
| 12. Iceberg Tracker | §12 Optional extension | ↔ 🖼️ 22 |
| 13. Deployment and a Year in the Field | §13 Deployment and a year in the field | Figure 13 (PDF) ↔ 🖼️ 15 |
| 14. Expected Benefits and Impact | §14 Expected benefits and impact | — |
| 15. Risk Register and Mitigations | §15 Risks, challenges & responses | ↔ 🖼️ 16 |
| 16. Phased Development Roadmap | §15 (phased development note) | ↔ 🖼️ 17 |
| 17–18. Repository Blueprint & Getting Started | *New in this README — engineering blueprint* | 🖼️ 19 (auxiliary) |
| 19. Glossary | §16 Glossary | — |
| 20. References and Credits | §17 Image credits & notes | — |

### Appendix B. Figure index

All figures are generated by `python3 scripts/gen_figures.py` into `assets/figures/`.
Regenerate and commit whenever the underlying design changes.

| # | File | Content | Section |
|---|---|---|---|
| 1 | `fig_01_system_architecture.png` | System architecture — sea / satellite / shore tiers | §4.1 |
| 2 | `fig_02_operational_flow.png` | End-to-end operational flow with feedback loop | §4.2 |
| 3 | `fig_03_mission_timeline.png` | One ~10-day cycle as a four-lane timeline | §6.1 |
| 4 | `fig_04_waveglider_propulsion.png` | Wave Glider propulsion mechanics | §5.1 |
| 5 | `fig_05_argo_cycle.png` | Argo 10-day depth-time cycle | §5.2 |
| 6 | `fig_06_buoyancy_engine.png` | Buoyancy engine: oil in/out | §5.2 |
| 7 | `fig_07_ml_models.png` | The two ML models feeding the planner | §7.1 |
| 8 | `fig_08_uncertainty.png` | Uncertainty envelopes, ellipses, no-go polygons | §7.1 |
| 9 | `fig_09_planning_loop.png` | Adaptive planning loop | §8.1 |
| 10 | `fig_10_checkpoints.png` | Checkpoint routing around an ice hazard | §8.1 |
| 11 | `fig_11_ice_handling.png` | Three-step ice-hazard handling | §8.3 |
| 12 | `fig_12_energy_flow.png` | Energy flow: waves + sunlight | §8.4 |
| 13 | `fig_13_software_stack.png` | Embedded software stack, both vehicles | §9.1 |
| 14 | `fig_14_data_products.png` | Illustrative science products (2×2) | §10.2 |
| 15 | `fig_15_year_plan.png` | A year in the field: daylight, ice, cycles | §13.2 |
| 16 | `fig_16_risk_matrix.png` | 5×5 risk matrix | §15.2 |
| 17 | `fig_17_gantt.png` | Phased development Gantt | §16 |
| 18 | `fig_18_solar_soc.png` | Seasonal solar yield + battery SOC simulation | §8.4 |
| 19 | `fig_19_rendezvous_prob.png` | Rendezvous probability & zone shrinkage | §7 (auxiliary) |
| 20 | `fig_20_debugger_gate.png` | Pre-deployment debugger quality gate | §9.4 |
| 21 | `fig_21_dashboard.png` | Dashboard wireframe | §11 |
| 22 | `fig_22_tracker.png` | Iceberg tracker data flow | §12 |
| 23 | `fig_23_comms.png` | Communications matrix diagram | §4.3 |
| 24 | `fig_24_data_pipeline.png` | Shore-side data pipeline | §10.3 |
| 25 | `fig_25_sensor_payload.png` | Sensor payload map (both platforms) | §5.9 |
| 26 | `fig_26_prediction_worked_example.png` | Zone shrinkage + ensemble (worked example) | §7.15 |
| 27 | `fig_27_annual_energy_budget.png` | Annual energy production vs consumption | §8.12 |
| 28 | `fig_28_data_latency_budget.png` | Ocean-to-archive latency by delivery path | §10.11 |
| 29 | `fig_29_dashboard_flow.png` | Dashboard layout + screen flow | §11.11 |
| 30 | `fig_30_impact_pathways.png` | Impact pathways to societal benefit | §14.9 |

### Appendix C. Mermaid diagram index

| # | Diagram type | Content | Section |
|---|---|---|---|
| 1 | `mindmap` | Design principles | §3.2 |
| 2 | `mindmap` | System overview | §3.4 |
| 3 | `flowchart` | Architecture tiers (sea/satellite/shore) | §4.1 |
| 4 | `flowchart` | Six-step data path loop | §4.2 |
| 5 | `flowchart` | Communications matrix | §4.3 |
| 6 | `quadrantChart` | Why the pairing works | §5.4 |
| 7 | `sequenceDiagram` | Rendezvous protocol (autonumbered) | §6.2 |
| 8 | `stateDiagram-v2` | Mission states & transitions | §6.4 |
| 9 | `flowchart` | Ensemble forecasting | §7.4 |
| 10 | `flowchart` | Model lifecycle & retraining | §7.6 |
| 11 | `flowchart` | Planning loop | §8.1 |
| 12 | `flowchart` | Ice-hazard handling | §8.3 |
| 13 | `flowchart` | Energy flow | §8.4 |
| 14 | `flowchart` | Embedded software layers + shared core | §9.1 |
| 15 | `flowchart` | Health-monitoring loop | §9.3 |
| 16 | `flowchart` | Debugger checklist flow | §9.4 |
| 17 | `flowchart` | Data lifecycle | §10.3 |
| 18 | `flowchart` | Alert triage & override | §11.2 |
| 19 | `flowchart` | Tracker data flow | §12.2 |
| 20 | `flowchart` | Deployment flow | §13.1 |
| 21 | `flowchart` | Seasonal operations logic | §13.3 |
| 22 | `timeline` | Phased roadmap | §16 |
| 23 | `flowchart` | Phase dependencies | §16.3 |
| 24 | `gitGraph` | Git workflow | §17.5 |
| 25 | `flowchart` | CI/CD pipeline | §18.6 |
| 26 | `erDiagram` | Shore database entities & relations | §11.6 |
| 27 | `classDiagram` | Domain model with cardinalities | §11.6 |
| 28 | `flowchart` | Release process | §18.12 |
| 29 | `flowchart` | Sensor-to-sample pipeline | §5.9 |
| 30 | `flowchart` | Prediction pipeline (worked example) | §7.15 |
| 31 | `flowchart` | Energy decision ladder | §8.12 |
| 32 | `stateDiagram-v2` | Dashboard screen navigation | §11.11 |
| 33 | `sequenceDiagram` | A day in the life — ops interactions | §13.13 |
| 34 | `flowchart` | Impact pathways chain | §14.9 |

### Appendix D. File manifest

| Path | Purpose | Regenerate with |
|---|---|---|
| `README.md` | This blueprint | — |
| `assets/figures/*.png` | 30 blueprint figures | `python3 scripts/gen_figures.py` + `scripts/gen_figures_v3.py` |
| `assets/images/*` | Illustrative photography | Web search (see §20.5) |
| `scripts/gen_figures.py` | Figure-runner | — |
| `scripts/gen_figures_part1.py` | Figures 1–13 | — |
| `scripts/gen_figures_part2.py` | Figures 14–24 | — |
| `uploads/Polar_Ocean_Observation_System_Overview.pdf` | Source concept document v1.0 | — |
| `scripts/gen_figures_v3.py` | Figures 25–30 | — |
| `modules/*.md` | V3 expansion modules + pristine base | — |
| `scripts/splice_v3.py` / `scripts/restore_v3.py` | V3 build pipeline (restore = pristine, splice = expanded) | — |

### Appendix E. Revision history

| Version | Date | Changes |
|---|---|---|
| 1.0 | 2026 | README blueprint created from the concept document (text + tables) |
| 2.0 | 2026-09-11 | Added: 24 Python-generated figures, 28 Mermaid diagrams, 11 Google-sourced photographs, expanded sections (§2.5 science questions, §4.4–4.6 resilience/security/interfaces, §7.4–7.6 ensemble/lifecycle, §10.4–10.5 formats/example, §11.3–11.4 RBAC/tech, §13.5 playbook, §14.4 KPIs, §17 module contracts/standards, §18.7 FAQ), appendices A–E |
| 2.1 | 2026-09-11 | Added: 6 new Python figures (25–30), 6 new Mermaid diagrams (34 total), 5 new photographs (15 total) — §5.9 sensor payload, §7.15 worked prediction example, §8.12 annual energy budget, §10.11 data latency budget, §11.11 dashboard walkthrough, §13.13 day-in-the-life vignettes, §14.9 impact pathways, Appendices T–U |

### Appendix F. KPI definitions and formulas

| KPI | Formula (illustrative) | Source tables (§11.6) |
|---|---|---|
| Profile delivery rate | `count(PROFILES with outcome ∈ {delivered, delivered_late}) / count(CYCLES)` | PROFILES, CYCLES |
| QC pass rate (real-time) | `count(PROFILE_LEVELS with qc ∈ {1,2}) / count(levels)` | PROFILE_LEVELS |
| Rendezvous success rate | `count(RENDEZVOUS_EVENTS with outcome = success) / count(attempted)` | RENDEZVOUS_EVENTS |
| Data-loss rate | `1 − count(profiles delivered within 2 cycles) / count(profiles collected)` | PROFILES |
| Prediction coverage | `count(true surfacing ∈ zone) / count(predictions)` vs advertised confidence | PREDICTIONS, RENDEZVOUS_EVENTS |
| Mean zone radius | `mean(radius_km)` at each horizon | PREDICTIONS |
| Stand-off violation rate | `count(vehicle-inside-standoff events) / days` — target 0 | EVENT_LOGS, HAZARDS |
| Vehicle loss rate | `count(lost vehicles) / vehicle-years` — target 0 | VEHICLES |
| Energy self-sufficiency | `count(recharges) / count(rendezvous)` | RENDEZVOUS_EVENTS |
| Uptime of dashboard | Standard availability measure, target ≥ 99.5 % during ops | server metrics |

### Appendix G. Acronyms

| Acronym | Expansion |
|---|---|
| ACC | Antarctic Circumpolar Current |
| ADR | Architecture Decision Record |
| AIS | Automatic Identification System |
| ASV | Autonomous Surface Vehicle |
| BGC | Biogeochemical (sensors) |
| CDOM | Coloured Dissolved Organic Matter |
| CF | Climate and Forecast (metadata conventions) |
| COG / SOG | Course / Speed Over Ground |
| CRC | Cyclic Redundancy Check |
| CTD | Conductivity–Temperature–Depth |
| DVL | Doppler Velocity Log |
| GDAC | Global Data Assembly Centre (Argo) |
| GNSS | Global Navigation Satellite System |
| HITL / SITL | Hardware / Software In The Loop |
| IMU | Inertial Measurement Unit |
| KPI | Key Performance Indicator |
| MIZ | Marginal Ice Zone |
| MPPT | Maximum Power Point Tracking |
| NCPOR | National Centre for Polar and Ocean Research |
| OIDC | OpenID Connect |
| QC | Quality Control |
| RBAC | Role-Based Access Control |
| RSSI | Received Signal Strength Indicator |
| SBOM | Software Bill of Materials |
| SOC | State of Charge |
| SST | Sea-Surface Temperature |
| TSDB | Time-Series Database |

### Appendix H. Simulator scenario catalog

| Scenario | Duration | Conditions | Purpose |
|---|---|---|---|
| `southern-ocean-summer` | 10 days | Open water, calm → storm | Baseline rendezvous chain |
| `winter-ice-advance` | 20 days | Advancing MIZ, low solar | Ice re-planning + load-shedding |
| `rendezvous-failures` | 30 days | Injected: lock fault, comm outage, late glider | Fallback ladder (§6.5) |
| `prediction-stress` | 30 days | Weak currents forecasts, chaotic drift | ML robustness + coverage |
| `tracker-live` | 15 days | Iceberg on collision-adjacent drift | Tracker → planner integration (§12) |
| `full-season` | 365 days | Seasonal daylight + ice cycles | Year-level energy and ops (§13) |
| `operator-drill` | 6 days | Scripted alert cascade | Dashboard + override training |
| `schema-fuzz` | — | Malformed frames, wrong versions | Server quarantine behaviour (§4.8) |

Each scenario outputs the same artefacts the real system produces — plans, predictions, alerts,
logs, profiles — so staging can replay a mission end-to-end before any real deployment.

---

### Appendix I. Sign-off record

| Role | Name | Date | Signature |
|---|---|---|---|
| Science lead (NCPOR) | *(pending)* | | |
| Engineering lead | *(pending)* | | |
| Operations lead | *(pending)* | | |
| Programme reviewer | *(pending)* | | |

### Appendix J. Design-review checklist

Use this checklist at every phase gate (§16.2). Every "no" blocks the gate until resolved.

| # | Area | Question |
|---|---|---|
| 1 | Requirements | Does the design still satisfy the twelve elements (§3.1) and the objectives (§1.4)? |
| 2 | Safety | Is every failure mode in the register (§15.1) and catalog (§15.4) answered by a tested response? |
| 3 | Fail-safe | Does any path force the glider into ice, or hold science data hostage? (§8.3, §6.3) |
| 4 | Energy | Does the winter worst-week simulation (Fig. 18) close? What is the SOC floor? |
| 5 | Prediction honesty | Do models report coverage ≈ confidence on held-out floats? (§7.5) |
| 6 | Comms fallback | Are all three fallback paths (glider relay, float burst, store-and-forward) exercised this phase? |
| 7 | Data integrity | Are schemas versioned and contracts tested? Can a profile be traced from CTD to archive? |
| 8 | Security | Are command channels authenticated and audited? Are keys provisioned at the debugger gate? |
| 9 | Human factors | Can an operator complete the §13.5 playbook through the dashboard under time pressure? |
| 10 | Operations | Is the runbook (§13.6) current, and has the on-site engineer signed the last debugger record? |
| 11 | Reproducibility | Can a new developer rebuild the figures, models and simulator from the repo alone? (§18) |
| 12 | Documentation | Do ADRs, schemas and this README match the code under review? |

**Review board**: science lead, engineering lead, operations lead, and one external reviewer for
P3/P4 gates.

### Appendix K. Further reading

| Topic | Reference |
|---|---|
| Argo programme | International Argo programme documentation — float design, cycle, data format and QC conventions |
| Wave Gliders | Liquid Robotics / Boeing Wave Glider technical literature (representative platform) |
| Profiling float engineering | Argo float manufacturer manuals (APEX, ARVOR, Navis) — buoyancy engines and CTD integration |
| Southern Ocean science | ACC dynamics, Antarctic Bottom Water formation, marginal-ice-zone process studies |
| Sea-ice operations | National ice services' sea-ice chart products and iceberg bulletins |
| Probabilistic forecasting | Ensemble and Monte-Carlo forecasting; proper scoring rules (CRPS) for distribution forecasts |
| Ocean ML | Ocean drift-prediction literature — physics-informed neural networks for Lagrangian prediction |
| Robotics standards | Maritime autonomy and COLREG awareness literature for surface vehicles |
| Data standards | CF conventions, netCDF, Argo real-time QC manual |

> Bibliographic details are compiled as the technical library is built during Phase 1; this
> appendix lists topic areas, not a frozen citation list.

### Appendix L. Requirements traceability matrix

Requirements (REQ) are traced to the twelve elements (§3.1), to design sections, and to
verification methods. This is the spine for audits and phase-gate reviews.

| REQ | Requirement (condensed) | Element | Design | Verified by |
|---|---|---|---|---|
| R-01 | Operate glider + float as one cooperative mission | E1 | §4.1, §6 | Simulator scenarios T-01…T-10 (§8.9); coastal trials |
| R-02 | Deliver ≥ 30 QC-passing profiles/year | E2 | §5.2, §10 | Season KPI (§14.4) |
| R-03 | Continuous surface met record, ≥ 95 % scheduled uptime (energy-adjusted) | E3 | §5.1, §8.4 | Archive completeness; Fig. 14b |
| R-04 | Predict glider trajectory with reachable set + envelope | E4 | §7.2, §7.7 | Held-out validation (§7.5) |
| R-05 | Predict surfacing zone with radius + confidence | E5 | §7.3, §7.8 | Coverage ≈ confidence on held-out floats |
| R-06 | Generate feasible checkpoints, re-planned on new data | E6 | §8.1, §8.5 | Planner property checks; T-01…T-10 |
| R-07 | Maximise P(rendezvous) under constraints | E7 | §8.2, §8.6 | Rendezvous scorecard (§6.8) |
| R-08 | Zero ice stand-off violations | E8 | §8.3, §8.7 | Vehicle logs; T-03/T-06 |
| R-09 | Dock, offload, recharge, release — data first | E9 | §6.3, §6.6 | Tank (P1) + coastal trials (P2) |
| R-10 | Health monitoring with layer-coded fault localisation | E10 | §9.2, §9.5 | Fault injection (chaos drills §17.8) |
| R-11 | Pre-deployment debugger gate blocks bad launches | E10 | §9.4 | Debugger records audited at each deployment |
| R-12 | Live dashboard with alerts + audited override | E11 | §11.1–11.3 | Usability acceptance tests (§11.7) |
| R-13 | Degraded-mode dashboard during outages | E11 | §11.1, §11.7 | Simulated outage test |
| R-14 | Optional tracker feeds polygons to the planner | E12 | §12.2 | `tracker-live` scenario (§18.4, Appendix H) |
| R-15 | No profile loss under any single failure | — | §6.5 | Failure-drill scenarios; season statistics |
| R-16 | Command channel authenticated + audited | — | §4.5 | Security review; E-4 drill |
| R-17 | Models validated before guiding vehicles | — | §7.5–7.6 | Validation reports attached to every model PR |
| R-18 | Data in Argo-standard formats and QC | — | §10.4, §10.6 | Archive acceptance |
| R-19 | One ship visit per season | — | §13.1 | Ops records |
| R-20 | Reproducible build of docs, figures, models, simulator | — | §17, §18 | Clean-room rebuild test at each gate |

**Traceability rules**

1. Every requirement has exactly one owning work package (WP-xx, §16.4).
2. No design section exists without a requirement it serves — if one appears, either add the
   requirement or delete the section.
3. Verification evidence (test reports, scorecards, validation reports) is archived and linked
   from the phase-gate review record (§16.2).

---

### Appendix M. Interface control summary

| Interface | Between | Controlled by | Change process |
|---|---|---|---|
| IC-01 Dock mechanical/electrical | Float ⇄ Glider | WP-02 | Prototype freeze at P1; change board after |
| IC-02 Short-range RF | Float ⇄ Glider | WP-04/05 | Schema-versioned firmware releases |
| IC-03 Satellite air interface | Vehicles ⇄ constellation | Provider | Provider-managed; monitored by ops |
| IC-04 Shore gateway ⇄ mission server | Infrastructure ⇄ WP-10 | WP-10 | CI-tested adapter |
| IC-05 Mission server ⇄ dashboard | WP-10 ⇄ WP-11 | WP-10 | OpenAPI spec, contract tests in CI |
| IC-06 Planner ⇄ ML models | WP-09 ⇄ WP-07/08 | ML team | Artefact contract (§7.11) |
| IC-07 Env feeds ⇄ planner | Providers ⇄ WP-09 | WP-09 | TTL/staleness policy (§7.10) |
| IC-08 Archive ⇄ data centres | WP-17 ⇄ centres | WP-17 | Agreed at P2, tested end-to-end |

Each IC has a one-page control document in `docs/decisions/` recording the owner, the frozen
version, and the change procedure.

### Appendix N. Sample season dataset (excerpts)

An illustrative end-to-end trace of one cycle's data, in the formats the real system produces.
*(Synthetic values — shape only, not measurements.)*

**N.1 Surfacing-zone prediction artefact (Model 2, cycle 17)**

```json
{
  "model_version": "float_surfacing_v3.1",
  "valid_from_utc": "2026-11-09T06:00:00Z",
  "valid_to_utc": "2026-11-12T06:00:00Z",
  "geometry": {
    "type": "Polygon",
    "coordinates": [[[-62.42, 34.14], [-62.38, 34.30], [-62.41, 34.26], [-62.45, 34.19], [-62.42, 34.14]]]
  },
  "uncertainty_radius_km": 18,
  "confidence": 0.80,
  "window": ["2026-11-12T03:30:00Z", "2026-11-12T05:30:00Z"],
  "n_members": 200,
  "inputs_freshness": { "currents_h": 6, "ice_chart_h": 22 },
  "degraded": false
}
```

**N.2 Plan checkpoint list (cycle 17)**

```json
{
  "plan_id": "PLN-2026-11-09T06Z-4",
  "generated_utc": "2026-11-09T06:00:00Z",
  "objective": { "p_success": 0.81, "expected_energy_wh": 340, "risk_score": 0.02 },
  "checkpoints": [
    { "index": 1, "lat": -62.05, "lon": 33.40, "eta": "2026-11-09T12:00Z", "type": "transit" },
    { "index": 2, "lat": -62.18, "lon": 33.75, "eta": "2026-11-10T00:00Z", "type": "transit" },
    { "index": 3, "lat": -62.31, "lon": 34.05, "eta": "2026-11-10T18:00Z", "type": "transit" },
    { "index": 4, "lat": -62.40, "lon": 34.20, "eta": "2026-11-11T06:00Z", "type": "loiter",
      "loiter_until": "2026-11-12T03:30Z" }
  ],
  "hazards_considered": ["A-23a (tracked)", "MIZ 2026-11-09 chart"],
  "fallbacks": ["extend_surface_window", "float_direct_burst"]
}
```

**N.3 Profile header (as archived netCDF, attributes only)**

```text
:platform = "NCPOR-FLT-001" ;  :cycle_number = 17 ;
:profile_id = "NCPOR-FLT-001-017" ;
:date_surface = "2026-11-12T03:58:00Z" ;
:lat = -62.418 ; :lon = 34.207 ;
:retrieval_method = "glider_offload" ;
:calibration_ref = "CAL-2026-003" ;
:qc_version = "pipeline v1.4" ;  :data_mode = "R" ;
```

**N.4 Rendezvous event record (scorecard inputs, §6.8)**

```json
{
  "cycle": 17,
  "arrival_offset_h": -21.5,
  "zone_containment": true,
  "dock_attempts": 1,
  "offload_bytes": 98422,
  "offload_complete": true,
  "wh_delivered": 6.1,
  "float_soc_after_pct": 78,
  "window_used_min": 74,
  "outcome": "success"
}
```

**N.5 QC summary line (per level, cycle 17)**

```text
LEVELS=986  GOOD=978  PROBABLY_GOOD=6  PROBABLY_BAD=2  BAD=0  FLAGGED_BY="QC-01 QC-03"
```

### Appendix O. Review meeting calendar

| Meeting | Cadence | Attendees | Purpose |
|---|---|---|---|
| Daily ops stand-up (in season) | Daily, 15 min | Operators, eng on-call | Alerts, plan status, anomalies |
| Engineering sync | Weekly | Engineering, ML | Scorecards (§6.8), tuning, backlog |
| Science working group | Bi-weekly | Scientists, ML | Data quality, sampling priorities |
| Prediction-skill review | Monthly | ML, science, ops | Coverage/radius trends (§7.10) |
| Risk board | Per phase + on L3 events | All leads | Register update (§15), mitigation status |
| Phase-gate review | At each gate (§16.2) | Board + external reviewer | Go / no-go with the checklist (§Appendix J) |
| Season closeout | End of season | All | §13.8 checklist, reports, next-season plan |

### Appendix P. Definition of Done

A work item is *done* only when all of the following hold — at every phase, not just the last:

| # | Criterion | Evidence |
|---|---|---|
| 1 | Code implements the requirement it claims (Appendix L traceability) | Linked requirement in PR |
| 2 | Tests cover the behaviour, including failure paths | Test report in CI |
| 3 | Documentation matches the code (README/ADRs/schemas updated in the same PR) | Doc diff in PR |
| 4 | No open regression in the simulator scenario suite | CI green |
| 5 | For ML: validation report attached, gate passed (§7.5) | Report in PR |
| 6 | For firmware: HITL scenario passed; watchdog-aware review done | HITL log + review |
| 7 | For schemas: compatibility lint passed; migration documented | CI + migration note |
| 8 | Security-sensitive changes reviewed by a second approver | Review record |
| 9 | The item's risk-register row (if any) is updated | Register diff |
| 10 | Handover note written for operators (if behaviour changed) | Note in release (§18.12) |

### Appendix Q. Change-log template

Every release appends to this table (mirrors Appendix E's document-level history):

| Release | Date | Type | Component(s) | Changes | Validation | Rollback |
|---|---|---|---|---|---|---|
| v0.3.1 | 2026-xx-xx | patch | server | fixed alert dedupe | unit + contract CI | automatic |
| v0.4.0 | 2026-xx-xx | minor | planner | winter stand-off policy | simulator T-06, T-10 | config revert |
| m-float-v2 | 2026-xx-xx | model | surfacing model | ensemble spread +5 % | coverage 81 % @ 80 % | registry pointer |
| fw-wg-1.2 | 2026-xx-xx | firmware | glider | charge curve fix | HITL R-2 replay | A/B boot slot |

**Release types:** `patch` (fix, no behaviour change) · `minor` (behaviour change, backward
compatible) · `model` (ML promotion) · `firmware` (vehicle software) · `config` (mission
parameters). Breaking changes are `major` and require a design review before implementation.

### Appendix R. One-page quick reference card

The card every reviewer, operator and visitor receives. *(One page; all values illustrative.)*

```text
COOPERATIVE POLAR-OCEAN OBSERVATION SYSTEM — QUICK REFERENCE
─────────────────────────────────────────────────────────────
WHAT      A Wave Glider + Argo Float, paired as ONE system, for a
          year-round Southern Ocean season without a ship.
WHERE     Indian Ocean sector, ~55°S–70°S (ice-dependent box, §2.7).
WHY       Winter fluxes + deep profiles where nobody measures (§2).
HOW       Every 10 days: float dives 2,000 m → ML predicts surfacing
          zone (§7) → glider meets it → data offload + recharge → repeat.
BACKUP    If they miss: float bursts essentials via satellite (§6.5).
WINTER    Slower, quieter, fewer passes — data first, never risk (§13.10).
ICE       Avoidance, never armour: charts + tracker + stand-offs (§8.3).
ML        Ensemble surfacing forecast + drift model; coverage vs
          confidence is the contract (§7.11).
CONTROL   Shore planner proposes, operators supervise, audit trail
          records everything (§9, §11).
DATA      Argo-grade QC, netCDF/CF archive, provenance throughout
          (§10). >25,000 profiles + full year of surface met.
PEOPLE    NCPOR team + simulator-trained operators + ship visit per
          season (§13, §14.7).
RISK      Top risks: ice, winter energy, docking at sea, comms (§15).
          Everything fails safe; nothing that fails loses the data.
ROADMAP   P1 shore build → P2 coastal trials → P3 polar pilot →
          P4 full season, gated at every step (§16).
─────────────────────────────────────────────────────────────
VERSION 2.0 · 2026 · prepared for NCPOR — all values illustrative.
```

### Appendix S. Ten open questions the mission is designed to answer

The science questions of §2.5 are the *targets*; the ten below are the *frontier* — the mission
is instrumented so that each can be answered from its data, now or by a successor mission.

| # | Open question | Where the answer will come from |
|---|---|---|
| 1 | How do winter air–sea fluxes in the Indian sector compare with reanalysis products? | Glider met record through winter (§5.1) |
| 2 | What is the true drift (speed, direction, variability) of the 1,000–2,000 m currents here? | Every float cycle is a Lagrangian experiment (§2.5 Q3) |
| 3 | How well does machine-learned drift prediction generalise across seasons? | Skill report after a full season (§7.10) |
| 4 | What is the energy budget of a solar-plus-wave surface vehicle through polar winter? | Glider power telemetry + energy model (Fig. 18) |
| 5 | Can a recharge rendezvous work reliably in the MIZ? | P3 pilot + P4 docking record (§6.8) |
| 6 | How much does co-located surface forcing improve interpretation of Argo profiles? | Paired glider-float records (§10.2) |
| 7 | What is the hazard footprint of a large tabular iceberg, operationally? | Optional tracker + planner logs (§12) |
| 8 | What is the true cost per winter profile, delivered autonomously? | Programme cost accounting vs profile count (§14.5) |
| 9 | Which autonomy failures are predictable and which are novel? | Lessons-learned register (§15.5) |
| 10 | Can two small vehicles scale to a fleet? | Everything in this README is written to be reused at N × (§16.6) |

> The mission is a *hypothesis test* with hardware: if these ten questions get answers, the
> programme has succeeded even if every individual prediction missed a cycle.

### Appendix T. Worked numerical ledgers

The quantitative backbone of §8.12, §10.11 and §14.5, collected in one place for reviewers.
*(All values illustrative — planning figures, not commitments.)*

**T.1 Energy ledger (illustrative season totals)**

| Item | Value | Basis |
|---|---|---|
| Total production | ≈ 214 kWh/season | Solar ≈ 123 kWh + wave ≈ 91 kWh (monthly rows in §8.12) |
| Total consumption | ≈ 181 kWh/season | Propulsion ≈ 45 %, comms ≈ 20 %, science ≈ 15 %, charging float ≈ 20 % |
| Winter margin (Jun–Aug) | ≈ +7.4 kWh | The sizing case for battery capacity and shed-ladder design |
| Reserve at winter solstice | ≈ 58 % SOC target | Set so SAFE HOLD alone survives 21 days |
| Float recharge delivered per rendezvous | 5–8 Wh (nominal 6.1 Wh) | §6.3 charge curves; scorecard field `wh_delivered` |
| Cost of one missed rendezvous | ≈ 0 Wh but ~2.5 Wh later | Replanning energy; the real cost is data latency (§10.11) |

**T.2 Data ledger (per cycle, nominal)**

| Source | Bytes (nominal) | Path | Arrival |
|---|---|---|---|
| Full profile (CTD + BGC, 1 m bins) | 84–110 KB | Rendezvous offload | +10 days nominal (§10.11) |
| Decimated profile (essentials) | 3–5 KB | Direct burst | +1 day |
| Met series (10 days × 1-min) | ~300 KB | Relay after rendezvous | +10 days |
| Health + telemetry | ~20 KB | Every pass | ≤ 1 day |
| Commands / configs (downlink) | ~2 KB | Per pass | ≤ 1 day |
| **Season total to archive** | **< 120 MB** | — | Trivially archivable (§10.8) |

**T.3 Rendezvous scorecard scenarios** (the five §6.8 columns, five ways)

| Scenario | Arrival offset | Zone containment | Dock attempts | Outcome | Charge |
|---|---|---|---|---|---|
| Nominal | −21.5 h | ✓ | 1 | success | 6.1 Wh |
| Storm-extended window | −9 h (window +12 h) | ✓ | 1 | success | 4.8 Wh |
| Late arrival | +3.5 h | ✓ (float waited) | 1 | success | 3.9 Wh |
| Missed, burst fallback | — | ✗ | 0 | no-meet | 0 Wh |
| Polynya winter meet | −40 h (gap 40 days) | ✓ | 2 | success | 7.4 Wh |

**T.4 Cost-per-profile worked example**

```text
Season programme cost (all phases, illustrative)   ≈ ₹ / $ X
Profiles delivered (30 cycles × ~0.98 success)     ≈ 29
Cost per full-depth winter profile                 ≈ X / 29
Glider met record cost per day                     ≈ X / 365
Ship-day equivalent (one polar cruise)             ≈ Y days of ship time
→ Autonomy delivers the same profile for a fraction of the ship cost,
  and the fraction is the headline of the §14.5 impact report.
```

> The ledgers are reviewed quarterly (Appendix O): when a real season's numbers replace these
> illustrative ones, every downstream claim in §14 gets re-anchored to them.

### Appendix U. Season simulation trace (condensed)

One full season, 36 float cycles, compressed to the moments that matter. This trace is the
"canonical season" the simulator replays (§18.4) and the planner test bank samples (§8.9).
*(Synthetic values — shape, not measurements.)*

| Cycle | Day | Event | System response | Outcome |
|---|---|---|---|---|
| 1 | 1 | Deployment from ship; float dives, glider transits | Commissioning mode; debugger report signed (§9.4) | First profile scheduled |
| 2 | 11 | First rendezvous attempt | Full sequence rehearsed at sea (§6.9 R-4) | Success, 5.8 Wh, 92 KB |
| 4 | 31 | Storm during surfacing window | Window extended; stand-off 30 km (§13.13 V1) | Success at +9 h |
| 8 | 71 | Ice tongue crosses checkpoint 3 | Emergency replan in 15 s (§8.11) | Detour, +40 Wh |
| 12 | 111 | Solar peak; reserve building | Charge float to 80 % SOC target | Reserve at 62 % |
| 17 | 161 | Canonical worked prediction (Appendix N, §7.15) | Zone 54 → 11 km; coverage 0.84 | Success |
| 20 | 191 | Winter solstice passes | Winter doctrine active (§13.10); 1 pass/day | Routine |
| 24 | 231 | Midwinter rendezvous after 40-day gap | Polynya routing; data-first offload | 7.4 Wh, full backlog |
| 28 | 271 | Glider sensor flag (wind vane) | Degraded mode; QC flag QC-08 (§10.7) | Science continues |
| 31 | 301 | Missed window (ice too heavy) | Burst fallback; essentials archived | No-meet, no loss |
| 34 | 331 | Light returns; shed ladder descends | Full cadence restored automatically | Normal ops |
| 36 | 361 | Recovery; ship visit complete | Closeout checklist (§13.8); wrap-up report (§13.12) | Season complete |

**What the trace teaches** (the three lessons every operator quotes):

1. **The season is mostly routine.** 36 cycles, 2 genuinely dangerous moments (cycles 4, 31) —
   the system's job is to make the other 34 boring (§6.5 "inconvenience, not loss").
2. **The winter is won in summer.** Cycles 12–17 built the reserve that cycles 24–31 spent.
3. **Every anomaly becomes training data.** Cycles 4, 8 and 31 each produced a lessons-learned
   entry (§15.5) and at least one planner test (§8.9) so the next season meets them prepared.

---

<p align="center">
  <em>Cooperative Polar-Ocean Observation System — blueprint & documentation</em><br>
  <em>Version 2.1 · 2026 · Prepared for NCPOR scientists, engineers and reviewers</em><br>
  <em>A fuel-free, wave-and-solar-powered surface Wave Glider and a diving Argo Float, paired as one
  intelligent, self-recharging observation mission — predicting their drift through the Southern
  Ocean, meeting at the surface to hand over data and power, avoiding ice, and streaming the polar
  ocean and atmosphere to scientists ashore.</em><br>
  <em>All quantitative values illustrative unless stated otherwise.</em>
</p>
