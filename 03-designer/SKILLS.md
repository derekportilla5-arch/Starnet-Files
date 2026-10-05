---
name: designer-rebranding-node
description: Rewriting skills for the DESIGNER bay — infer the target demographic, compose an emotional value hook and punchy benefit bullets, and hold price and URL immutable. Use when the DESIGNER agent turns verified technical parameters into high-converting copy.
bay: 03-designer
stage_id: designer
stage_next: validation
allowed-tools: demographics.infer, copy.hook, copy.punch, immutable.guard
---

# DESIGNER AGENT — SKILLS

**Skill name:** Designer Rebranding Node

## Skills

| Skill | Signature | Purpose |
| --- | --- | --- |
| `demographics.infer` | `(features, category) -> segment` | Derives the target audience from product signals. |
| `copy.hook` | `(title, features) -> {human_title, value_hook, benefit_bullets[]}` | Composes the emotional through-line. |
| `copy.punch` | `(bullet) -> string` | Verbs first, no filler adverbs. |
| `immutable.guard` | `(price, url) -> assertion` | Hard assertion that these fields are unchanged on write. |

## Guard condition

> Any bullet that asserts a fact not present in `item_features` must be dropped before write.

## Output contract

Emits `<RUN_ID>-designed.json` to `CONVEYOR_OUTBOX` with a stamped `designer_copy` block; `base_price` and `source_listing_url` pass through unchanged.
