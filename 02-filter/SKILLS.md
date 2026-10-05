---
name: filter-gatekeeper
description: Boundary-check skills for the FILTER bay — validate required keys, price range, URL form and feature presence, and emit a reject record for any failing run. Use when the FILTER agent must pass or kill a scalped crate before it reaches the design desk.
bay: 02-filter
stage_id: filter
stage_next: designer
allowed-tools: keys.required_check, price.boundary_check, url.wellformed_check, features.nonempty_check, reject.emit
---

# FILTER AGENT — SKILLS

**Skill name:** Filter Gatekeeper

## Skills

| Skill | Signature | Purpose |
| --- | --- | --- |
| `keys.required_check` | `(obj, [title, base_price, source_listing_url, item_features]) -> PASS \| FAIL` | Fails on any blank required key. |
| `price.boundary_check` | `(price, FLOOR, CEILING) -> PASS \| FAIL` | Fails on `null`, non-numeric, or out-of-range. |
| `url.wellformed_check` | `(url) -> PASS \| FAIL` | Rejects malformed source links. |
| `features.nonempty_check` | `(list) -> PASS \| FAIL` | Rejects an empty feature set. |
| `reject.emit` | `(run_id, failed_test, reason) -> terminal` | Terminates the run and logs it. |

## Guard condition

> FILTER may not repair a failing field; repair-eligible data is rejected, not patched.

## Output contract

Emits `<RUN_ID>-filtered.json` to `CONVEYOR_OUTBOX` on PASS; emits a `reject` record to `LOG_DIR` and notifies COMMS on FAIL.
