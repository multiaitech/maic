# Configuration checkpoints

## What a checkpoint holds

A checkpoint is a copy of two files: settings.json, which carries roles with their bound models and the MCP servers, and aura-workspace.json, which carries agents, pipelines, teams and modules together with their sub-services and executors. Modules, pipelines and settings a user built are therefore inside the snapshot.

## Retention

Aura keeps the 3 most recent ordinary snapshots, written after a module is added, removed or changed, plus up to 3 pinned safety snapshots. The store never grows without limit and no single action wipes the whole history.

Snapshots are ordered by (at_ms, id) descending, so two snapshots written in the same millisecond still have a stable order.

## The three safety points

- Before an app update, labelled "Before update v<current> -> v<target>"
- Before Reset Factory, labelled "Before Reset Factory <date time>"
- Before a rollback, labelled "Before rollback to <label> <time>"

These are fail-closed. If a safety snapshot cannot be written, the update, the reset or the rollback is cancelled instead of continuing without a backup.

A rollback also reads the target snapshot into memory first and writes the current state as a new snapshot before applying anything, so a rollback can itself be undone.

## Why checkpoints survive an update and a factory reset

- Reset Factory only rewrites settings.json and aura-workspace.json, clears the memory store and the secret vault. It never deletes the config directory, so checkpoints/ stays.
- The NSIS installer only replaces application files. AuraData/ is not touched.
- Production code never removes checkpoints/. The only remove_dir_all calls against the config directory sit inside test modules.

## Reading a snapshot before you restore it

Each row shows what the snapshot contains, for example:

    3 modules - 2 pipelines - 5 MCP - 4 models

Snapshots from 0.2.61 and earlier have no stored summary. They are counted on read, so an old row shows real numbers instead of zero. The model count includes only roles whose slot is actually filled.

A button in the sidebar takes a checkpoint manually.

## System Log events

- config_checkpoint.created (INFO), with real counts and a pinned marker for safety points
- config_checkpoint.pruned (INFO) when an ordinary snapshot is evicted
- config_checkpoint.pinned_pruned (WARN) when a safety snapshot is evicted
- config_checkpoint.prune_failed (WARN) when a file cannot be removed
- config_checkpoint.pre_update, pre_reset and pre_rollback (WARN) before each safety event
- config_checkpoint.rolled_back (WARN) after a restore
- config_checkpoint.pinned_deleted (WARN) when the delete button removes a pinned row

## Verification

- cargo test --lib config_checkpoint: 10 passed, 0 failed. This covers retention, summaries, pinning, rollback, factory reset, the pre-update path and prune logic.
- cargo test --lib core::update: 13 passed, 0 failed, including the pre-update safety checkpoint.
- cargo test --lib commands::settings::tests: 10 passed, 0 failed, including the pre-reset safety checkpoint.
- vitest full suite: 71 files, 522 tests passed.
- cargo check --manifest-path src-tauri/Cargo.toml: exit 0

## Limits worth knowing

- Safety snapshots are capped at 3. A fourth safety event evicts the oldest one, so an old restore point is not kept forever.
- Snapshots written before pinning existed count as unpinned and rotate away until a new safety snapshot is written.
- The backend event config-checkpoint-changed is not emitted. The panel refreshes itself by listing snapshots, so a change can be missed if the panel is never opened again.
- Deleting a pinned row from the UI has no confirmation dialog. It is recorded as config_checkpoint.pinned_deleted so it stays auditable.
