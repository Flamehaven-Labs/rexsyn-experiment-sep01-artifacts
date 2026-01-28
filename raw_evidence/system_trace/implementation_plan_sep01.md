# SEP-01: 1st Official Experiment Implementation Plan

**Code Name**: `SEP-01 (Sovereign Experiment Protocol 01)`
**Objective**: Experimentally derive high-value medical business hypotheses using deep full-text data, with strict exclusion of slop and hallucination.

## 1. Architecture Flow
`BioMedical Harvester` (Done) $\to$ **`Data Transformer` (New)** $\to$ `LOGOS Framework` (Configured) $\to$ `RExSyn Feasibility`

## 2. Component Implementation

### A. Data Transformer (`transform_harvest_to_logos.py`)
*   **Input**: `d:\Sanctum\BioMedical-Paper-Harvester\results\deep_harvest_results.json`
*   **Output**: `d:\Sanctum\렉스바이오 파이프라인\sep01_evidence_bundle.json`
*   **Logic**:
    *   Iterate through 10 full-text papers.
    *   Split `full_text` dictionary (Intro, Methods, Results, Discussion) into distinct **Evidence Spans**.
    *   **Anti-Slop**: Discard spans < 50 characters or containing generic boilerplate.
    *   **Meta-Tagging**: Tag each span with `source_title` and `section_type`.

### B. LOGOS Configuration (Strict Mode)
Modify `run_logos_pipeline.py` (or create subclass `run_sep01_logos.py`) with:
*   `logos_min_omega`: **0.70** (Up from 0.55) - High confidence required.
*   `strict_evidence`: **True** - Hypotheses must be grounded in specific spans.
*   `grounding_status_ratios`: Increase "clean" requirement to 0.85.

### C. Master Orchestrator (`run_sep01.py`)
*   Step 1: Execute Transformer.
*   Step 2: Execute LOGOS with Strict Config.
*   Step 3: Generate "Missing Link Report" filtering for **Decision: PASS** only.

## 3. Anti-Hallucination & Slop Protocol
*   **Pre-Processing**: Filter out "Introduction" (too general) if "Results" exist. Focus on "Methods" and "Results" for FACTS.
*   **Post-Processing**: Any hypothesis with `grounding_overlap < 0.3` is discarded automatically.

## 4. Verification Plan
1.  **Data Check**: Inspect `sep01_evidence_bundle.json` to ensure "Results" sections are correctly segmented.
2.  **Execution**: Run `python run_sep01.py`.
3.  **Audit**: Check `logos_execution_report.md`.
    *   pass condition: At least 1 Hypothesis with Grade 'A' or 'S'.
    *   fail condition: All hypotheses < 0.70 Omega (This is an acceptable experimental outcome).

## 5. RExSyn Integration (Handover)
*   The output of SEP-01 will be a `candidate_molecule_concept` or `therapeutic_mechanism`.
*   This will be formatted as a JSON query for `RExSyn` to perform structural validation in the next phase.
