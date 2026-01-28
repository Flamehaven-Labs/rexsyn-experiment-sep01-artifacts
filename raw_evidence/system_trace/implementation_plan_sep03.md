# SEP-03: RExSyn & NNSL Verification Plan

**Code Name**: `SEP-03-VERIFY`
**Objective**: Validate the "Liposomal Upadacitinib" hypothesis using the downstream engines (RExSyn & NNSL).
**Context**: "Force Injected" hypothesis from SEP-02 is the input.

## 1. Architecture

### A. RExSyn Bridge (`run_rexsyn_sep03.py`)
*   **Input**: `d:\Sanctum\렉스바이오 파이프라인\rexsyn_input_sep02.json`
*   **Engine**: `d:\Sanctum\RExSyn-Nexus-main\src\logos\rexsyn_service.py`
*   **Action**: Call `RExSynLOGOSService.validate_hypothesis()`.
*   **Goal**: Confirm that the hypothesis is logically consistent and structurally plausible (LOGOS "A" Grade or higher).
*   **Output**: `sep03_rexsyn_output.json`.

### B. NNSL Bridge (`run_nnsl_simulation.py`)
*   **Input**: `sep03_rexsyn_output.json` (Validated Hypothesis Text).
*   **Engine**: `d:\Sanctum\Flamehaven-Labs\Flamehaven-NNSL-Engine\engine\run_experiment.py`
*   **Action**: Inject the hypothesis text into the **Resonance Engine**.
*   **Simulated Metric**:
    *   **SR9 (Resonance)** $\approx$ Pharmacokinetic Stability.
    *   **DI2 (Drift)** $\approx$ Side Effect Potential (Acne Risk).
*   **Output**: `sep03_nnsl_output.json`.

## 2. Orchestration (`run_sep03_orchestrator.py`)
*   **Step 1**: Execute RExSyn Bridge.
*   **Step 2**: If RExSyn Pass, Execute NNSL Bridge.
*   **Step 3**: Generate Final Report (`sep03_final_report.md`).

## 3. Verification criteria
1.  **RExSyn**: `validation_status` == "validated".
2.  **NNSL**: `sr9_resonance` > 0.80 (High Stability = Good Drug Candidate).

## 4. Environment Notes
*   Must set `PYTHONPATH` correctly for both `RExSyn-Nexus-main` and `Flamehaven-NNSL-Engine`.
*   NNSL config path must be explicitly provided.
