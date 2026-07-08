# 📋 Requirements Engineering & Traceability

[← Back to main README](../README.md)

---

### Q: What makes a requirement "well-formed," and what are the common quality defects (ambiguity, non-verifiability, compound requirements) that a systems engineer should catch during requirements review?

**Answer:**
A well-formed requirement is **clear, unambiguous, verifiable, singular (atomic), and necessary** — expressed such that different readers would interpret it identically and a specific, feasible verification method can be defined for it. Common defects: **ambiguity** (vague terms like "user-friendly" or "fast" without a measurable, specific definition — what specifically constitutes "fast"?), **non-verifiability** (a requirement that can't practically be tested/demonstrated/analyzed/inspected against a clear pass/fail criterion), and **compound requirements** ("the system shall detect and classify and report intrusions within 5 seconds" bundles three distinct capabilities into one requirement statement, making it unclear which specific part failed if verification fails, and complicating clean traceability since a single verification result can't cleanly indicate which of the three bundled capabilities actually passed or failed). Catching these defects during review — ideally with a systematic requirements quality checklist, not just ad-hoc read-through — is far cheaper than discovering them during verification, when a non-verifiable or ambiguous requirement becomes a genuine blocker.

---

### Q: What is the difference between a functional requirement, a performance requirement, and a non-functional (or "quality") requirement, and give an example of each for a simple system like a home thermostat?

**Answer:**
A **functional requirement** specifies a capability the system must provide — e.g., "the thermostat shall allow the user to set a target temperature." A **performance requirement** specifies a quantified level of a capability — e.g., "the thermostat shall maintain the room temperature within ±1°F of the target setting." A **non-functional/quality requirement** specifies a system property not directly tied to a specific functional capability — e.g., "the thermostat's user interface shall be operable by users with limited dexterity" (accessibility) or "the thermostat shall have a mean time between failures of at least 10 years" (reliability). Correctly categorizing requirements this way matters because each category often warrants **different verification approaches** (functional requirements often verified by demonstration/test, performance requirements by test/analysis against a quantified threshold, and some quality requirements verified by inspection/analysis rather than direct functional testing).

---

### Q: What is bidirectional requirements traceability, and why is tracing only "forward" (from requirement to design) insufficient — what specific risk does tracing "backward" (from design/test back to requirements) additionally protect against?

**Answer:**
Bidirectional traceability maintains links in **both directions**: forward from requirements to the design/implementation elements that satisfy them, and backward from design/implementation elements (and test cases) back to the requirements that justify their existence. Forward-only tracing (requirement → design) can confirm every requirement has been addressed, but provides **no visibility into design or test elements that exist without any corresponding requirement** — backward tracing specifically protects against **unintended scope creep or "gold-plating"** (implementing capabilities, or writing test cases, that weren't actually required by any documented requirement, representing wasted effort, uncontrolled cost/schedule risk, and potentially unreviewed/unauthorized functionality) — a mature traceability practice checks both directions: "is every requirement satisfied by something?" AND "does everything in the design/test set trace back to a legitimate requirement?"

---

### Q: What is requirements decomposition/flow-down, and what specific risk arises if a lower-level (derived) requirement isn't correctly traced back to the higher-level requirement it was meant to satisfy?

**Answer:**
Requirements decomposition breaks a high-level system requirement down into more detailed, lower-level requirements allocated to specific subsystems/components — e.g., a system-level "detect intrusions within 5 seconds" requirement might flow down into more specific sensor-level and processing-level timing requirements that, together, are intended to satisfy the parent system-level requirement. If a derived requirement isn't correctly traced (via a `«derive»` relationship, as discussed in the SysML topic) back to its parent, several risks arise: **orphaned requirements** (a lower-level requirement with no clear justification for why it exists, making it hard to know if it's still needed if the higher-level requirement later changes or is removed), and — more insidiously — **incomplete flow-down that isn't detected**, where the actual set of derived lower-level requirements, even if individually well-specified, doesn't collectively and sufficiently satisfy the full intent of the parent requirement, a gap that's much harder to catch without explicit, reviewable trace relationships making the decomposition logic visible and auditable.

---

### Q: What is a requirements management tool's "impact analysis" capability, and walk through a concrete example of how you'd use it when a customer proposes changing a high-level system requirement mid-program.

**Answer:**
Impact analysis uses the model's traceability relationships to automatically identify **everything potentially affected by a proposed change** — every derived requirement, design element, interface, and verification activity connected (directly or transitively) to the element being changed. Concrete example: if a customer proposes changing the system-level "detect intrusions within 5 seconds" requirement to "within 2 seconds," a query against the model's trace relationships would surface every **derived requirement** flowed down from it (sensor response time, processing latency budgets), every **design element** allocated to satisfy those derived requirements (which might now be infeasible under the tighter budget), and every **verification test case** written against the old threshold (which would need updating) — providing program leadership with a **concrete, evidence-based assessment** of the change's actual scope/cost/schedule impact within minutes, rather than requiring a slow, manual, and likely incomplete cross-document search to identify everything the change might touch.

---

### Q: What is the difference between a "shall" statement and a "goal" or "objective" in requirements documentation, and why does mixing these categories within a single requirements baseline create verification/contractual problems?

**Answer:**
A **"shall" statement** represents a mandatory, binding requirement the delivered system must satisfy, subject to formal verification. A **"goal" or "objective"** (sometimes phrased with "should" or "will" rather than "shall") represents a desired but not strictly mandatory outcome — often used to communicate intent or priority without creating a binding contractual/verification obligation. Mixing these categories within a single, undifferentiated requirements baseline creates real problems: **contractual ambiguity** (a dispute about whether a "should" statement was actually mandatory, particularly costly in a contracted development program where verification against the requirements baseline determines contractual completion/payment), and **verification scope creep or gaps** (if goals are inadvertently verified with the same rigor as mandatory requirements, this wastes verification effort; if genuinely mandatory requirements are miscategorized as mere goals, they might not receive adequate verification rigor) — a disciplined requirements baseline explicitly and consistently distinguishes these categories, typically through consistent, controlled use of "shall" for binding requirements and separate, clearly-differentiated language/categorization for goals/objectives.

---

### Q: What is requirements volatility, and how would you use MBSE traceability specifically to help manage (not eliminate — requirements changes are often legitimate and unavoidable) the risk that frequent requirement changes pose to a program's schedule and design stability?

**Answer:**
Requirements volatility refers to the rate/frequency of change to a program's requirements baseline over time — some volatility is normal and often legitimate (evolving customer understanding, discovered technical constraints), but excessive or poorly-managed volatility is a major driver of program cost/schedule overrun. MBSE traceability helps manage this risk (rather than eliminate the underlying volatility, which is often a genuine program reality, not simply a discipline failure) by making the **true cost of each proposed change visible and quantifiable** via the impact analysis capability discussed above — rather than a change being approved or rejected based on an incomplete, informal sense of its scope, decision-makers can see the concrete, traced impact (how many derived requirements, design elements, and tests are affected) before approving a change, supporting **better-informed change control board decisions** and creating a natural, evidence-based friction against low-value or poorly-justified changes, while still enabling legitimate, well-justified changes to proceed with clear-eyed understanding of their actual cost.

---

### Q: What is the difference between requirements "verification" and requirements "validation," and give an example distinguishing a requirement that could pass verification while the underlying system still fails validation.

**Answer:**
**Verification** confirms the system meets its **specified requirements** ("did we build the system right?" — checking the delivered system against the documented requirements baseline). **Validation** confirms the system satisfies the **actual stakeholder need/intended use** ("did we build the right system?" — checking whether the requirements baseline itself, even if perfectly satisfied, actually reflects what the stakeholder genuinely needed). A concrete example: a requirement specifying "the system shall display an alert within 5 seconds of detecting an intrusion" might be perfectly **verified** (the system reliably displays the alert within 5 seconds, precisely as specified) — but if it later turns out that a genuinely useful, safe response actually required the alert within 2 seconds (perhaps a detail the requirements-writing process missed or the stakeholder didn't successfully communicate), the system would still **fail validation**, despite fully passing verification against its own documented requirements — this is exactly why both verification and validation are necessary, complementary activities, and passing verification alone is never sufficient evidence of true program success.

---

### Q: How would you design a requirements review process (peer review, formal inspection) to systematically catch requirements quality defects before a requirement enters the formal baseline, rather than relying purely on ad-hoc individual judgment?

**Answer:**
A systematic requirements review process should use an explicit, structured **quality checklist** (covering the well-formedness criteria discussed earlier — clarity, verifiability, atomicity, necessity) applied consistently by reviewers, rather than relying purely on individual reviewers' ad-hoc judgment/experience, which can vary significantly in what it catches. Effective processes often incorporate: **independent review by someone outside the requirement's original author's team** (an author is often too close to their own requirement to spot ambiguity they didn't intend but a fresh reader would encounter), **explicit verification method identification during the review itself** (forcing the reviewer/author to state how this specific requirement would actually be verified surfaces non-verifiability issues immediately, rather than discovering them much later when an actual verification activity is attempted), and **traceability completeness checks** (does this new requirement properly trace to a parent requirement or stakeholder need, and is it stated at an appropriate level of decomposition) — building this review discipline into a **formal gate** before a requirement enters the controlled baseline is significantly more effective than relying on requirements quality being caught opportunistically or informally later in the program.

---

### Q: What is the concept of a "requirements baseline," and why does establishing and controlling a formal baseline (with a defined change control process) matter significantly more for a large, multi-team, multi-year program than it might for a small, single-team project?

**Answer:**
A requirements baseline is a formally reviewed and approved set of requirements, placed under **configuration/change control**, serving as the fixed reference point against which subsequent design, verification, and change assessment activities are performed. This matters significantly more for large, multi-team, multi-year programs because **many different teams are independently making design decisions in parallel, all depending on a stable, shared understanding of what's actually required** — without a controlled baseline, different teams could easily be working against **different, informally-diverging versions** of "the requirements" (one team having incorporated an informal verbal change that another team never heard about), leading to costly integration mismatches discovered late. For a small, single-team project with tight, constant informal communication, the same risk of divergent, inconsistent requirements understanding across parallel teams simply doesn't exist to nearly the same degree — which is why formal baseline/change control discipline, while always good practice, becomes an operationally essential (not merely a nice-to-have bureaucratic) control specifically as program scale/team-count/duration grows.

---
