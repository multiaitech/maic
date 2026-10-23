# How to verify a build

Every build we publish is fingerprinted before it becomes downloadable. The fingerprint is
published first, so a file you already have can be checked against the file we say we shipped.
Nothing in this process requires an account.

## 1. Read the published fingerprint

    GET https://api.multiaitech.com/api/v1/downloads/apps

The endpoint is public and needs no key. It lists published builds only, and each entry carries
the product, its channel, its version and its SHA-256. A product with no published build does not
appear at all, and it never appears with an empty hash.

## 2. Hash the file you downloaded

Windows, PowerShell:

    Get-FileHash .\the-file-you-downloaded -Algorithm SHA256

Windows, Command Prompt:

    certutil -hashfile the-file-you-downloaded SHA256

macOS:

    shasum -a 256 the-file-you-downloaded

Linux:

    sha256sum the-file-you-downloaded

## 3. Compare the two strings

The hash is 64 hexadecimal characters, so compare the whole string, not the first few characters.
Case does not matter.

- **Identical** - the file is byte-for-byte the build we published.
- **Different** - do not run it. It changed in transit or it is not our file. Delete it, download
  again, and if it still differs, tell us before you run it.

## What this does and does not prove

It proves the file you are holding is the file we published. It does not prove the build is safe
or good; that is what a published status and a version history are for. It says nothing about the
blockchain, and nothing about the value of MAIC.

## Rules we hold ourselves to

- No hash is published for a build that is not published.
- When a new version is published, the old hash is replaced, not quietly kept alongside it.
- If a page and the endpoint ever disagree, the endpoint is correct.
