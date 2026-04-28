# AI Digital Timecard Evaluation

An automated decision-support system designed to review workforce timecards, apply policy guardrails, and assign standardized decision and reason codes. This project demonstrates a transition from basic rule-based logic to an optimized, validated LLM-inspired workflow.

## 🚀 Overview
This system automates the verification of employee timecards by checking against specific corporate policies. To ensure reliability and prevent model hallucinations or validation errors, the project utilizes:
* **Human-in-the-Loop (HITL) Design**: Modular architecture for manual oversight.
* **Strict Guardrailing**: Use of Pydantic and JSON schema validation.
* **A/B Prompt Testing**: Systematic comparison between initial and optimized logic.
* **Jupyter Framework**: Step-by-step execution to minimize processing risks.

## 🛠️ Technical Stack
* **Language**: Python 3.x
* **Data Handling**: `Pandas`, `JSON`
* **Validation**: `Pydantic` (BaseModel)
* **Environment**: Jupyter Notebook

## 📊 Performance Summary
We conducted A/B testing on 100 records to measure the impact of prompt optimization and additional data fields.

| Metric | Raw Prompt (v1) | New Prompt (v2) | Improvement |
| :--- | :--- | :--- | :--- |
| **JSON Coverage** | 100% | 100% | -- |
| **Decision Accuracy** | 69.0% | **88.0%** | **+19%** |
| **Reason Accuracy** | 66.0% | **77.0%** | **+11%** |
| **Joint Accuracy** | 66.0% | **77.0%** | **+11%** |

### Key Findings
* **Validation**: Achieved a **100% valid JSON rate**, ensuring the system never fails due to formatting errors.
* **Optimization**: Version 2 significantly improved accuracy by better handling complex scenarios like overtime justification and missed punches.

## 🏗️ Core Architecture

### Data Validation
The system uses a `ModelDecision` Pydantic class to enforce strict output schemas.
* **`normalized()`**: Standardizes outputs by trimming whitespace and converting to uppercase.
* **`validate_json_output()`**: Verifies that the model's response matches `ALLOWED_DECISIONS` and `ALLOWED_REASON_CODES`.

### Logic Iterations
1.  **`raw_model_response` (v1)**: Focused on basic flags (VMS, missing data, negative hours, and simple overtime).
2.  **`new_model_response` (v2)**: Enhanced logic incorporating:
    * Detailed overtime thresholds (>10 hours).
    * Mismatch detection between reported and approved hours.
    * Natural Language Processing (NLP) cues within notes (e.g., checking for "approved" or "missing").
    * Missed punch review logic.

## 🔍 Analysis of False Negatives
Through the evaluation script, we identified common edge cases that still require human review:
* **Ambiguity**: Multiple conditions triggering conflicting reason codes.
* **Context Missing**: Overtime worked without corresponding justification in the notes.
* **Data Quality**: Misspelled entries or inconsistent manual input from employees.

## 📂 Project Structure
* `timecard_approval.ipynb`: Main development and testing notebook.
* `timecards.csv`: Source dataset.
* `timecards_valid.csv`: Processed output with assigned decisions.
* `timecards_summary.csv`: Final comparative statistics.

---

### How to use
1. Clone the repository.
2. Install dependencies: `pip install pandas pydantic`.
3. Run the Jupyter Notebook to view the step-by-step validation and evaluation.
