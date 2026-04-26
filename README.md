# Hospital-to-Home Transitions: A Multi-Agent Framework for Safe Post-Discharge Care

A multi-agent system that supports patients during the high-risk 30-day window after hospital discharge. Three specialist agents (a Schedule Planner, Symptom Evaluator, and Insurance Liaison) coordinate through a deterministic Orchestrator to translate discharge instructions into a structured care plan, monitor daily symptoms, detect deterioration trajectories, and escalate to a human nurse when needed. The system was designed and evaluated for one specific patient: Margaret, a 72-year-old recovering from coronary artery bypass surgery (CABG) at UPMC.

## Team
- Anshika Shukla
- Nicole Sanna
- Nicole Zhou
- Yuhang Zhou

Course: 94815 Agentic Technologies, CMU Heinz College

## Track B — Applied Agent Experience.

## How to access the project

This project does not require local setup. All artifacts are accessible directly:

- **Interactive prototype:** open `media/figma_prototype_link.txt` for the Figma Make link. The prototype is fully clickable across the patient journey from login through daily check-in, severity routing, and escalation. The staff dashboard is reachable via demo-mode role switch on the home screen.
- **Final report:** `docs/final_report.pdf`
- **Architecture diagram:** `docs/architecture_diagram.pdf`
- **5-minute project video:** `media/demo_video_link.txt`
- **Evaluation results:** `eval/test_cases.csv`, `eval/evaluation_results.csv`, `eval/failure_log.md`

## Required platforms

No code execution is required to review this project. The artifacts depend on:

- A modern web browser (for the Figma prototype link)
- A PDF reader (for the report and architecture diagram)
- Spreadsheet or text viewer (for the evaluation files)

The architecture specification names the LLMs, APIs, and tools that a production version would use (GPT-4o, OpenFDA, FAISS with text-embedding-3-small, NPPES NPI Registry, Epic FHIR), but these are not invoked in the prototype. See `platform_notes.md` for the full platform specification.

## Folder guide
team-project/
├── README.md                    This file
├── AI_USAGE.md                  AI tools used, prompts, manual edits
├── platform_notes.md            Platform and tool specification
├── docs/
│   ├── final_report.pdf         Phase 3 final report
│   ├── architecture_diagram.pdf System architecture
│   ├── project_summary.pdf      Standalone project summary
│   └── screenshots/             4–8 captioned screenshots + index
├── eval/
│   ├── test_cases.csv           42 test scenarios with results
│   ├── evaluation_results.csv   Aggregated outcomes
│   ├── failure_log.md           Five documented failure cases
│   └── version_notes.md         Iteration history
├── outputs/
│   ├── demo_outputs/            Prototype interaction examples
│   ├── exported_artifacts/      Sample agent outputs
│   └── sample_runs/             Walkthrough recordings
├── media/
│   ├── demo_video_link.txt      Link to 5-minute project video
│   └── figma_prototype_link.txt Link to interactive prototype
└── phase_submissions/
├── phase1/                  Scoping & justification
├── phase2/                  Architecture, prototype, evaluation plan
└── phase3/                  Final submission materials

## Evaluation materials

The system was evaluated against a matrix of 42 test scenarios anchored to Margaret's clinical context. Coverage spans five dimensions: precision and noise filtering, multi-day episodic memory, cross-referencing logic, human-in-the-loop fallbacks, and low-latency deterministic overrides.

The test set includes happy paths (routine recovery), alternate paths (multi-day reasoning), and edge cases (adversarial prompts, API failures, ambiguous input). Results are recorded with case ID, scenario, expected behavior, observed behavior, outcome, and supporting evidence in `eval/test_cases.csv`.

Five failure cases are documented in detail in `eval/failure_log.md` (FC-01 through FC-05), each with root cause analysis, clinical impact assessment, and prioritized remediation. Two failures (FC-01 and FC-03) carry direct patient safety implications and are flagged as P0 blocking issues.

## Outputs included

- **Final architecture specification** with four agent canvases, trigger-to-agent transition table, and coordination logic
- **Interactive Figma prototype** covering nine connected screens across patient and clinical surfaces
- **Eight-to-ten captioned screenshots** of key prototype states in `docs/screenshots/`
- **42 evaluation cases** with structured results, including 8 documented failures and 34 passes
- **Five detailed failure case writeups** with root cause analysis and remediation priorities
- **5-minute project video** demonstrating the problem, architecture, main workflow, evidence layer, and a representative failure

## Known limitations

- **The prototype is design-first, not executable.** It demonstrates the interaction flow and renders agent outputs as pre-populated examples rather than live LLM calls. The agent specifications, evaluation framework, and failure analysis assume executable behavior; bringing the system online is the natural next phase.
- **The patient name displayed in some prototype screens is "Sarah," not "Margaret."** This is a placeholder artifact from earlier prototype iterations. All written documentation, evaluation cases, and clinical context are anchored to Margaret.
- **Scope is restricted to one persona and one surgery type.** All evaluation and architecture decisions are grounded in Margaret's post-CABG recovery. Generalization to other surgery types, chronic conditions, or demographic profiles is future work.
- **No real PHI or HIPAA infrastructure.** The system is built for a happy-path simulated environment and does not implement HIPAA-compliant storage, Business Associate Agreements with LLM providers, or production-grade access controls. These are required before any clinical deployment.
- **Demographic bias evaluation (Case 9) is specified architecturally but not yet executed at runtime.** Demographic blinding is enforced in the design as an Orchestrator constraint, but inspectable evidence of the constraint holding under varied demographic profiles is future work.
- **Two patient-safety-critical failures (FC-01 sentiment override, FC-03 referral closure) must be remediated before any clinical pilot.** The system is appropriate as a research prototype demonstrating the architecture; it is not appropriate for unsupervised clinical use without the P0 remediations in `eval/failure_log.md`.
