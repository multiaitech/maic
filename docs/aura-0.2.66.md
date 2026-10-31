# Aura 0.2.66

Aura is the desktop app: Autonomous Unified Reasoning Agent. This note records what changed since 0.2.62, how the build was produced and checked, and what is still open.

## What changed for the user

**Screen recordings are saved while they record (0.2.63).** A recording is no longer held in memory until Stop. The file is created when recording starts and each one-second chunk is appended to it, so the length of the recording no longer decides whether the save succeeds. Stopping shows the same "Recorded Video" popup with the full path, Open Folder and Play Video, and it now appears only after the last chunk is on disk. If saving fails, no popup is shown, the partial file is removed, and the error appears in the Share panel.

**Leader is no longer offered as a harness (0.2.64).** Model Hub > Harness listed Leader as an ACP-ready harness even though Leader is a role, not a harness. The catalog no longer contains it, and an entry already stored in the workspace is filtered out when settings load. One check (is_role_id) now refuses every role id as a harness.

**Settings > Lesson > Delete uploaded LL data (0.2.64).** The confirmation panel shows three separated buttons: Download LL, Delete LL and Cancel. Restore backup moved to the Lesson panel row, next to the delete button. The line that counted the local uploaded records was removed.

**Consensus (0.2.65).** The running conclusion is pinned at the top. Each member answer is collapsed by default and can be expanded on its own, without opening the whole list first. The show/hide label now matches what is rendered while a run is still processing. Telegram runs the same one-by-one chain as the desktop app instead of a parallel fan-out.

**English-only interface copy (0.2.66).** The Long-term Memory panel showed Indonesian text, and so did the sidebar project delete and restore dialogs. They now read "Loading...", "No memory to display yet." and English dialog text, so the interface is consistent for an international user.

## Version bump

0.2.66 in six places: package.json:4, package-lock.json:3 and :9, src-tauri/tauri.conf.json:4, src-tauri/Cargo.toml:3, src-tauri/Cargo.lock:69.

## Build

Frontend bundle: dist/index.html = 1,172 bytes, main chunk 1,009.32 kB (gzip 295.86 kB), exit 0.
Rust release build: "Finished release profile [optimized] target(s) in 13m 32s", aic-desktop.exe = 47,094,784 bytes.
NSIS installer: Aura_0.2.66_x64-setup.exe = 60,381,150 bytes, exit 0. Only NSIS is built, because tauri.conf.json sets targets to nsis, so no MSI is attempted.
The installer is large because ffmpeg (169 MB) ships inside it together with its licence.

SHA-256 of the installer: 2eb33cd73f3963920ec8604b19ed7bc691c4c976afd788b6a9d56cefe1288cd4

## Publishing and the update manifest

The published installer and the version-locked copy are byte-identical, and both match the hash above.
The update manifest (aura-update-manifest.json) and the version file are byte-identical: 368 bytes, SHA-256 85e0879d92739e774ceda3de23d4454407504452512003d541f48ba3b7950a88.
The manifest url field is version-qualified (.../Aura-Windows-x64-Setup-0.2.66.exe), never a generic latest name.
GET https://multiaitech.com/api/v1/aura/version answers with version 0.2.66, the version-qualified URL and the SHA-256 above, so the in-app updater offers this release.

## How to verify

1. Open the version endpoint and read version, url and sha256.
2. Hash the installer you downloaded. docs/verify-a-build.md lists the command for each platform.
3. Compare the whole 64-character string. If it differs, do not run the file.

## What this release does not do

The installer has to be applied by the owner. Nothing installs itself; press Update in Aura.
The installer is not code-signed, the same as every earlier release.
The consensus chain gives every member the original question. The running conclusion is folded from each answer, but an earlier conclusion is not fed back into the next member prompt; that is a design decision, not a defect.
Delete LL stays disabled until Download LL finishes, so a backup exists before anything is deleted.
There is no UI test for consensus rendering on Telegram; only the Rust chain is covered.
