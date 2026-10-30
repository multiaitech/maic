# Coder model slots

## The problem

Setting a model for Frontend and Backend, then switching to Coder, overwrote those models. The reverse direction did the same: picking models in split mode overwrote the Coder model. A model that has been set should stay set.

## Root cause

The interface showed three coder slots, Frontend, Backend and Coder, but storage had only two keys: coder-frontend and coder-backend. The unified model therefore had no slot of its own and had to overwrite one of the halves.

- In unified mode, enabled_coder_ids() returned a single canonical runtime slot, coder-frontend.
- bindCoderUnified() wrote one unified choice into both halves through setAuraRoleExecution, syncRoleModel and activateRolesWithModel, which erased the separate models already set per half.
- A unifiedHealKey effect copied a model from one half to the other as soon as the Coder tab was opened.
- UNIFIED_CODER_ROLES held the two halves those helpers wrote to.

## What the fix does

There are now three genuinely independent slots.

- RoleKey::Coder maps to the key "coder" in src-tauri/src/core/settings.rs:63, so AppSettings.roles has a third key.
- enabled_coder_ids() returns only "coder" in unified mode and the enabled halves in split mode.
- UNIFIED_CODER_ROLE is 'coder', so the unified picker writes to that slot only.
- setUnifiedCoder(on) changes enable flags and writes no model. Switching mode changes which slot runs, never what a slot holds.
- unifiedHealKey and UNIFIED_CODER_ROLES are deleted.

This replaced the original plan of snapshotting and restoring models. With three independent slots nothing has to be remembered and replayed.

## The contract

Frontend equals A, Backend equals B, Coder equals C.

- Switching mode leaves A, B and C untouched.
- unified_slot_is_seeded_once_from_the_halves fills the unified slot once, and only when it is empty and the halves hold a model.
- unified_slot_never_seeds_the_split_halves asserts that the reverse never happens, so Frontend and Backend are never overwritten.
- serde(default) on AppSettings::roles lets a 0.2.61 settings.json load even when the coder key is missing or truncated.

## How to check it by hand

1. Open Settings > Coder.
2. Set Frontend to model A and Backend to model B.
3. Switch to unified Coder and pick model C.
4. Switch back to Frontend and Backend. A and B are still there.
5. Repeat in the other direction: C, then split, then unified again. C survives.

## Verification

- tsc --noEmit -p tsconfig.app.json: exit 0
- vitest full suite: 71 files, 522 tests passed
- vitest src/tests/coderSlotPersistence.test.ts: 4 tests passed
- cargo check --manifest-path src-tauri/Cargo.toml: exit 0
- cargo test --lib core::settings::tests: 33 passed, 0 failed
- cargo test --lib coder: 32 passed, 0 failed

## Still open

- The fix exists in source. A user on an older build sees it after updating.
- setUnifiedCoder(false) still force-enables the halves, flags only and no model, by design: a half with no model never becomes active staff.
- An earlier note named coder-mode-model-locking.md is superseded by this one.
