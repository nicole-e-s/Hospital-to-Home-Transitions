# AI Usage Log
**For Evaluation Part**
* **Tool name and version:** Gemini Advanced (Gemini 3.1 Pro)
* **What you used it for:** To operationalize my conceptual evaluation framework by structuring, expanding, and formatting the test case matrix and evaluation results. I used it to translate my baseline assumptions and ideas into highly detailed, synthetic clinical scenarios.
* **Exact prompt or task given to the tool:** "Based on my defined system architecture, assumptions, and the CLASSic evaluation framework, expand my initial test case scenarios into more comprehensive test cases. Generate realistic clinical inputs (patient quotes/data) and define strict success/failure criteria for each. Map these outcomes to specific evaluation metrics, covering Happy Paths, Alternate Paths, and complex Edge Cases."
* **What you changed manually afterward:** I manually reviewed and rewrote various generated clinical input descriptions to ensure they accurately reflected realistic patient phrasing and behavioral nuances. I also strictly guided the iterative revisions of each version, prompting the AI to adjust specific parameters based on failure case analysis, increase granularity, and refine the technical accuracy of the expected behaviors to match my exact project requirements.
* **What you verified independently:** I independently verified the logical consistency between the generated scenarios and the system's actual technical constraints (e.g., verifying that the confidence score thresholds, latency overrides, and state-tracking logic made sense for our architecture). I also validated the accuracy of the final evaluation criteria and ensured all outputs met rigorous academic and engineering standards before compiling the final matrices.

**For Packaging**
* **Tool name and version:** Claude Opus 4.7
* **What you used it for:** To help with technical issues faced during Github setup and summarize information.
* **Exact prompts given to the tool:**
* - "I tried to copy this folder guide into GitHub but it doesn't show up right in the README"
  - "can you just give me step by step instructions ons etting up this structure on Github? obviously some of the files like the pngs and pdfs arent ready yet, but i can set up the folders"
  - "Can you make a 'List of submitted files and folders' for the final report"
* **What you changed manually afterward:** I accepted Claude's instructions to set up the Github and troubleshoot issues.
* **What you verified independently:** I verified that the instructions were correct by checking the structure and formatting after committing and cerified that the list of submitted files and folders were correct.

**For Figma**
* **Tool name and version:** Sonnet 4.6
* **What you used it for:** To help with front end generation
* **Exact prompts given to the tool:**
* Design a high-fidelity interactive prototype for the Post-Discharge Care Companion, a multi-agent AI system that supports patients during hospital-to-home recovery. The prototype should operationalize agentic concepts (persistent memory, autonomous monitoring, multi-agent coordination, human-in-the-loop escalation) through lived patient experience — not feature lists or dashboards.
 
Screen 1 — Discharge Onboarding (Schedule Planner Agent) A warm, accessible welcome screen addressed to the patient by first name. Display a plain-language summary of their discharge plan (e.g., medications, activity restrictions, follow-up dates), translated from clinical language into simple instructions. Include a large "Confirm I Understand" button and a secondary "Read to Me" accessibility option. Annotate this screen: "Agent action: Parses discharge PDF → builds structured care plan → stores in patient memory"
 
Screen 2 — Daily Check-In (Symptom Evaluator Agent) A conversational interface that appears each morning. Show 3–4 symptom questions (e.g., "How is your breathing today?" with a visual scale from 1–5; "Did you take your morning medications?"). Design this as a branching decision flow — depending on answers, route to one of three outcomes:
•	Path A (All clear): Affirming message + view today's care plan
•	Path B (Mild concern): Gentle flag + add-on instruction (e.g., "Rest more today, check in again at 3pm")
•	Path C (High-risk pattern): Urgent escalation screen (Screen 4)
Annotate: "Agent action: Evaluates symptoms against risk thresholds → updates patient state → triggers conditional routing"
Screen 3 — Care Plan View (Schedule Planner Agent) A clean daily timeline showing medications (with times and dosage), activity goals, and a follow-up appointment card. Include a "Mark as Done" interaction for each task. Show a subtle streak/adherence indicator (e.g., "3 days on track"). Include an inline nudge for a missed medication with a reschedule option. Annotate: "Agent action: Reads patient memory → renders personalized plan → logs completions → adjusts reminders based on patterns"
Screen 4 — Escalation Alert (Human-in-the-Loop) Triggered from Path C in Screen 2. Display a high-contrast urgent screen with: a plain-language summary of what symptoms triggered the alert, a reassuring patient message ("Your care team has been notified"), and a live status card showing: "Nurse [Name] reviewing your case — expected response in 15 min." Include a secondary action: "Call 911 if symptoms are severe." Show a parallel view (caregiver/nurse perspective) as an annotation sidebar — what the clinical stakeholder receives. Annotate: "Agent action: Escalation agent fires → notifies care coordinator → logs event → pauses autonomous check-in loop"
 
Screen 5 — Insurance & Follow-Up Coordination (Insurance Liaison Agent) After escalation resolves or from the care plan view, show a proactive screen: "Your care team suggests a follow-up with a cardiologist within 7 days. Here are in-network options near you." Display 2–3 provider cards with name, distance, estimated cost, and availability. Include one-tap "Request Appointment" and "See Insurance Details" actions. Annotate: "Agent action: Queries provider directory → filters by insurance network → pre-populates appointment request → reduces patient cognitive burden"
 
Screen 6 — Weekly Reflection & Outcome Summary A gentle end-of-week wrap-up screen showing adherence stats (medications taken, check-ins completed), a short prompt ("What felt hardest this week?"), and a visual trend of symptom patterns over 7 days. Include a "Share with Doctor" button that generates a summary PDF. Annotate: "Agent action: Aggregates longitudinal state → surfaces patterns → prepares handoff summary for clinical review"
 
Design Requirements:
•	Primary users: elderly patients with low health literacy → use large text (min 18pt), plain language, high contrast, large touch targets
•	Color system: calm and clinical — use soft blues and teals for safe states, warm amber for mild alerts, coral/red only for urgent escalation
•	Show connector arrows between all screens with branch labels ("if high-risk → Screen 4", "if resolved → Screen 3")
•	Add a floating annotation layer on each screen labeling which agent is active, what memory it reads, and what action it triggers
•	Include a component sidebar with reusable elements: symptom scale, medication card, provider card, alert banner, status indicator
•	Mobile-first layout (390px wide), iOS-style navigation with a persistent bottom nav bar showing: Home, Check-In, Care Plan, Messages
* - “Extend the “Recovery Companion” healthcare app by adding a Medication Pickup and Refill interface. The design should feel consistent with the existing care workflow: calm, structured, and easy for older adults to use.
  - Purpose:
Allow patients to see where to pick up medications, track refill status, and request help if needed.
1. Entry Point (from Home Screen)
Add a small link under “Today’s Medications”:
Text: “Pickup & refill details”
Tapping opens a new screen: Medication Pickup
2. Medication Pickup Screen Layout
Header
Title: “Medication Pickup”
Subtitle: “Your prescriptions and pharmacy details”
Section 1 — Ready for Pickup
Card title: “Ready for pickup”
For each medication:
Medication name (bold)
Status tag: “Ready” (green)
Pharmacy name + address
Pickup window (e.g., “Available until April 28”)
Button: “Get directions”
Section 2 — In Progress / Refill Needed
Card title: “Refill needed”
Each item:
Medication name
Status tag: “Refill required” (amber)
Last refill date
Button: “Request refill”
Section 3 — Pharmacy Info
Card:
Pharmacy name (e.g., UPMC Pharmacy)
Address
Phone number
Hours
Button: “Call pharmacy”
Section 4 — Delivery Option (optional but strong)
Card:
Title: “Delivery available”
Text: “Some medications may be delivered to your home”
Button: “Check delivery options”
3. Chatbox Integration

Update the chat assistant behavior:

Suggested prompts:

“Where do I pick up my medication?”
“Is my prescription ready?”
“Help me refill my medication”

Chat responses should:

Pull structured data from care plan + pharmacy info
Provide short answers:
“Your Metoprolol is ready at UPMC Pharmacy.”
Include action buttons:
“View pickup details”
“Call pharmacy”

If user says:
“I ran out of medication”
Chat should respond:

“I can help you request a refill.”
Button: “Request refill”
4. Design Style (match existing system)
Same card layout and spacing as “Today’s Plan”
Status colors:
Green = ready
Amber = refill needed
Gray = processing
Large readable buttons
No clutter, no dense text
5. Agent System Mapping (for annotation in Figma)

Add notes in your design:

Medication data comes from Agent A (care plan extraction)
Refill status could be:
Prototype: static / mock data
Production: pharmacy API integration
Chatbox = navigation + retrieval layer, not decision-make”
- “Add sticky notes outside of the phone screen explaining. You should have an icon, when press it, the sticky note explaning the agent will show 

Which backend agent populates this content
What data model field maps to each UI element
What triggers navigation or state change
What the system displays vs. what the agent actually computed

This makes the prototype reviewable by engineers, clinicians, and evaluators — not just designers.

Agent mapping quick reference
Agent A (Schedule Planner): Home screen medications, appointments, activity/diet cards
Agent B (Symptom Evaluator): Check-in result screen, escalation banners, History trend, staff detail page sections 1–4
Agent C (Insurance Liaison): Provider options in staff detail section 5 (only when triggered)
Orchestrator: Determines which screens are reachable and which agents activate — not visible in UI, but drives all routing logic“

- “put all purple modules, such as 

Agent: Symptom Evaluator Agent
Evaluates symptoms against risk thresholds → updates patient state → triggers conditional routing

Baseline vitals, symptom history, risk factors

Routes to affirmation, mild concern, or urgent escalation

on the left or grey empty area. Do not put it on the bottom which blocks the view”

- “it sould be exactly for phone screen, make the bottom stuff gone, or appear outside of the app design layout.

Agent: Symptom Evaluator Agent
Evaluates symptoms against risk thresholds - updates patient state - triggers conditional routing

Baseline vitals, symptom history, risk factors”

* **What you changed manually afterward:** I manually directed UI refinements throughout the design process, including page layout and structure, screen functionality and interaction flow, color theme and visual hierarchy, button placement and labeling, banner design and escalation indicators, and stakeholder-specific views (patient app vs. nurse dashboard).
* **What you verified independently:** I independently verified that the UI was user-friendly, visually clear, and aligned with the intended use case — evaluating whether the patient-facing screens were appropriately simple and non-clinical, whether the escalation flags were prominent and actionable, and whether the nurse dashboard accurately reflected the system's triage logic.
