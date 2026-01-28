# EXP-02: Pivot Experiment (Solid Lipid Nanoparticles)

**Code**: `EXP-02-PIVOT`
**Origin**: Pivot from failed SEP-03 (Liposomal SR9=0.25).
**Objective**: Identify a formulation with SR9 > 0.80 (Stable).

## 1. The Pivot Hypothesis
*   **Concept**: **Solid Lipid Nanoparticles (SLN)**.
*   **Rationale**: Unlike fluid liposomes, SLNs possess a solid core matrix at room temperature, physically entrapping the drug and preventing leakage (Drift).
*   **Draft Hypothesis**: "Encapsulating Upadacitinib within a Cetyl Palmitate-based Solid Lipid Nanoparticle (SLN) matrix ensures thermodynamic stability and sustained release, minimizing systemic exposure."

## 2. Execution Pipeline

### A. Input Generation
*   Target: `d:\Sanctum\렉스바이오 파이프라인\rexsyn_input_exp02.json`
*   Content: Structure definition for SLN + Upadacitinib.

### B. Bridge Execution (Reuse)
*   **Step 1**: Run `run_rexsyn_sep03.py` (Modified to accept generic input arg or I will clone it to `run_rexsyn_generic.py`).
    *   *Decision*: Evaluation shows `run_rexsyn_sep03.py` is hardcoded to `rexsyn_input_sep02.json`.
    *   *Action*: Refactor bridge scripts to accept CLI arguments for flexibility. `run_rexsyn_bridge.py <input_json> <output_json>`.

### C. Orchestrator (`run_exp02_orchestrator.py`)
*   Automates the flow: Input -> RExSyn -> NNSL -> Report.

## 3. Success Criteria
*   **RExSyn**: Validated (Grade A/S).
*   **NNSL**: **SR9 Score > 0.80** (The Critical Gate).

## 4. Deliverables
*   `exp02_nnsl_output.json`
*   `EXP02_PIVOT_REPORT.md`
