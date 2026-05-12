# Robotic Process Automation — Process Mining

Insurance claims process mining using Python and PM4Py to discover workflow patterns, detect anomalies, and visualize the end-to-end claim lifecycle with Directly-Follows Graphs and Heuristic Miner process maps.

**Notebook:** [Open in Google Colab](https://colab.research.google.com/drive/12O8Ul5oJ-Mvt2IXuJ-S4k2mB4DllT-SC?usp=sharing)

---

## Background

This project analyzes an insurance claims event log containing **22,625 unique claims** and **131,233 total events** across **6 distinct activities**. The goal is to apply process mining techniques to understand how claims flow through the system, identify deviations from the expected process, and surface opportunities for automation and control improvements.

The claims process follows this general lifecycle:

> First Notification of Loss (FNOL) → Assign Claim → Set Reserve → Decide Claim → Payment Sent → Close Claim

---

## Dataset

| Field | Description |
|---|---|
| `case_id` | Unique identifier for each insurance claim |
| `activity_name` | Process activity (e.g., FNOL, Assign Claim, Decide Claim) |
| `timestamp` | Date and time the activity occurred |
| `claim_amount` | Dollar value of the claim |
| `car_make` | Vehicle manufacturer |
| `car_model` | Vehicle model |
| `car_year` | Vehicle year |
| `type_of_accident` | Category of accident (e.g., Head-on, Rear-end) |

---

## Analysis

### Key Metrics

| Question | Result |
|---|---|
| Unique insurance claims | 22,625 |
| Total claim events | 131,233 |
| Unique activities | 6 |
| Claims with payment sent | 18,108 |
| Payment Sent → Decide Claim flows (anomaly) | 107 |
| Process always ends with Close Claim | False |
| Most common path: Set Reserve before Decide Claim | True |

### Key Findings

- **Standardized early process:** Every claim runs through the same FNOL → Assign → Set Reserve → Decide sequence, indicating a highly structured intake workflow — but also raises the question of whether low-risk claims need all steps.
- **Payment before decision (control gap):** 107 cases show payment being issued before a formal Decide Claim activity is recorded, signaling a governance and auditability risk.
- **Not all claims close cleanly:** A small number of cases end at Payment Sent without reaching Close Claim, pointing to potential data quality issues or incomplete process execution.
- **18,108 of 22,625 claims (80%) resulted in payment;** the remaining ~20% were rejected or closed without payment.
- **Automation opportunity:** Given how linear and repeatable this process is, straight-through processing for straightforward, low-risk claims is a natural next step.

---

## Process Maps

The full interactive process maps are rendered in the Colab notebook linked above.

### Directly-Follows Graph (DFG)

Shows the frequency of transitions between activities across all 22,625 cases.

![DFG Process Map](images/download_(4).png)

### Heuristic Miner (HM)

Shows the most common ("happy path") flow through the process, filtering out noise.

![Heuristic Miner Process Map](images/download_(5).png)

---

## Tools & Libraries

| Tool | Purpose |
|---|---|
| Python | Core analysis language |
| pandas | Data loading and exploration |
| PM4Py | Process mining — event log conversion, DFG, Heuristic Miner |
| Google Colab | Interactive analysis environment |
| matplotlib | Supporting visualizations |
