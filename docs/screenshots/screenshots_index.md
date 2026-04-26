# Screenshot Index — Post-Discharge Care Companion Prototype
**Project:** A Multi-Agent Framework for Safe Hospital-to-Home Transitions
**Team:** Anshika Shukla, Nicole Sanna, Nicole Zhou, Yuhang Zhou
**Prototype Tool:** Figma Make
**Report Referenced:** Phase 3 Final Report (Failure Analysis & Evaluation)

###**Note:** Screenshots are numbered 1-27 in the order of the prototype flow
---

## How to Read This Index

Each entry follows this structure:
- **screenshot_file** — filename as submitted
- **what_it_shows** — description of the screen's content and visible UI elements
- **why_it_matters** — clinical, architectural, or evaluative significance
- **where_it_is_discussed_in_the_report** — specific section(s) in the Phase 3 Final Report

---

## Screenshot 1
**screenshot_file:** `Landing_Page_1.png`

**what_it_shows:**
The entry point of the prototype. A mobile screen titled "Recovery Companion — Post-discharge care support" presents two role-selection cards: (1) "I am a patient" — pre-filled as Margaret Chen, with access to the care plan and daily check-in; and (2) "I am a care coordinator" — pre-filled as Nurse Rivera, with access to the patient triage queue and alerts. A purple prototype note at the bottom explains that in production these would be two separate apps (mobile patient app and web staff dashboard), but the prototype uses a single interface with a role-switch for demonstration purposes.

**why_it_matters:**
Establishes the two primary stakeholder interfaces the prototype supports. The dual-role design reflects the architectural decision to separate patient-facing and staff-facing surfaces, while the prototype note makes clear the scoping decision to demonstrate both in one Figma artifact. This screen is the gateway to both the patient journey and the clinical staff workflow.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 5 (Build Summary): "Patient-facing UI before staff-facing UI" scoping decision — the patient is the primary user and faces important problems the system addresses (low health literacy, accessibility constraints, plain-language safety messaging).
- Phase 3, Section 3 (User Profile): the two stakeholder groups (patients in the 30-day post-discharge window and clinical staff/nurses) whose needs drive the dual-interface design.

---

## Screenshot 2
**screenshot_file:** `Trasparent_Agent_Lookup_info2.png`

**what_it_shows:**
The same landing page as Screenshot 1, but with the "Agent Mapping Quick Reference" side panel open (accessed via the purple gear icon). The panel displays all four system components in color-coded cards: Agent A (Schedule Planner) — scope covers home screen medications, appointments, activity/diet cards; data model includes medication schedule, activity goals, appointment dates, adherence history. Agent B (Symptom Evaluator) — scope covers check-in result screen, escalation banners, History trend, staff detail sections 1–4; data model includes baseline vitals, symptom history, risk thresholds, 7-day trajectory analysis. Agent C (Insurance Liaison) — scope covers provider options in staff detail section 5 (only when triggered); data model includes insurance plan details, in-network providers, patient location, appointment history. Orchestrator — determines which screens are reachable and which agents activate.

**why_it_matters:**
This is the primary evaluator-facing artifact that maps every UI screen to the agent that populates it. It makes the prototype reviewable by engineers, clinicians, and evaluators who need to trace each output to a specific agent — a clinical safety requirement. The panel is accessible from any screen, ensuring the architecture is never more than one tap away from the UI it produces.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 5 (Build Summary): "Annotations as an artifact" scoping decision — agent-to-screen mapping built into the prototype as removable sticky notes, making the artifact reviewable by engineers and clinicians, not just designers.
- Phase 3, Section 4 (Architecture and design choices): the four-component system (three worker agents + one orchestrator) and the rationale for separating their responsibilities.

---

## Screenshot 3
**screenshot_file:** `Home_Page_Agent_info3.png`

**what_it_shows:**
The patient-facing home screen for "Sarah, Day 7 of recovery." Shows a green status banner ("Recovery on track — no concerns flagged, last updated 9:15 AM"), a "Today's medications" section listing Lisinopril 10mg at 8:00 AM (with green checkmark indicating confirmed) and Atorvastatin 20mg at 9:00 PM (unchecked, pending), and the beginning of a "Next appointment" section. An Agent A sticky note is open to the right: Data Model — `appointments[] { provider, specialty, date, time, location }, patient.dischargeDate`; Navigation Trigger — user taps 'Add to calendar' → generates .ics file → native calendar integration; Display vs. Computed — Display: 'Dr. Sarah Johnson · Cardiology · Tuesday, April 15 at 10:00 AM · In 4 days'. Computed: days from today calculated as (appointment.date - currentDate). A "Demo Mode" banner with "Switch Role" and "Staff View" buttons is also visible.

**why_it_matters:**
Demonstrates Agent A's structured output (the appointments array) rendered in the patient UI. The annotation clarifies the distinction between what the patient sees (plain-language appointment card) and what Agent A computed (structured JSON parsed from the discharge PDF and enriched via the OpenFDA Drug Labelling API). The calendar integration reflects the production-grade feature described in the architecture — the prototype renders in-app while a production version would integrate a calendar API for push notifications.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Architecture and design choices): "Agent A uses PyMuPDF for deterministic text extraction, GPT-4o for structured interpretation, and the OpenFDA Drug Labelling API queried per medication for warnings, interactions, and dosage."
- Phase 3, Section 5 (Build Summary): prototype build approach treats the Phase 2 architecture document as the source of truth; each Figma Make prompt was grounded in a specific agent canvas section.

---

## Screenshot 4
**screenshot_file:** `Home_Page_Agent_info_4.png`

**what_it_shows:**
The same patient home screen as Screenshot 3, but with a different Agent A sticky note open focusing on the medications section. Data Model — `medications[] { name, dosage, time, completed, foodRequirement }, adherenceHistory[]`; Navigation Trigger — user taps medication checkbox → updates completed status → logs to adherence history → triggers streak calculation; Display vs. Computed — Display: 'Lisinopril 10mg · 8:00 AM' with checkbox. Computed: medication.name + medication.dosage + medication.time, completion state from medications[].completed, food warning from medications[].foodRequirement.

**why_it_matters:**
Shows how the medications array written by Agent A at onboarding is rendered as an interactive checklist. The annotation explains that the patient's checkbox tap writes to the adherence history, which feeds the Orchestrator's independent medication adherence tracking. This is the mechanism behind the Case 3 escalation path: the Orchestrator can detect a critical adherence gap independently of Agent B's symptom assessment, even when Agent B returns Green. This design is also directly relevant to Failure Case FC-02, where self-reported adherence was accepted without pharmacy cross-check — the adherence log written by this checkbox was treated as ground truth.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Memory model): "Recalculated adherence state is what allows the Orchestrator to detect critical adherence gaps independently of Agent B's symptom assessment, which is the mechanism behind the Case 3 escalation path."
- Phase 3, Section 8 (Failure Analysis, FC-02): "Based solely on this self-report, the Orchestrator Agent recorded the patient as Adherent and automatically closed the adherence record for the day."

---

## Screenshot 5
**screenshot_file:** `Medication_Pickup_5.png`

**what_it_shows:**
The "Medication Pickup" screen titled "Your prescriptions and pharmacy details." Shows a "Ready for pickup" section listing two medications: Lisinopril 10mg (30-day supply, green supply bar, UPMC Pharmacy at 200 Lothrop St, Pittsburgh, PA 15213, available until April 28, "Get directions" button) and Atorvastatin 20mg (30-day supply, green supply bar, same pharmacy, same availability, "Get directions" button). A "Refill needed" section header is partially visible at the bottom.

**why_it_matters:**
Demonstrates the patient-facing pharmacy workflow surfaced from Agent A's medication data. Supply status bars provide a visual cue for adherence support, and the "Get directions" button reflects the design goal of reducing cognitive burden for post-discharge patients — particularly elderly patients with chronic conditions and low health literacy. This screen is the context for Failure Case FC-02, where the system failed to cross-check a patient self-report against actual pharmacy dispense records (CVS Rx #4471829 showing zero pills remaining after 30 days with no refill).

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-02): "Pharmacy data from CVS Pharmacy (Rx #4471829) indicates the medication was last dispensed on March 26, 2026 — 30 days prior — with an estimated zero pills remaining. The system failed to perform a required pharmacy cross-check."
- Phase 3, Section 3 (User Profile): target population includes elderly patients with chronic conditions and those with low health literacy or limited support.

---

## Screenshot 6
**screenshot_file:** `Medication_Refil_6.png`

**what_it_shows:**
The lower portion of the Medication Pickup screen, showing a "Refill needed" section. Metoprolol 50mg (last filled March 15) is highlighted with an orange/amber supply bar, with the message "You have 3 days of medication remaining. Request a refill now to avoid running out" and a green "Request refill" button. Below is a "Your pharmacy" section showing UPMC Pharmacy details (200 Lothrop St, Pittsburgh, PA 15213; phone (412) 647-8762; hours Mon–Fri 8:00 AM–8:00 PM, Sat–Sun 9:00 AM–6:00 PM) with a "Call pharmacy" button.

**why_it_matters:**
The amber supply bar and refill warning for Metoprolol illustrates adherence support functionality. Metoprolol is a beta-blocker — the exact class of medication at the center of Failure Case FC-05, where a physician-issued hold instruction (hold Metoprolol if resting HR < 50 bpm) was not integrated into the adherence decision logic. The system flagged the patient as Non-Adherent and sent a false alert to Nurse Rivera, when the patient had correctly skipped the dose per physician instruction. The refill alert also surfaces the FC-02 gap: the system should cross-reference patient self-reports against actual pharmacy dispense records.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-05): "Despite both objective physiological data and a valid clinician-issued directive supporting the patient's action, the system classified the event as Non-Adherent and generated an alert to Nurse Rivera citing a missed dose."
- Phase 3, Section 8 (FC-05, Root Cause #3): "Hold condition not integrated into adherence decision path: The adherence logic evaluates dose taken vs. dose scheduled without checking whether an active hold condition permits or requires omission."
- Phase 3, Section 8 (Future Improvement, P1): "Integrate hold condition registry into adherence decision logic."

---

## Screenshot 7
**screenshot_file:** `Pharmacy_Details_for_Pickup_refil_7.png`

**what_it_shows:**
The lower section of the Medication Pickup screen. Shows the "Your pharmacy" card (UPMC Pharmacy, 200 Lothrop St, Pittsburgh PA 15213, (412) 647-8762, hours Mon–Fri 8:00 AM–8:00 PM, Sat–Sun 9:00 AM–6:00 PM, "Call pharmacy" button) and a "Delivery available" section with a "Home delivery" card noting that some medications may be delivered for free with 3–5 business day turnaround, with a "Check delivery options" button.

**why_it_matters:**
Completes the pharmacy details view. The home delivery option reflects the system's goal of reducing care access friction for post-discharge patients, particularly those with physical activity restrictions — Margaret has strict post-CABG limits prohibiting extended travel. Reducing friction for medication pickup directly addresses the adherence gap risk documented in FC-02 (medication self-report without pharmacy cross-check) and FC-05 (dose skip without hold condition check).

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 3 (User Profile): target population includes post-surgical patients with physical activity restrictions and those without strong in-home support.
- Phase 3, Section 8 (Failure Analysis, FC-02 and FC-05): both failures involve medication adherence tracking gaps that this screen's pharmacy integration is designed to support.

---

## Screenshot 8
**screenshot_file:** `Next_Appointment_Details_8.png`

**what_it_shows:**
The lower portion of the patient home screen, scrolled to reveal the "Next appointment" section fully: Dr. Sarah Johnson, Cardiology, Tuesday April 15 at 10:00 AM, "In 4 days," with an "Add to calendar" link. Below are two side-by-side cards: Activity (light walking only, no lifting >10 lbs, rest when tired, "See full restrictions" link) and Diet (low sodium, stay hydrated, heart-healthy fats, "See full restrictions" link). A large green "Start today's check-in" CTA button is at the bottom with navigation tabs (Today, History, Providers, Help).

**why_it_matters:**
Shows the complete Day 0 care plan surface in the patient app: appointment, activity restrictions, and dietary guidelines — all outputs of Agent A's structured parsing of the discharge PDF. The "Start today's check-in" button triggers the daily check-in workflow that activates Agent B. The appointment card is directly relevant to Failure Case FC-03, where an urgent cardiology referral — structurally identical to this scheduled appointment — was closed by the Orchestrator based solely on a positive patient self-report without verifying attendance.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-03): "A high-risk, provider-initiated referral was prematurely closed based solely on a single, low-fidelity self-report, with no confirmation of whether the recommended evaluation was completed."
- Phase 3, Section 4 (Architecture): "Agent A (Schedule Planner) parses Margaret's discharge document into a structured care plan at Day 0 and does not run again unless the plan is updated."

---

## Screenshot 9
**screenshot_file:** `After_clicking_checkin_Symptom_Evaluator_9.png`

**what_it_shows:**
The daily check-in screen, Step 1 of 2, dated April 11, 2026. Three structured questions are visible: "How difficult is your breathing today?" (1–5 scale, Easy to Very Difficult); "Any chest pain?" (None / Mild / Strong); "Did you experience dizziness when standing?" (Yes / No). An Agent B sticky note is open to the left: Data Model — `questions[] { id, type, text, redFlagThreshold }, answers { questionId: value }`; Navigation Trigger — user selects answer → if value >= redFlagThreshold → immediately show interrupt screen → notify staff. Otherwise → continue to next question; Display vs. Computed — Display: '1 2 3 4 5' scale buttons. Computed: breathing >= 5 OR chestPain === 'Strong' → triggers red flag interrupt (deterministic front-end rule, not LLM).

**why_it_matters:**
This is the critical screenshot for understanding the hard-coded red-flag interrupt mechanism. The annotation explicitly states that the trigger is a deterministic front-end rule, not an LLM call — implementing the <1000ms latency requirement from Case 7 with zero LLM calls on the acute path. This is one of the key safety design choices: the takeover renders in under one second before Agent B runs. This screen is also the entry point for the input type evaluated in Failure Case FC-01, where positive sentiment in the free-text field suppressed a Red-level clinical signal that should have escalated immediately.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Key safety design choices): "A hard-coded front-end red-flag interrupt fires before Agent B runs when a patient selects 'Strong' chest pain or breathing difficulty 5/5. The takeover renders in under one second with zero LLM calls on the acute path."
- Phase 3, Section 6 (Evaluation, Case 7): Deterministic UI Override & Low Latency — "UI Interrupt Latency < 1000 milliseconds. LLM API calls required to trigger the alert = 0."
- Phase 3, Section 8 (Failure Analysis, FC-01): the check-in input path where sentiment contamination occurred — a patient selected non-red options here but reported a Red-level symptom in the free-text field.

---

## Screenshot 10
**screenshot_file:** `Fill_in_the_check_in_questionaire_10.png`

**what_it_shows:**
Check-in Step 1 of 2 showing the lower portion of the structured questions with answers filled in. "Any chest pain?" shows "Mild" selected (highlighted in amber/orange). "Did you experience dizziness when standing?" shows "Yes" selected (green button). "Did you take all your medications today?" shows "Yes" selected (green button). A green "Next" button leads to Step 2. The chat assistant FAB is visible at the bottom right.

**why_it_matters:**
Shows a non-red-flag check-in in progress: Mild chest pain (not "Strong," so no front-end interrupt fires) with dizziness — this input profile routes to Agent B for full assessment. The amber "Mild" highlight visually communicates the yellow-tier signal without alarming the patient. This is the input pattern relevant to the four-tier severity model refined in Phase 3: yellow-low (monitor without alerting care team) vs. yellow-high (escalate with ambiguity flag). The "Medications: Yes" answer also feeds the adherence log — the same self-report that FC-02 shows was accepted without pharmacy cross-check.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 5 (Build Summary): "Four screens of severity rather than three" — yellow-low added to represent cases that should be monitored but not yet flagged to the care team.
- Phase 3, Section 4 (Key safety design choices): the four-tier severity model (green / yellow-low / yellow-high / red).
- Phase 3, Section 8 (Failure Analysis, FC-02): medication confirmation ("Did you take all your medications today? Yes") is the self-report the Orchestrator accepted as ground truth without pharmacy cross-check.

---

## Screenshot 11
**screenshot_file:** `Next_Add_Aditional_Details_11.png`

**what_it_shows:**
Check-in Step 2 of 2, dated April 11, 2026. A free-text field labeled "Anything else you'd like to report?" has been filled in with "Tiredness and heavy breathing." A note below the field reads "Your care team will review this along with your answers above." Two action buttons: "Submit check-in" (primary, green) and "Back."

**why_it_matters:**
This is the open-text input field that feeds Agent B's NLP assessment alongside the structured scale responses from Step 1. This is the exact surface where Failure Case FC-01's input — "Feeling great! But my incision opened and is bleeding heavily" — would be entered. Agent B evaluated the message's sentiment holistically rather than parsing each clinical assertion independently, and the positive opening sentiment caused it to return Green for what was an active wound bleed. FC-01 is a direct indictment of how Agent B processes free-text in this field.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-01): "Patient submitted the following free-text entry: 'Feeling great! But my incision opened and is bleeding heavily.' Despite the presence of a clear, high-risk clinical signal, Agent B classified the case as Green ('Looks stable today') and did not trigger any escalation."
- Phase 3, Section 8 (FC-01, Root Cause #2): "No sequential clause parsing: The agent evaluated message sentiment holistically rather than parsing each clinical assertion independently. The conjunctive structure ('but') was not treated as a signal inversion."
- Phase 3, Section 8 (Future Improvement, P0): "Implement clinical keyword hard floors in Agent B: Map terms including 'bleeding heavily,' 'incision opened,' 'chest pain,' and 'can't breathe' to a minimum Red severity that cannot be overridden by sentiment scoring."

---

## Screenshot 12
**screenshot_file:** `Submit_Checkin_Evaluator_agent_give_suggestion_12.png`

**what_it_shows:**
The "Check-in complete" results screen showing a Yellow escalation outcome. An amber banner reads "Your care team may take a look — Dizziness has been increasing over the past 3 days, which your care team will review." Below: "We recommend contacting your care team today — Your responses suggest something your care team should review. You do not need to wait for them to call you." Three action buttons: "Call care team now," "Find a nearby nurse station," and "Send a message instead." The text "A care coordinator has also been automatically notified" is partially visible at the bottom. An Agent B sticky note is open to the right: Data Model — `checkInResponse { breathing: 1-5, chestPain: 'None'|'Mild'|'Strong', ... }, symptomHistory[], riskThresholds`; Navigation Trigger — Check-in submitted → Agent B evaluates against thresholds → returns severity ('green'|'yellow'|'red') → routes to result screen → notifies staff if red/yellow; Display vs. Computed — Display: 'Looks stable today' or 'Your care team may take a look'. Computed: Agent B runs rule-based + trajectory analysis → returns `{ severity: 'green', reason: 'All symptoms within baseline range', shouldEscalate: false }`.

**why_it_matters:**
Demonstrates a Yellow trajectory escalation: dizziness increasing over 3 consecutive days triggers escalation regardless of any single day's individual classification — a core safety design choice. The "find a nearby nurse station" option represents the yellow-low action path from the four-tier severity model refined in Phase 3. The Agent B annotation distinguishes Display vs. Computed, making the architecture auditable. This screen also shows the correct escalation behavior that FC-01 failed to produce: FC-01's Green result with no escalation was the opposite of this screen.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Key safety design choices): "Three-day yellow trajectories trigger escalation regardless of any individual day's classification, which accounts for gradual health deterioration."
- Phase 3, Section 5 (Build Summary): "Four screens of severity rather than three" — yellow-low added; the "find a nearby nurse station" is the yellow-low action path.
- Phase 3, Section 8 (Failure Analysis, FC-01): contrast — this screen shows correct Yellow escalation behavior; FC-01 produced a Green result for a clinically worse input.

---

## Screenshot 13
**screenshot_file:** `What_happens_next_summary_13.png`

**what_it_shows:**
The lower portion of the "Check-in complete" Yellow escalation screen. Shows a soft amber banner confirming "A care coordinator has also been automatically notified." A "What happens next" section explains "A care coordinator may reach out within 24 hours. You can also contact them directly above." A "Questions or concerns?" section with two buttons: "Call (555) 123-4567" and "Send message." A "Today's summary" expandable section (open) shows: Breathing difficulty: 2/5; Chest pain: None; Dizziness when standing: Yes; All medications taken: Yes. A "Back to home" green button is at the bottom.

**why_it_matters:**
Shows the full post-check-in summary the patient receives, including the structured symptom log written to the episodic memory record. The "Today's summary" section demonstrates how structured responses become append-only episodic memory entries that Agent B uses for trajectory detection. The automatic care coordinator notification confirms the Orchestrator's escalation action. The system never leaves the patient without a clear next step — directly enforcing the governance rule against care avoidance.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Memory model): "Episodic memory is an append-only log of daily check-ins; Agent B retrieves a 7-day rolling window to detect trajectories."
- Phase 3, Section 9 (Risk & Governance): "Over-reliance and care avoidance" risk — "The system never says 'you're fine,' only describes what was reported. Hard rule: never discourage seeking care. Every escalation path includes the 'contact your provider' option."

---

## Screenshot 14
**screenshot_file:** `Patient_History_Screen_a__14.png`

**what_it_shows:**
The patient-facing "Your recovery history" screen for April 5–11, 2026. Shows: a "7-day trend" visualization with seven colored circles — Mon (green), Tue (green), Wed (orange/yellow), Thu (green), Fri (green), Sat (green), Sun (outlined/pending). A "Summary" text block reads: "Past 7 days: mostly stable, with one yellow flag on Wednesday for mild dizziness. You've completed daily check-ins and maintained good medication adherence." A "Medication adherence" progress bar shows 6/7 days with "All doses confirmed" partially visible. The History tab is active.

**why_it_matters:**
Demonstrates Agent B's 7-day episodic memory window rendered as a patient-facing visualization, grounded in the post-surgical readmission risk literature (Jencks et al., 2009). The color-coded trend chart is the output of trajectory detection across the rolling window. The Wednesday single yellow dot followed by return to green correctly did not trigger escalation — illustrating the distinction between a one-day yellow (monitor) and a 3-consecutive-day trajectory (escalate). The medication adherence bar tracks adherence state derived from the episodic log independently of Agent B's symptom assessment.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Memory model): "Episodic memory is an append-only log of daily check-ins; Agent B retrieves a 7-day rolling window to detect trajectories, an interval grounded in the post-surgical readmission risk literature (Jencks et al., 2009)."
- Phase 3, Section 4 (Key safety design choices): "Three-day yellow trajectories trigger escalation regardless of any individual day's classification."

---

## Screenshot 15
**screenshot_file:** `Patient_History_Screen_b__15.png`

**what_it_shows:**
The recovery history screen scrolled down to show: Medication adherence in full — progress bar (6/7 days), "All doses confirmed," and a yellow flag note "Saturday: Evening dose not confirmed." Daily check-ins section with Mon Apr 5 (green, collapsed: "Breathing easy, no pain") and Tue Apr 6 (green, expanded: "Slight fatigue, otherwise stable," detailed data: Breathing: 2/5, Pain: 1/5, Swelling: 1/5, Medications: All taken). Thu Apr 8 entry is partially visible at the bottom.

**why_it_matters:**
Shows the detailed daily check-in log constituting the episodic memory entries. The Saturday missed dose flag demonstrates the adherence tracking the Orchestrator uses independently of Agent B's symptom assessment to detect critical medication gaps. This surface makes Failure Case FC-02 legible: the system recorded "Adherent" for Coreg based on patient self-report even though pharmacy records showed zero pills remaining — had the cross-check been implemented, this episodic log entry would instead show a critical non-adherence flag.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-02): "The system failed to perform a required pharmacy cross-check and did not detect this inconsistency. As a result, a critical non-adherence event was misclassified as adherence."
- Phase 3, Section 8 (FC-02, Root Cause #1): "No pharmacy cross-check initiated: The Orchestrator did not query the pharmacy data source after receiving the patient's self-report."

---

## Screenshot 16
**screenshot_file:** `Patient_History_Screen_c__16.png`

**what_it_shows:**
The recovery history screen continued, showing the remainder of the 7-day log: Wed Apr 7 (yellow/orange dot, "Mild dizziness when standing"), Thu Apr 8 (green, "Back to normal, no dizziness"), Fri Apr 9 (green, "Feeling strong"), Sat Apr 10 (green, "Good progress"), Sun Apr 11 (green, "Stable and improving"). All entries in collapsed view.

**why_it_matters:**
Completes the 7-day episodic memory visualization. The single Wednesday yellow entry followed by immediate return to green demonstrates correct non-escalation: one isolated yellow day does not meet the 3-consecutive-day threshold. This also provides the historical baseline that makes the staff-facing RED escalation in Screenshots 24–27 clinically significant — the patient recorded as "stable and improving" here is the same patient flagged RED two days later with breathing difficulty 5/5 and strong chest pain.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Key safety design choices): "Three-day yellow trajectories trigger escalation regardless of any individual day's classification, which accounts for gradual health deterioration."
- Phase 3, Section 8 (Failure Analysis, FC-01): the episodic memory history is the context within which Agent B should assess new inputs — the positive history shown here does not justify returning Green for an active bleed reported in free text.

---

## Screenshot 17
**screenshot_file:** `Provider_Page_17.png`

**what_it_shows:**
The "Follow-Up Care — Recommended by your care team" screen shown in the full Figma canvas view with an Agent C: Insurance Liaison sticky note open to the right. The screen shows: a blue info banner ("Your care team suggests a follow-up with a cardiologist within 7 days based on your recent symptoms"), a "Why This Matters" section ("A cardiologist can assess your heart health and ensure your recovery is on track. Early follow-up helps prevent complications"), and under "In-Network Providers Near You": Dr. Michael Rodriguez (Cardiology, 2.3 miles away, $45 copay in-network, next available April 14 2:00 PM, "Request Appointment" button) and Dr. Emily Watson partially visible below. The Agent C sticky note shows: Data Model — `Insurance.plan, ProviderNetwork.directory[], Patient.location, Appointment.history[]`; Navigation Trigger — User taps provider card → pre-populates appointment request with insurance details → sends to provider scheduling system; Display vs. Computed — Display: provider name, specialty, distance, copay (in-network status), availability. Computed: distance from patient location, insurance coverage verification, next available appointment slot.

**why_it_matters:**
This is Agent C's output rendered in the patient UI, with the sticky note making Agent C's full data pipeline explicit for evaluators. The computed fields — distance calculation, insurance coverage verification, appointment slot availability — are exactly the data elements that Failure Case FC-04 exposes as vulnerable to staleness. Agent C reported ACTIVE insurance status based on a March 14 discharge-time snapshot even after the patient's employer-sponsored coverage had lapsed due to a layoff three weeks later. The `Insurance.plan` field in the data model annotation is the precise record that FC-04 shows was never re-verified after discharge.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-04): "Agent C (Insurance Liaison) continued to report the patient's insurance status as Active, relying solely on the initial discharge-time verification. The agent made no attempt to re-query or validate coverage status."
- Phase 3, Section 8 (FC-04, Core Failure): "Agent C has no temporal reasoning about record freshness and no mechanism to detect coverage changes caused by external life events."
- Phase 3, Section 8 (Future Improvement, P2): "Add insurance record staleness detection to Agent C: Define a re-verification cadence (recommended: every 7 days post-discharge). Surface a staleness flag when last-verified date exceeds threshold."
- Phase 3, Section 9 (Risk & Governance): "Incorrect provider information" risk — all provider data timestamped with last-verified date shown to patient; output includes 'confirm with your insurer.'

---

## Screenshot 18
**screenshot_file:** `Care_Assistant_Bot_18.png`

**what_it_shows:**
The Care Assistant chatbot overlay opened on top of the Follow-Up Care provider page (Dr. Michael Rodriguez's card is visible in the dimmed background — Cardiology, 2.3 miles away, $45 copay, "Request Appointment" button). The chatbot panel shows: title "Care Assistant — I can help you find information from your care plan"; a "Try asking:" prompt-suggestion section with four pre-populated question chips: "What medications do I take tonight?", "Where do I pick up my medication?", "Is my prescription ready?", "When is my next appointment?"; and a free-text "Type your question..." input field at the bottom. No message has been sent yet — this is the empty/opening state of the chatbot.

**why_it_matters:**
Shows the Care Assistant in its initial state before a question is asked, demonstrating the suggested-prompt UX designed for low-health-literacy users. The four suggested questions all query Agent A's structured care plan data (medications, pharmacy, appointments) — not a general LLM. Offering pre-written prompts reduces the barrier for patients who may not know how to phrase medical questions, directly addressing the user profile of elderly patients and those with limited health literacy. This screen also demonstrates that the chatbot is accessible from any point in the app, including mid-workflow (the provider page is visible behind it), providing continuous access to care plan information without interrupting the user's current task.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 3 (User Profile): target population includes elderly patients with low health literacy and those without strong in-home support — the suggested-prompt design reduces the cognitive burden of formulating questions.
- Phase 3, Section 5 (Build Summary): "The chat assistant's reach beyond medication pickup" is noted as a demonstration of the patient-facing interface scope; the chatbot is accessible system-wide.
- Phase 3, Section 4 (Architecture): "No LLM is permitted to rely on parametric medical knowledge; every clinical claim must be traceable to a source" — the chatbot queries Agent A's structured outputs rather than generating responses from parametric knowledge.

---

## Screenshot 19
**screenshot_file:** `Care_assistant_Bot_Chat_19.png`

**what_it_shows:**
The Care Assistant chatbot overlay on the home screen (background dimmed, showing "Next appointment: Dr. Sarah Johnson, Cardiology, Tuesday April 15 at 10:00 AM"). The chat panel shows: a patient message bubble in green: "What medications do I take tonight?"; and a system response in white: "You have Atorvastatin 20mg scheduled for 9:00 PM tonight. Remember to take it with food." A free-text "Type your question..." input field is at the bottom.

**why_it_matters:**
Demonstrates the chatbot in its answered state, showing how Agent A's structured medication data (including the foodRequirement field) surfaces through a conversational interface. The food reminder is pulled directly from the medications array, not generated by a general LLM — grounding the response in the patient's specific care plan. The Phase 3 build summary notes that the chat assistant's responses are pre-populated demonstrations rather than live LLM calls, as the data pipeline was sufficient evidence for evaluation purposes.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 5 (Build Summary): "The chat assistant's reach beyond medication pickup" cited as a pre-populated visualization; demonstrating the data pipeline was sufficient rather than requiring live LLM calls for evaluation.
- Phase 3, Section 4 (Architecture): "No LLM is permitted to rely on parametric medical knowledge; every clinical claim must be traceable to a source."

---

## Screenshot 20
**screenshot_file:** `Appointment_Requested_18.png`

**what_it_shows:**
The provider page with a browser-level confirmation dialog overlaid: "An embedded page at [Figma URL] says: Appointment request sent for Dr. Michael Rodriguez — OK." The underlying screen shows the Follow-Up Care page with Dr. Michael Rodriguez's card (Cardiology, 2.3 miles away, $45 copay in-network, next available April 14 2:00 PM) and Dr. Emily Watson partially visible below.

**why_it_matters:**
Demonstrates the appointment request interaction at the point where the patient acts on Agent C's provider recommendation. This screenshot is directly relevant to Failure Case FC-03: the system correctly generates an appointment record linked to a referral, but the failure is that the Orchestrator subsequently closed the referral the next day based on a positive patient self-report ("Feeling fine, no issues to report today"), without verifying whether this appointment was attended. This screenshot marks the precise workflow moment before that failure pathway opens — the referral is open and the appointment is requested, but the Orchestrator has no mechanism to confirm attendance.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-03): "The system appropriately generated a follow-up appointment record linked to this referral. On the following day… the Orchestrator Agent marked the referral as 'Resolved' and closed the case without verifying appointment attendance or triggering any escalation."
- Phase 3, Section 8 (FC-03, Root Cause #1): "No cross-reference logic between referral and check-in events: The provider referral and the patient check-in exist as two independent records in the system with no linkage."
- Phase 3, Section 8 (Future Improvement, P0): "Implement referral-to-check-in cross-reference gate: Open provider referrals must require attendance confirmation before closure. Missed urgent referrals must auto-escalate back to the originating provider."

---

## Screenshot 21
**screenshot_file:** `help_screen_20.png`

**what_it_shows:**
The "Help & Support" screen (Help tab active in navigation). Three sections: (1) "Contact care team" — "Call: (555) 123-4567" (green button) and "Send a message" (outlined button); (2) "Emergency" — "If you experience severe symptoms, call 911 immediately." with a red "Call 911" button; (3) "Resources" — four links: View discharge instructions, Medication information, Activity guidelines, FAQs. The chat assistant FAB is visible.

**why_it_matters:**
Demonstrates the system's core safety principle: the patient always has a direct path to human intervention and emergency services, regardless of what any agent has assessed. The 911 button is available at all times from the Help tab, independent of any Agent B evaluation. This screen enforces the governance rule that the system never discourages seeking care. The discharge instructions and medication information links also allow patients to verify the care plan against the original source document — supporting the auditability principle.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 9 (Risk & Governance): "Over-reliance and care avoidance" risk — "Every output includes a disclaimer. The system never says 'you're fine,' only describes what was reported. Hard rule: never discourage seeking care. Every escalation path includes the 'contact your provider' option."
- Phase 3, Section 4 (Key safety design choices): "A hard-coded front-end red-flag interrupt fires before Agent B runs… the takeover renders in under one second with zero LLM calls on the acute path."

---

## Screenshot 22
**screenshot_file:** `Care_Coordinator_Landing_Page_21.png`

**what_it_shows:**
The staff-facing "Patient triage queue" screen, dated Saturday April 11, 2026. A filter row allows sorting by All / Red / Yellow / Green. Four patient cards are visible: Margaret Chen, 72 (RED badge — "Severe chest pain reported," flagged 2 hours ago, last check-in Today 10:15 AM); Robert Williams, 68 (RED badge — "Breathing difficulty = 5," flagged 45 minutes ago, last check-in Today 12:30 PM); Sarah Martinez, 65 (YELLOW badge — "3-day dizziness trajectory," flagged 3 hours ago, last check-in Today 9:45 AM); James Thompson, 71 (YELLOW badge, partially visible). An Orchestrator + Agent B sticky note is open: Data Model — `patients[] { id, name, age, latestCheckIn, escalationHistory[], severity }`; Navigation Trigger — staff opens triage queue → Orchestrator fetches all active patients → sorts by severity (red → yellow → green) → orders by flaggedTime; Display vs. Computed — Display: patient cards sorted red-first. Computed: Orchestrator queries database → Agent B provides severity for each patient → Orchestrator sorts and renders.

**why_it_matters:**
This is the central command view for clinical staff. The severity-sorted queue directly implements the Orchestrator's deterministic escalation routing: RED cases appear first (highest urgency), followed by YELLOW. The annotation makes explicit that this is a computed sort — the Orchestrator aggregates Agent B's per-patient severity classifications and renders them in priority order, not a UI decision. Sarah Martinez's yellow card ("3-day dizziness trajectory") is the staff-facing representation of exactly the trajectory escalation case shown in Screenshot 12.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Deterministic Orchestrator): "Every transition between agents is triggered by an explicit, pre-approved condition… in a clinical setting, routing logic must be auditable by a clinical team, and an LLM router cannot be reliably audited."
- Phase 3, Section 4 (Architecture): "The Orchestrator Agent mediates all data flow between agents, manages workflow state, and writes results to the patient record."
- Phase 3, Section 6 (Evaluation Framework): staff dashboard is the secondary stakeholder surface displaying Agent B outputs for clinical review.

---

## Screenshot 23
**screenshot_file:** `Patient_Level_Info_Screeb_22.png`

**what_it_shows:**
Margaret Chen's individual patient detail page in the staff view. Header: "Margaret Chen, 72 — Cardiac surgery (CABG) — RED — Flagged 2 hours ago." Two action buttons: "Mark as reviewed" and "Contact patient." A "Medication and care plan context" section lists current medications (Lisinopril 10mg once daily ACE inhibitor; Aspirin 81mg once daily with food; Atorvastatin 20mg once daily at bedtime; Metoprolol 25mg twice daily beta-blocker) with a yellow warning box: "Note: Beta-blocker may explain mild fatigue but should not cause acute chest pain or severe breathing difficulty." The beginning of a "Structured assessment" section is visible at the bottom.

**why_it_matters:**
Shows Agent A's medication data cross-referenced with Agent B's contradicting indicators on the staff-facing page. The beta-blocker warning note is Agent B's cross-reference output: fatigue is an expected side effect of Metoprolol, but acute chest pain and severe dyspnea are not — making these contradicting indicators that drive Red escalation. This same cross-reference logic is what Failure Case FC-05 broke: the adherence module did not query the hold condition registry or wearable data, so it flagged a physician-authorized dose skip as non-adherence and sent a false alert to Nurse Rivera.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Architecture): "Agent B uses GPT-4o for assessment, grounded in retrieval from a curated clinical knowledge base (FAISS vector store) of post-cardiac-surgery recovery guidelines, and cross-referenced against OpenFDA for medication side effects."
- Phase 3, Section 8 (Failure Analysis, FC-05): "Despite both objective physiological data and a valid clinician-issued directive supporting the patient's action, the system classified the event as Non-Adherent… adherence logic fails to query clinician-issued hold instructions."
- Phase 3, Section 6 (Evaluation, Case 9): demographic blinding — medication context passed to Agent B retains clinically necessary fields (age, diagnosis, comorbidities, medications) while stripping race, language, and insurance.

---

## Screenshot 24
**screenshot_file:** `More_Patient_Level_Info_23.png`

**what_it_shows:**
Margaret Chen's detail page scrolled to show the full "Structured assessment" section (Agent B's JSON output rendered in readable form): Reported symptoms: "Severe breathing difficulty (5/5), strong chest pain, moderate swelling." Expected indicators: "Mild fatigue from beta-blocker, gradual improvement in mobility." Contradicting indicators: "Acute worsening of respiratory and cardiac symptoms not explained by current medications." Severity: "RED — Immediate attention required." Escalation reason: "Red flag symptoms (breathing = 5, chest pain = strong)." An Agent B sticky note is open to the right with full data model, navigation trigger, and Display vs. Computed annotation.

**why_it_matters:**
This is the most clinically detailed screenshot in the prototype, showing Agent B's full structured assessment JSON rendered for clinical staff. It demonstrates: (1) the contradicting indicators field driving escalation beyond simple severity scoring; (2) plain-language translation of JSON fields for nurse-readable output; (3) the exact fields logged to the audit trail for every escalation. This screen represents the correct behavior that Failure Case FC-01 failed to produce: FC-01 returned Green with no contradicting indicators for a patient actively bleeding — the direct opposite of what is shown here.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-01): expected behavior was "Ignore positive sentiment framing. Parse each clinical clause independently. Map 'incision opened and bleeding heavily' to Red severity. Escalate immediately." — this screenshot shows what that correct output looks like.
- Phase 3, Section 9 (Risk & Governance): "False positive alerts" risk — "Escalation summaries include structured reasoning for rapid triage." The structured assessment JSON shown here is that reasoning.
- Phase 3, Section 7.2 (Ongoing Mitigation): "Escalation logging — Every escalation is logged with triggering data, agent assessment, severity classification, and outcome for auditing escalation quality."

---

## Screenshot 25
**screenshot_file:** `More_patient_info_24.png`

**what_it_shows:**
Margaret Chen's detail page scrolled to show the "Recommended action" section. A red-bordered card reads: "Recommended action — Nurse contact recommended immediately. Consider same-day evaluation for possible post-operative complications (PE, cardiac event, infection)." Two action buttons in red: "Call patient now" and "Send to ER." The escalation reason ("Red flag symptoms — breathing = 5, chest pain = strong") is partially visible at the top.

**why_it_matters:**
Shows the highest-severity clinical action surface in the prototype. The "Call patient now" and "Send to ER" options are the human-in-the-loop actions the system surfaces without taking autonomously — the nurse decides. This is the hard constraint the governance plan enforces: no autonomous clinical action. This screen is also directly referenced in Failure Case FC-03's evaluation: the same recommended action card appeared for the urgent cardiology referral that was subsequently closed by the Orchestrator based on a positive patient self-report the next day, without requiring attendance confirmation.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 9 (Risk & Governance): "Unauthorized autonomous action" risk — "Hard rule: read-only on medical orders. Agents are only given read access to external systems; no tools for creating, modifying, or deleting data in external systems are made available."
- Phase 3, Section 4 (Human intervention built into the workflow): "Every Agent B escalation is a hard handoff to the triage nurse."
- Phase 3, Section 8 (Failure Analysis, FC-03): the "Recommended action" card with "Call patient now" and "Send to ER" is shown in the FC-03 screenshot — the same screen whose associated referral was later improperly closed by positive self-report.

---

## Screenshot 26
**screenshot_file:** `More_patient_info_25.png`

**what_it_shows:**
Margaret Chen's detail page showing the "7-day symptom trajectory" chart for the staff view. Seven colored circles: Mon (green), Tue (green), Wed (yellow/orange), Thu (green), Fri (green), Sat (yellow/orange), Sun (red). Three day-level drill-downs: Fri — green dot, Breathing: 1, Pain: 1, Swelling: 1; Sat — yellow dot, Breathing: 3, Pain: 2, Swelling: 2; Sun — red dot, Breathing: 5, Pain: Strong, Swelling: 3.

**why_it_matters:**
This is the staff-facing episodic memory visualization that enables trajectory-based clinical reasoning. The Fri → Sat → Sun deterioration (green → yellow → red) is the multi-day pattern the architecture is designed to detect, matching the evaluation Case 5 scenario. The nurse can see the escalation built over days rather than appearing suddenly. The drill-down vitals per day are the structured fields from the append-only episodic memory entries. This visualization captures exactly the kind of gradual post-CABG heart failure trajectory (dyspnea + edema) described in the evaluation — the system is designed to surface this before it becomes an acute emergency.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 4 (Memory model): "Episodic memory is an append-only log of daily check-ins; Agent B retrieves a 7-day rolling window to detect trajectories, an interval grounded in the post-surgical readmission risk literature (Jencks et al., 2009)."
- Phase 3, Section 6 (Evaluation, Case 5): Episodic Memory & Gradual Deterioration — "Agent B retrieves the 7-day episodic memory. It performs multi-step reasoning, correlating dyspnea with edema to identify a classic post-CABG heart failure trajectory. Escalation Triggered by Day 5 = True. False Negative Rate = 0."
- Phase 3, Section 4 (Key safety design choices): "Three-day yellow trajectories trigger escalation regardless of any individual day's classification."

---

## Screenshot 27
**screenshot_file:** `More_Patient_info_26.png`

**what_it_shows:**
Margaret Chen's detail page showing a "Why this was flagged" plain-language reasoning section. Text reads: "Patient reported severe breathing difficulty (5/5) and strong chest pain during today's check-in. This represents a significant worsening from yesterday's assessment. Beta-blocker may explain some fatigue, but acute chest pain and severe dyspnea are not expected at this stage of recovery." Two tag chips: "Symptom trajectory" and "Contradicting indicator." The beginning of the "7-day symptom trajectory" section is visible below. An Agent B sticky note is open to the right with the full data model, navigation trigger, and Display vs. Computed annotation.

**why_it_matters:**
Shows the human-readable reasoning layer that bridges Agent B's structured JSON and the clinical decision the nurse must make. The "Why this was flagged" section is the plain-language translation of the `reasoning` field in Agent B's output — making AI reasoning auditable and interpretable without requiring clinical staff to read raw JSON. The "Symptom trajectory" and "Contradicting indicator" tag chips categorize the two distinct escalation triggers, enabling rapid triage. This screen is the most direct evidence in the prototype that the system's reasoning is traceable to a specific agent and grounding source — the core clinical safety requirement the architecture is built around.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 7.2 (Ongoing Mitigation): "Escalation logging — Every escalation is logged with triggering data, agent assessment, severity classification, and outcome for auditing escalation quality."
- Phase 3, Section 9 (Risk & Governance): "False positive alerts" risk mitigation — "Escalation summaries include structured reasoning for rapid triage."
- Phase 3, Section 8 (Failure Analysis Summary): "In every failed case, the agent either over-relied on a single input source, lacked cross-referencing capability, or applied incorrect logic that suppressed a warranted escalation" — this screen shows what warranted, correctly-reasoned escalation looks like.
- Phase 3, Section 4 (Architecture): "No LLM is permitted to rely on parametric medical knowledge; every clinical claim must be traceable to a source."

---

---

# Failure Case Screenshots Index

The five screenshots below document the failure cases identified during structured QA evaluation of the multi-agent system (Phase 3, Section 8). Each image is a composite evaluation artifact — not a single prototype screen — showing the input submitted, the system's actual output, and an annotated failure analysis panel. They are included here as part of the screenshot index because they are submitted alongside the prototype screenshots and are essential for evaluators reviewing the system's safety boundaries.

Each entry follows the same structure as the prototype screenshots above.

---

## Failure Case Screenshot FC-01
**screenshot_file:** `failedcase1.png`

**what_it_shows:**
A composite evaluation image titled "Failed Test Case Example: Self-report distortion (Noise Filtering)." Left side: the check-in Step 2 of 2 free-text field showing the submitted input — "Feeling great! But my incision opened and is bleeding heavily." — with the field outlined in red to highlight the problematic input. Center annotation in red text: "The user input opens with strong positive sentiment 'Feeling great', before saying their incision is bleeding heavily" and "The test checks whether Agent B can separate emotional tone from clinical content and assign severity based purely on the medical facts." Right side: the "Check-in complete" result screen showing a green banner with the checkmark and text "Looks stable today" (with smaller subtitle text: "Your symptoms are within expected range for day 7 of recovery") — the incorrect output. Far right: a pink annotation box labeled "Agent B (Symptom Evaluator) Decision Logic Failure" explaining: "Agent B was expected to ignore the positive tone and escalate to Red for the active wound bleed. A Strong positive sentiment at the start of a message is enough to suppress even an explicit, unambiguous Red-level clinical signal later in the same sentence. That's the core failure."

**why_it_matters:**
This is the most critical of the five failure cases — rated SEVERITY: CRITICAL — Patient Safety in the report. It provides visual proof that the system returned Green ("Looks stable today") for an active, heavy post-surgical wound bleed. The image shows exactly what a patient would see after submitting this input: a reassuring green checkmark and a message that their symptoms are within expected range, with no escalation and no nurse alert. This false negative has potential for serious patient harm and is classified as a blocking issue that must be remediated before the platform operates in a live clinical environment. The composite format — input screen, annotation, output screen, failure analysis — makes the failure immediately legible for clinical and engineering reviewers.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-01): full case write-up — background, observed behavior, expected vs. actual decision logic table, root cause findings (sentiment weighting overrides clinical content; no sequential clause parsing; absent keyword-to-severity hard mapping).
- Phase 3, Section 8 (FC-01, Root Cause #1): "Sentiment weighting overrides clinical content: Agent B detected strong positive opening sentiment ('Feeling great') and applied sentiment-weighted scoring that suppressed subsequent clinical signals in the same message."
- Phase 3, Section 8 (FC-01, Root Cause #3): "Absent keyword-to-severity hard mapping: Phrases such as 'bleeding heavily' and 'incision opened' did not trigger a hardcoded severity floor, allowing soft sentiment scoring to dominate."
- Phase 3, Section 8 (Main Takeaway — Failure Theme): "Sentiment Contamination of Clinical Logic — NLP sentiment signals are applied to the full message and allowed to influence clinical severity scoring, creating a suppression pathway for serious symptoms framed within positive language."
- Phase 3, Section 8 (Future Improvement, P0): "Implement clinical keyword hard floors in Agent B: Map terms including 'bleeding heavily,' 'incision opened,' 'chest pain,' and 'can't breathe' to a minimum Red severity that cannot be overridden by sentiment scoring."
- Phase 3, Section 8 (Final paragraph): "Two cases — FC-01 (active wound bleed classified as stable) and FC-03 (urgent cardiac referral closed on patient self-report) — carry direct patient safety implications and must be treated as blocking issues."

---

## Failure Case Screenshot FC-02
**screenshot_file:** `failedcase2.png`

**what_it_shows:**
A composite evaluation image titled "Failed Test Case Example: Unknown Drug (State Tracking)." Left side: a staff-facing patient message panel for Margaret Chen (Day 12 post-discharge, Coreg 25mg BID) showing the patient message "Took my Coreg this morning, feeling okay." (submitted Apr 25, 2026 at 08:14), NLP sentiment flagged as Positive, Agent B severity flagged as Green — with a red annotation "Patient self-reported they took Coreg (Prescription Only)." Below this: a pharmacy record panel showing CVS Pharmacy Rx #4471829 — Last dispensed: Mar 26, 2026; Days since refill: 30 days; Supply remaining: Est. 0 pills — with a red annotation: "There's no pharmacy refill record for 30 days — meaning patient doesn't have enough supply." Status shows "Under review." Right side: a pink annotation box labeled "Orchestrator Agent Decision Logic Failure" showing three numbered steps: (1) Patient message received — Agent B returned Green, no clinical flags; (2) Pharmacy cross-check — Not performed, orchestrator did not query pharmacy source; (3) Final state written — Marked Adherent falsely: trusted patient message only. A footer note reads: "Patient's self-report is not just unverified, it's materially impossible given the dispensing record. The orchestrator should treat this as a critical non-adherence flag, not a discrepancy to note."

**why_it_matters:**
Provides visual proof of the Orchestrator accepting a self-report as ground truth when it was materially impossible — the pharmacy record shows zero pills remaining, making the claimed dose physically impossible to have taken. The image clearly separates the two data sources (patient message panel vs. pharmacy record panel) that the system failed to cross-reference, and shows that the final adherence state written was "Adherent" despite the contradiction. Cardiovascular medication non-adherence (Coreg is a beta-blocker) carries significant post-discharge risk. Rated SEVERITY: HIGH — Adherence Integrity.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-02): full case write-up — background, observed behavior, expected vs. actual decision logic table, root cause findings.
- Phase 3, Section 8 (FC-02, Root Cause #1): "No pharmacy cross-check initiated: The Orchestrator did not query the pharmacy data source after receiving the patient's self-report. The workflow routed directly from Agent B's Green output to state write."
- Phase 3, Section 8 (FC-02, Root Cause #2): "Self-report accepted as ground truth: The system architecture treats patient-reported adherence as sufficient evidence when uncontradicted, without validating against dispensing records."
- Phase 3, Section 8 (Main Takeaway — Failure Theme): "Single-Source Trust — Agents accept patient self-reports as ground truth without cross-referencing pharmacy records, wearable data, or clinical notes. When the single source is unreliable or deliberately positive, no failure mode is activated."
- Phase 3, Section 8 (Future Improvement, P1): "Add pharmacy dispense cross-check to adherence workflow: When a patient self-reports taking a medication, the Orchestrator must query the pharmacy record. Zero estimated pills remaining must trigger a critical non-adherence flag."

---

## Failure Case Screenshot FC-03
**screenshot_file:** `failedcase3.png`

**what_it_shows:**
A composite evaluation image titled "Failed Test Case Example: Provider-Requested Appointment Not Attended." Left side: a staff-facing panel showing the prior escalation — Severity: RED — Immediate attention required; Escalation reason: Red flag symptoms (breathing = 5, chest pain = strong); Recommended action: "Nurse contact recommended immediately. Consider same-day evaluation for possible post-operative complications (PE, cardiac event, infection)." with "Call patient now" and "Send to ER" buttons. Right side: two panels showing the failure. Top panel: "Latest patient check-in" — Apr 25, 2026 · 9:02 AM — "Feeling fine, no issues to report today." — Agent B: Green · Sentiment: Positive. Bottom panel: "Follow-up status" — a green "Resolved" badge — Apr 25 · 9:02 AM — "Provider referral marked closed · No escalation." A red annotation reads: "Nurse Rivera's urgent cardiology referral was marked closed based on a patient self-report next-day." Top right: a pink annotation box labeled "Agent A (Event Scheduler Agent) Failure" explaining: "The provider referral and the patient check-in exist as two separate events in the system with no cross-reference logic between them. No escalation triggered back to Nurse Rivera despite the referral being flagged as urgent and the visit having no attendance record."

**why_it_matters:**
Shows how a Red-level urgent cardiology referral — issued by Nurse Rivera for possible pulmonary embolism and acute cardiac events — was silently closed by the Orchestrator because the patient said "Feeling fine" the next day. The "Resolved" green badge is the most alarming element: it presents a dangerous false closure as a normal, completed event. The image juxtaposes the prior RED recommendation (same-day evaluation for PE, cardiac event, infection) with the Green resolution triggered by positive sentiment — making the failure's clinical stakes immediately visible. Rated SEVERITY: CRITICAL — Patient Safety and classified as a blocking issue alongside FC-01.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-03): full case write-up — background, observed behavior, expected vs. actual decision logic table, root cause findings.
- Phase 3, Section 8 (FC-03, Root Cause #1): "No cross-reference logic between referral and check-in events: The provider referral and the patient check-in exist as two independent records in the system with no linkage. Resolving a check-in does not trigger a check against open referrals."
- Phase 3, Section 8 (FC-03, Root Cause #2): "Patient self-report used to close provider-initiated referral: The system allowed a subjective patient check-in ('feeling fine') to satisfy the follow-up requirement for an objectively-flagged Red-level clinical event."
- Phase 3, Section 8 (FC-03, Root Cause #3): "Absence of attendance record did not trigger flag: The system required a positive signal to keep the referral open, rather than requiring positive confirmation of attendance to close it."
- Phase 3, Section 8 (Main Takeaway — Failure Theme): "Missing Cross-Reference Logic — Events that should be linked (referral + check-in; self-report + pharmacy dispense; stated reason + hold condition) exist as independent records with no orchestration layer to detect contradictions."
- Phase 3, Section 8 (Future Improvement, P0): "Implement referral-to-check-in cross-reference gate: Open provider referrals must require attendance confirmation before closure. Missed urgent referrals must auto-escalate back to the originating provider."
- Phase 3, Section 8 (Final paragraph): "FC-03 (urgent cardiac referral closed on patient self-report) — carries direct patient safety implications and must be treated as a blocking issue. The platform should not operate in a live clinical environment until the P0 remediation actions are implemented and re-tested."

---

## Failure Case Screenshot FC-04
**screenshot_file:** `failedcase4.png`

**what_it_shows:**
A composite evaluation image titled "Failed Test Case Example: Group Number Expired (Insurance)." Left side: a staff-facing insurance panel for Margaret Chen (ID: MHC-2291047, Post-discharge day 22, Acme Corp Group PPO). The "Coverage status" card shows a green "ACTIVE" badge with Group #: ACM-44892 · Acme Corp PPO and "Last verified: Mar 14, 2026 (discharge date)" — outlined in a red box to highlight the stale verification date. Below: "Employment & coverage history" showing "Acme Corp Group PPO (assumed active)" — Discharge date: Mar 14, 2026; Employment status: Active (last checked: Mar 14); Group # last verified: Mar 14, 2026; New plan on file: None. Below: "Recent prescription activity" showing Metoprolol 50mg dispensed Apr 5 · CVS Pharmacy #0482; Lisinopril 10mg dispensed Apr 11 · CVS Pharmacy #0482; Atorvastatin 20mg dispensed Apr 19 · CVS Pharmacy #0482. Right side: a pink annotation box labeled "Agent C (Insurance Liaison) Retrieval Failure" with two numbered points: (1) Patient's Employment Insurance group plan invalidated 3 weeks post-discharge due to layoff. New individual coverage not yet activated. Insurance status last verified at discharge. (2) Agent C has no mechanism to re-query employment or coverage status after that point. A footer note reads: "From Agent C's perspective, the group number ACM-44892 is still active because nothing told it otherwise."

**why_it_matters:**
Shows the static snapshot assumption failure in its most concrete form: the ACTIVE badge and the discharge-date timestamp are visible side-by-side in the same panel, making it immediately apparent that the "last verified" date is 22 days old. Three prescription dispenses (Apr 5, Apr 11, Apr 19) may have been processed under a lapsed group number — exposing the patient to retroactive claim denials for medications she needs for post-CABG recovery. The "nothing told it otherwise" summary captures the root cause: Agent C has no mechanism to subscribe to or poll employment status changes. Rated SEVERITY: MEDIUM — Administrative.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-04): full case write-up — background, observed behavior, expected vs. actual decision logic table, root cause findings.
- Phase 3, Section 8 (FC-04, Root Cause #1): "No post-discharge re-verification trigger: Agent C has no mechanism to re-query insurance or employment status after the initial discharge snapshot. Coverage is assumed static unless externally updated."
- Phase 3, Section 8 (FC-04, Root Cause #2): "Staleness not surfaced to downstream agents: The 'Last verified: March 14' timestamp was available in the record but was not evaluated against a staleness threshold or flagged as potentially outdated."
- Phase 3, Section 8 (FC-04, Root Cause #3): "No employment status change listener: Agent C does not subscribe to or poll employment status events. A layoff — a foreseeable post-discharge event — is entirely outside the agent's observable state."
- Phase 3, Section 8 (Main Takeaway — Failure Theme): "Static Snapshot Assumptions — Agent state is captured at a point in time (typically discharge) and assumed valid indefinitely. No staleness detection, re-verification cadence, or change-event subscription exists."
- Phase 3, Section 8 (Future Improvement, P2): "Add insurance record staleness detection to Agent C: Define a re-verification cadence (recommended: every 7 days post-discharge). Surface a staleness flag when last-verified date exceeds threshold. Alert administrative team."

---

## Failure Case Screenshot FC-05
**screenshot_file:** `failedcase5.png`

**what_it_shows:**
A composite evaluation image titled "Failed Test Case Example: Medication Coherence (Skipped Dose — Physician Instructed)." Left side (patient-facing app): check-in screen for Margaret Chen, Day 22 post-discharge, Metoprolol 50mg due 8:00 AM daily. Shows: (1) "Heart rate · recorded 7:52 AM" — a heart rate gauge displaying 48 bpm in red text, positioned in the Low range on a Low/Normal (60-100)/High scale; (2) "Physician hold instruction" — a HOLD badge followed by "if resting HR < 50 bpm — Instructed by: Dr. Alan Torres · Cardiology — Issued: Mar 14, 2026 · Active condition"; (3) "Patient check-in message" — submitted Apr 25, 2026 · 8:06 AM — "Skipped my Metoprolol this morning — my heart rate was too low like doc said." — outlined in red. Center panels: "Medication status" showing Metoprolol 50mg — 8:00 AM dose: Not taken — Hold condition met: HR = 48 bpm (threshold: <50); "System response" — a red "Non-adherence flagged" badge — Alert sent to: Nurse Rivera — Reason: Metoprolol dose missed at 8:00 AM. Center annotation: "Patient wears an Apple Watch and showed their heartrate is below 48bpm. The physician told the patient in person that they should skip the pills when their heart rate is below 50 bpm." Right side: a pink annotation box — "adherence logic fails to query clinician-issued hold instructions" — with two numbered points: (1) Wearable HR cross-check not performed · Apple Watch reading of 48 bpm never fetched or compared against hold threshold; (2) Patient reason evaluated Ignored · "HR too low" treated as free text, not mapped to active hold condition — Final state written Marked Non-Adherent falsely · dose skip was clinically correct per doctor's hold instruction.

**why_it_matters:**
Shows all three required data elements visible on one screen — the Apple Watch reading (48 bpm), the active physician hold instruction (hold if HR < 50 bpm), and the patient's check-in message explaining the skip — yet the system ignored all of them and sent a false non-adherence alert to Nurse Rivera. The image makes the multimodal integration failure concrete: the wearable data, the hold condition registry, and the free-text patient reason all existed in the system but were not cross-referenced by the adherence logic. This failure is particularly damaging to system trust: the patient did exactly what their physician instructed, was flagged as non-compliant, and Nurse Rivera may have erroneously counseled them to take a medication that is contraindicated at their current heart rate. Rated SEVERITY: HIGH — Adherence Integrity.

**where_it_is_discussed_in_the_report:**
- Phase 3, Section 8 (Failure Analysis, FC-05): full case write-up — background, observed behavior, expected vs. actual decision logic table, root cause findings.
- Phase 3, Section 8 (FC-05, Root Cause #1): "Wearable data not fetched for adherence evaluation: The Apple Watch heart rate reading of 48 bpm was available via wearable integration but was never queried or compared against the hold condition threshold during adherence processing."
- Phase 3, Section 8 (FC-05, Root Cause #2): "Patient-stated reason treated as free text, not parsed against active hold conditions: The phrase 'my heart rate was too low like doc said' was not mapped to the active physician hold instruction in the system, despite an exact match in rationale."
- Phase 3, Section 8 (FC-05, Root Cause #3): "Hold condition not integrated into adherence decision path: The adherence logic evaluates dose taken vs. dose scheduled without checking whether an active hold condition permits or requires omission."
- Phase 3, Section 8 (Main Takeaway — Failure Theme): "Single-Source Trust — Agents accept patient self-reports as ground truth without cross-referencing pharmacy records, wearable data, or clinical notes."
- Phase 3, Section 8 (Main Takeaway — Failure Theme): "Missing Cross-Reference Logic — Events that should be linked (self-report + pharmacy dispense; stated reason + hold condition) exist as independent records with no orchestration layer to detect contradictions."
- Phase 3, Section 8 (Future Improvement, P1): "Integrate hold condition registry into adherence decision logic: Agent B must query active physician hold conditions before evaluating adherence. Wearable data must be fetched and compared against hold thresholds."

---

*End of Screenshot Index — 27 prototype screenshots + 5 failure case screenshots documented.*
*Generated for Phase 3 submission of the Post-Discharge Care Companion project.*
