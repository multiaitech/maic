# Lesson Learned: deleting local data

## The rule

The "Delete uploaded LL data" button in Settings > Lesson deletes a Lesson Learned record only when that record has been uploaded and accepted by the server. Every record that has not been uploaded stays on your PC.

A record is deleted only when it has:

- a row in the outbox with status SENT, and
- a non-empty server receipt

A record is kept when it is:

- still queued in pending.ll and never sent,
- in the outbox as PENDING and still being retried,
- refused by the server as REJECTED,
- stored but never entered the outbox,
- a .bak archive written before the deletion.

Before anything is written, Aura writes lessons.ll.<stamp>.pre-local-delete.bak. If nothing qualifies for deletion, nothing is written at all.

## Why the outbox row stays after the record is gone

The SENT outbox row remains as a marker that the server already holds that record. Because of it, requeue_missing_uploads has no body to offer again, and recover_deleted_uploads recognises the id as a duplicate instead of importing it a second time. The tests assert both.

## What the user sees

- Dialog title: "Delete uploaded Lesson Learned data from this PC?"
- Body: "Only Lesson Learned records already uploaded to the server are deleted from this PC; records still waiting to upload are kept."
- Button: "Delete uploaded"
- Result: "Deleted N uploaded records from this PC - M records not uploaded yet were kept."

## Refused records are not deleted automatically

A refused upload only changes state: the outbox row becomes REJECTED. The record, its payload, its credit row and its outbox row all remain on disk, because the server never stored the content and the local copy is the only one that exists.

The only automatic local deletion is a different case: when the server reports that a record it already holds is terminal and not counted, reconcile_lessons deletes that record locally with a ServerRejected tombstone. That is the server reviewing a record it already accepted, not an upload being refused.

A quarantined record can be sent again on purpose, either through the "Re-send quarantined lessons (N)" button or through the once-per-build automatic pass. If it is refused again it is marked REJECTED again, and it is still not deleted.

## Where this lives in the code

- delete_uploaded_lessons in src-tauri/src/core/lesson.rs:6274
- delete_lesson_data command in src-tauri/src/commands/lesson.rs:493
- System Log event lesson.local_delete, carrying deleted, kept_pending, kept_not_uploaded and kept_rejected
- Label constant DELETE_UPLOADED_LABEL in src/ui/dialogs/lessonCards.ts:214

## Verification

- cargo test --lib kept: 10 passed, 0 failed. This includes a test that a record the server accepted is deleted while its outbox row is kept, a test that a record with a pending upload is kept and pending.ll stays byte-identical, a test that a server-refused record is kept, and a test that a stored record which never entered the outbox is kept.
- cargo test --lib requeue_missing_uploads: 26 passed, 0 failed. This includes nothing_is_re_queued_or_re_imported_after_the_delete and a_timestamped_backup_of_the_store_is_written_before_the_delete.
- tsc --noEmit -p tsconfig.app.json: exit 0
- vitest run src/tests/lessonDeleteLocal.test.ts src/tests/lessonCards.test.ts: 2 files, 50 tests passed

## Still open

- LessonEngine::wipe is marked allow(dead_code) and has no caller, so there is no UI path for a full local wipe. Right-to-erasure is available only through the existing server-side withdrawal.
- Credit-ledger entries for deleted records are kept on purpose.
- The confirm dialog itself has no DOM render test; its copy is covered through pure helpers.
- A redistilled record with a new lesson_id is a new record on the server, because de-duplication is by client_submission_id only.
