---
name: scalper-raw-text-parser
description: Parsing skills for the SCALPER bay — extract title, base price, source listing URL and item features from raw packet content, optionally enriching blanks from the Etsy API. Use when the SCALPER agent receives a raw packet from COMMS and must emit structural variables.
bay: 01-scalper
stage_id: scalper
stage_next: filter
allowed-tools: text.parse_fields, etsy.enrich, price.normalize, features.dedupe
---

# SCALPER AGENT — SKILLS

**Skill name:** Scalper Raw-Text Parser

## Skills

| Skill | Signature | Purpose |
| --- | --- | --- |
| `text.parse_fields` | `(raw_content) -> {title, base_price, source_listing_url, item_features}` | Regex + heuristic extraction. |
| `etsy.enrich` | `(url) -> partial_fields` | GET the source listing; auth header `Authorization: [redacted-secret] ${ETSY_API_KEY}`. |
| `price.normalize` | `(str) -> float \| null` | Returns `null` if unparseable — never `0` by default. |
| `features.dedupe` | `(list) -> list` | Trims, drops empties, removes case-duplicate entries. |

## Guard condition

> Missing extraction produces explicit `null`, never an invented value.

## Output contract

Emits `<RUN_ID>-scalped.json` to `CONVEYOR_OUTBOX` with a stamped `scalper_analysis` block; no pass/fail judgement is made here.
