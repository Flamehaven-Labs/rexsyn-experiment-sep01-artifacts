# SEP-02: Scaled Experiment Implementation Plan

**Code Name**: `SEP-02-SCALE`
**Objective**: Re-run the experiment with 5x data volume (50 targets) to test if "Missing Links" emerge from a larger corpus.
**Philosophy**: "Quantity has a quality all its own" - Stalin (in data context).

## 1. Component Updates

### A. Harvester (`run_deep_harvest.py`)
*   **Change**: `max_results=20` $\to$ `max_results=50`.
*   **Target**: Expecting ~20 valid Full-Text PMC papers (given ~40% success rate).
*   **Queries**: Maintain current high-value queries (Pediatric AD, Asthma, Ichthyosis).

### B. Orchestrator (`run_sep01_experiment.py` -> `run_sep02_experiment.py`)
*   **Renaming**: Clone to `run_sep02_experiment.py` to preserve SEP-01 history.
*   **Config Adjustment**:
    *   `irf_mode`: **"conservative"** (Fixed from invalid "rigorous").
    *   `irf_global_min`: **0.60** (Relaxed from 0.70 to find soft signals).
    *   `strict_evidence`: **True** (Keep anti-hallucination active).

## 2. Execution Flow
1.  `python run_deep_harvest.py` (Approx 2-3 mins).
2.  `python run_sep02_experiment.py`.
3.  **Analyze Results**:
    *   If Hypotheses > 0: Determine "Winner" for RExSyn.
    *   If Hypotheses == 0: Proceed with the **"Upadacitinib Manual Injection"** (Plan B) as the final fallback.

## 3. Verification Plan
*   **Check 1**: `deep_harvest_results.json` size should be > 20 papers.
*   **Check 2**: `sep02_results.json` should exist.
*   **Check 3**: No `UnicodeEncodeError` in output logs.
