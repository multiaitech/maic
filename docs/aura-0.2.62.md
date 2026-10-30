# Aura 0.2.62

Aura is the desktop app: Autonomous Unified Reasoning Agent. This note records what changed in 0.2.62, how the build was produced and checked, and what is still open.

## What changed for the user

- Coder model slots are independent. Frontend, Backend and Coder each keep their own model. Switching between split mode (Frontend plus Backend) and unified mode (Coder) no longer overwrites a model that has already been set.
- Checkpoints in the sidebar. Aura keeps the 3 most recent configuration snapshots plus up to 3 pinned safety snapshots taken before an app update, before Reset Factory and before a rollback. Each snapshot holds settings.json and aura-workspace.json, so modules, pipelines, teams, MCP servers and role/model bindings are all inside it. Every row shows what it contains, and a button takes a snapshot manually.
- Settings > Lesson. The button now reads "Delete uploaded LL data" and deletes only Lesson Learned records the server has already accepted. Queued records and server-refused records stay on the PC.

Carried over from 0.2.61: the Telegram Aura Connector sits under User MCP, the sidebar footer shows the version once, and stopping a screen recording shows a "Recorded Video" popup with the full path plus Open Folder and Play Video.

## Version bump

0.2.61 to 0.2.62 in six places: package.json:4, package-lock.json:3 and :9, src-tauri/tauri.conf.json:4, src-tauri/Cargo.toml:3, src-tauri/Cargo.lock:69.

## Build

- Frontend bundle: dist/index.html = 1,172 bytes, exit 0.
- Rust release build: "Finished `release` profile [optimized] target(s) in 13m 46s", aic-desktop.exe = 46,644,224 bytes, no Rust warnings.
- NSIS installer: Aura_0.2.62_x64-setup.exe = 60,296,415 bytes, exit 0. Only NSIS is built, because tauri.conf.json sets "targets": ["nsis"], so no MSI is attempted.
- The installer is large because ffmpeg (169 MB) ships inside it together with its licence.

SHA-256 of the installer: be005ed9f5449fcf5b1ba8053537dc84531921ebe757798c3d4e620c8c5bbb46

## Publishing and the update manifest

- The published installer and the version-locked copy are byte-identical, and both match the hash above.
- The update manifest (aura-update-manifest.json) and the version file are byte-identical: 750 bytes, SHA-256 1ad4cd007585af775fbc4e97bb9c82a823c61d8f373e9b7ac8361fcc17bed971.
- The manifest url field is version-qualified (.../Aura-Windows-x64-Setup-0.2.62.exe), never a generic "latest" name.
- GET https://multiaitech.com/api/v1/aura/version answers with version 0.2.62, the version-qualified URL and the SHA-256 above, so the in-app updater offers this release.

## How to verify

1. Open the version endpoint and read version, url and sha256.
2. Hash the installer you downloaded. docs/verify-a-build.md lists the command for each platform.
3. Compare the whole 64-character string. If it differs, do not run the file.

## What this release does not do

- The installer has to be applied by the owner. Nothing installs itself; press Update in Aura.
- Pinned safety snapshots are capped at 3. After a fourth safety event (for example two updates, a Reset Factory and a rollback) the oldest one is evicted.
- Checkpoints created by 0.2.61 and earlier are unpinned, so they still rotate away until a new safety snapshot exists.
- Modules that still carry a dangling mcp_ids entry for Telegram were left untouched on purpose: that is user data.
