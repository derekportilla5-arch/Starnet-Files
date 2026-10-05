---
name: validation-risk-auditor
description: Audit skills for the VALIDATION bay — grammar and semantic checks plus ROI math against the station spreadsheet variables, graded PASS, CONDITIONAL or FAIL. Use when the VALIDATION agent audits a designed crate and computes target margin before release to publishing.
bay: 04-validation
stage_id: validation
stage_next: publisher
allowed-tools: grammar.check, semantics.audit, roi.compute, roi.constants, risk.grade
---

# VALIDATION AGENT — SKILLS

**Skill name:** Validation Risk Auditor

## Skills

| Skill | Signature | Purpose |
| --- | --- | --- |
| `grammar.check` | `(text) -> issues[]` | Isolates syntax and grammar defects. |
| `semantics.audit` | `(copy, source_features) -> issues[]` | Flags contradictions and unverifiable claims. |
| `roi.compute` | `(base_price, fees, costs) -> {net_margin, target_roi, breakeven_price}` | Calculates the target return on investment. |
| `roi.constants` | `() -> fee_rates` | Reads `ROI_TABLE`; standard Etsy listing + transaction + payment rates, overridable. |
| `risk.grade` | `(issues, roi_result) -> PASS \| CONDITIONAL \| FAIL` | Derives the final audit grade. |

## Guard condition

> Any claimed ROI must cite the table row it came from; unbacked figures are not emitted.

## Output contract

Emits `<RUN_ID>-validated.json` to `CONVEYOR_OUTBOX` with a stamped `validator_review` block on PASS or CONDITIONAL; FAIL returns the crate to DESIGNER.
