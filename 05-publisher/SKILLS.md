---
name: publisher-final-compiler
description: Compile skills for the PUBLISHER bay — map validated fields into deploy-ready markdown, resolve conditional items, write to the listing directory and verify the read-back. Use when the PUBLISHER agent compiles a validated crate into a saved product listing.
bay: 05-publisher
stage_id: publisher
stage_next: null
allowed-tools: compile.listing, condition.resolve, disk.write, readback.verify
---

# PUBLISHER AGENT — SKILLS

**Skill name:** Publisher Final Compiler

## Skills

| Skill | Signature | Purpose |
| --- | --- | --- |
| `compile.listing` | `(crate) -> markdown` | Field mapping only; no copy generation. |
| `condition.resolve` | `(validator_conditions, crate) -> resolved \| returned` | Clears each CONDITIONAL item or returns the crate upstream. |
| `disk.write` | `(path, content) -> written` | Scoped to `PUBLISH_ROOT`; refuses any path outside it. |
| `readback.verify` | `(path) -> ok` | Asserts the file exists and is non-empty after write. |

## Guard condition

> PUBLISHER may not write a listing whose title or description failed validation; formatting is permitted, rewriting is not.

## Output contract

Writes `listing.md` and a `publish_package.json` sidecar to `PUBLISH_ROOT/<RUN_ID>/`; terminal stage, relays completion to COMMS.
