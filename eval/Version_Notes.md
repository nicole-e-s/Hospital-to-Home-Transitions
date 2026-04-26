# Version Notes

## v1.0 (Baseline Multi-Agent System)
* **What Changed**: Initial implementation of the Multi-Agent Framework. 
* **Details**: Established baseline logic for Agent A, Agent B, and Agent C. Implemented basic RAG integration. System relied heavily on LLM autonomous resolution without deterministic guardrails.

## v1.1 (Security & Adversarial Defenses)
* **What Changed**: Introduced Confidence Fallbacks, UI Overrides, and Prompt Injection filters.
* **Details**: Added Confidence Score (0.0-1.0) requirement. Implemented Orchestrator fallback (<0.85). Added prompt sanitization to defend against Jailbreaks and SQL injections.

## v1.2 (Tool-Use Reliability & Cross-Agent State Guards)
* **What Changed**: Implemented strict DB cross-referencing and Cross-Agent State validations.
* **Details**: Fixed failures where Orchestrator trusted patient self-reports over missing DB records (e.g., pharmacy refills, appointment attendance). Added strict "Source Gap" handling.
