# ▶️ Simulation & Executable MBSE

[← Back to main README](../README.md)

---

### Q: What is "executable MBSE," and how does it differ from a traditional SysML model that's used purely for documentation and diagramming purposes?

**Answer:**
A traditional, non-executable SysML model captures structure, behavior, and requirements as **static diagrams and relationships** — valuable for documentation, communication, and traceability, but the behavioral diagrams (state machines, activities) are not actually "run" — they're read and interpreted by humans. **Executable MBSE** goes further, using precisely-defined behavioral semantics (often via a foundational subset like fUML — Foundational UML, or a simulation-capable profile) that allow the model's behavior to actually be **simulated/executed** by a tool, producing concrete, observable outputs for given inputs — transforming the model from a purely descriptive artifact into something closer to an **early, analyzable prototype** of the system's actual behavior, enabling the kind of dynamic verification (discussed in the behavioral modeling topic) that static diagram review alone cannot provide.

---

### Q: What is the value of building and executing a simulation of a system's behavior model early in the design process, before physical prototypes or code exist, in terms of the cost of finding and fixing defects?

**Answer:**
This connects to a foundational systems engineering principle: the cost of fixing a defect grows dramatically the later in the lifecycle it's discovered (a requirements/design defect caught during early modeling might cost a small fraction of what the same defect would cost to fix if discovered during integration testing or, worse, after fielding). Executing a behavioral model simulation early allows defects in the **logic, timing, and interaction patterns** of the design to be discovered and corrected **before** any physical hardware or software implementation investment has been made — e.g., discovering via simulation that a proposed control logic has a race condition or an unreachable state, and correcting the model, is vastly cheaper than discovering the same underlying design flaw after it's been implemented in embedded software and integrated with physical hardware, at which point the fix requires touching actual code, potentially re-testing integrated hardware, and possibly re-certifying affected components.

---

### Q: What is Discrete Event Simulation (DES), and why is it a commonly used simulation paradigm for systems engineering models compared to continuous-time simulation?

**Answer:**
Discrete Event Simulation advances simulated time by jumping directly from one significant **event** to the next (rather than advancing in small, fixed time increments the way a continuous-time physics simulation typically does), processing each event's effects and scheduling any resulting future events, then jumping to the next scheduled event's time. This is commonly used for systems engineering models because much of systems-level behavior (state transitions, message exchanges, process/workflow steps) is naturally **event-driven** rather than requiring fine-grained continuous physical dynamics modeling (which is the domain of the physics simulation discussed in the perception/simulation engineering repo in this series) — DES efficiently simulates long time periods with sparse, meaningful events (skipping directly between events rather than wastefully computing many small time-steps where nothing of interest is happening), making it well-suited to the kind of logical/behavioral systems engineering models (state machines, activity flows) that don't require detailed continuous physical dynamics simulation.

---

### Q: What is Model-in-the-Loop (MIL), Software-in-the-Loop (SIL), and Hardware-in-the-Loop (HIL) testing, and how do these represent progressively increasing levels of implementation fidelity in a verification strategy?

**Answer:**
**Model-in-the-Loop (MIL)** tests a purely simulated model of both the system under test and its environment — the earliest, cheapest, and lowest-fidelity stage, useful for early algorithm/logic validation before any real implementation exists. **Software-in-the-Loop (SIL)** replaces the simulated model of the system under test with the **actual production software/code** (still running against a simulated environment/hardware), validating that the real software implementation behaves consistently with the model's intended behavior. **Hardware-in-the-Loop (HIL)** further replaces simulated hardware with **actual physical hardware** (with only the broader environment/other systems still simulated), validating real hardware-software integration before full system-level integration. This progression — MIL → SIL → HIL → full system integration testing — allows defects to be caught and isolated at progressively increasing (and progressively more expensive) levels of implementation fidelity, with each stage designed to catch issues specific to the fidelity increase it introduces (e.g., a MIL-to-SIL transition specifically validates that the actual code correctly implements the modeled algorithm/logic) rather than deferring all such validation to a single, expensive, hard-to-diagnose full integration test at the very end.

---

### Q: What is the "M&S (Modeling and Simulation) credibility" or "verification, validation, and accreditation (VV&A)" concern specifically for simulation models used to support systems engineering decisions, and why does a simulation model itself need its own validation before its results can be trusted for program decisions?

**Answer:**
Just as the system under development needs verification/validation (discussed in the next topic), a **simulation model used to make decisions about that system** needs its own credibility established — otherwise, decisions might be made based on a simulation that, unbeknownst to decision-makers, doesn't actually accurately represent the real system/environment being modeled. VV&A specifically addresses this: **verification** confirms the simulation model is correctly implemented according to its own specification (the simulation software correctly implements the intended equations/logic), **validation** confirms the simulation model's behavior sufficiently matches real-world behavior for its intended use (comparing simulation predictions against real test data where available), and **accreditation** is the formal determination/certification by a responsible authority that a specific simulation is acceptable for use in a specific decision context. This matters because a program that uncritically trusts an unvalidated simulation's results for a significant design or verification decision risks building unwarranted confidence on a potentially flawed analytical foundation — exactly analogous to the sim-to-real gap concerns discussed in the Perception & Simulation Engineer repo in this series, applied here to systems engineering decision-support simulations specifically.

---

### Q: What is the difference between using a simulation for "design exploration" (early, iterative trade studies) versus for "formal verification" (providing evidence a requirement has been satisfied), and why might the credibility/fidelity bar for these two uses reasonably differ?

**Answer:**
**Design exploration** simulation supports early, iterative comparison of design alternatives — the goal is relative comparison and insight generation to inform design decisions, where a reasonably representative (but not necessarily perfectly validated) simulation is often adequate, since the specific absolute numbers matter less than the relative comparison between alternatives. **Formal verification** simulation is used to provide **actual evidence that a specific requirement has been satisfied**, feeding directly into the program's formal verification closure record — this generally demands a **much higher, formally established credibility bar** (a properly VV&A'd simulation, per the previous question), since an inadequately validated simulation being used as the sole evidence for closing out a safety-critical requirement's verification represents a genuinely serious risk if the simulation's fidelity turns out to be inadequate for that specific claim. Recognizing this distinction — and not treating every simulation as automatically credible enough for the highest-stakes verification claims just because it was useful for earlier design exploration — is an important, sometimes overlooked engineering judgment call.

---

### Q: What is "parametric sweep" or "Monte Carlo" analysis using an executable systems model, and what specific insight does this provide beyond a single, nominal-case simulation run?

**Answer:**
A single nominal-case simulation run evaluates system behavior under one specific, "typical" set of input parameter values. A **parametric sweep** systematically varies one or more input parameters across a defined range and re-runs the simulation for each combination, revealing how sensitive system behavior/performance is to variation in those specific parameters. A **Monte Carlo** analysis goes further, running many simulations with parameters **randomly sampled from defined probability distributions** (reflecting realistic parameter uncertainty — manufacturing tolerances, environmental variability), producing a **statistical distribution of possible system outcomes** rather than a single deterministic result. This provides insight a single nominal run cannot: understanding whether a design's performance is **robust across the realistic range of parameter uncertainty**, or whether it's only acceptable under an idealized, unrealistically precise nominal-case assumption that real-world manufacturing/environmental variability would actually violate a meaningful fraction of the time — directly informing risk-based design margin decisions rather than relying on a potentially misleading single-point analysis.

---

### Q: What is co-simulation, and why might a systems-level model need to integrate with specialized, discipline-specific simulation tools (e.g., a detailed thermal analysis tool, a specific control-systems simulation environment) rather than attempting to model every engineering discipline's detailed physics natively within the systems model itself?

**Answer:**
Co-simulation coordinates and synchronizes multiple, separately-specialized simulation tools (each an expert tool for its specific engineering discipline — thermal, structural, control systems, electrical) running together, exchanging data at defined synchronization points, to produce an integrated, multi-disciplinary simulation result. This is generally preferred over attempting to model every discipline's detailed physics natively within a single systems-level model because **discipline-specific simulation tools already embody deep, mature, validated expertise** in their specific domain (a dedicated thermal analysis tool has far more sophisticated, validated thermal physics modeling capability than a general-purpose systems engineering tool could practically replicate) — co-simulation lets the **systems model retain its role as the integrating framework** (managing the overall system structure, interfaces, and orchestrating which discipline-specific tools need to exchange which data at which points) while genuinely detailed, high-fidelity physics analysis remains the responsibility of the specialized tools actually built and validated for that specific purpose, avoiding the risky and often impractical alternative of reimplementing deep discipline-specific physics expertise from scratch within a general-purpose systems modeling tool.

---

### Q: How would you decide, for a specific program, how much investment in executable/simulatable MBSE capability is actually warranted, given that building genuinely executable models (versus purely diagrammatic, non-executable ones) requires meaningfully more modeling rigor and tooling investment?

**Answer:**
This decision should be driven by an explicit **cost-benefit assessment specific to the program's actual risk profile and complexity** rather than a blanket assumption that "more executable modeling is always better." Key factors favoring greater investment in executable MBSE: high system complexity with intricate, hard-to-manually-verify behavioral logic (where the risk of undiscovered logic defects is high and costly), safety-critical or high-consequence-of-failure domains (where early defect discovery has outsized value), and a program duration/scale long and large enough to amortize the upfront executable-modeling investment cost across substantial downstream benefit. Factors favoring more modest, primarily-diagrammatic (non-executable) MBSE investment: simpler systems with more straightforward, easily-manually-verified behavioral logic, or smaller/shorter programs where the upfront tooling/skill investment required for genuine executable modeling capability may not be justified by the program's scale — a mature systems engineering organization typically makes this investment decision deliberately and explicitly per-program, rather than either uniformly under-investing (missing genuine value on complex programs that would benefit) or uniformly over-investing (wasting effort building unnecessarily sophisticated executable models for programs simple enough not to need them).

---
