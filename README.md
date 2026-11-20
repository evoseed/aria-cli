# ARIA CLI

The command-line client for ARIA, evoseed's AI assistant for MikroTik network operations: chat with ARIA, run diagnostics and review changes from your terminal.

This repository hosts the prebuilt releases only: binaries, checksums and release notes. The source code is not public.

## Install

Install from **your ARIA instance**, so that you get the CLI version that matches it. Open the instance's sign-in page and go to **Other ways to sign in**: it shows the command for your system, which looks like this.

macOS and Linux:

```sh
curl -fsSL https://<your-aria-instance>/install.sh | sh
```

Windows (PowerShell):

```powershell
irm https://<your-aria-instance>/install.ps1 | iex
```

The installer downloads your instance's release from this repository, verifies its SHA-256 checksum and installs it in your home folder, with no administrator rights: `~/.local/bin/aria` on macOS and Linux, `%LOCALAPPDATA%\Programs\aria\bin\aria.exe` on Windows. If any step fails, nothing is installed and the CLI you already have stays as it is.

Then sign in:

```sh
aria login
```

## Supported systems

| System | File |
|---|---|
| macOS, Apple Silicon | `aria-darwin-arm64.gz` |
| macOS, Intel | `aria-darwin-x64.gz` |
| Linux x64 (glibc) | `aria-linux-x64.gz` |
| Linux arm64 (glibc) | `aria-linux-arm64.gz` |
| Linux x64 (musl, e.g. Alpine) | `aria-linux-x64-musl.gz` |
| Windows x64 | `aria-windows-x64.zip` |

On Alpine the CLI needs `libstdc++` and `libgcc`: `apk add libstdc++ libgcc`.

Every release also carries `SHA256SUMS`.

## Versions and updates

Each release has a version number (`x.y.z`, for example `1.0.0`) and an id (`cli-<id>`), where `<id>` identifies the exact code of the CLI. The number is the one you read and quote; the id is the release's identity (its tag and the names of its files). A new release appears only when the code changes, and every ARIA instance points to the release that matches its own version. Releases marked **Pre-release** belong to test instances.

- `aria --version` prints the version you have, as `aria <number> (<id>)`, for example `aria 1.0.0 (cli-0123456789ab)`.
- When it differs from your instance's, the CLI prints a one-line notice with the number and id of both, and carries on.
- `aria update` installs your instance's version, with the same checks as the installer: the new program must print exactly the line your instance expects, number included, before it replaces the one you have.

On Windows, if `aria update` is interrupted at the wrong moment and the `aria` command goes missing, run the install command again: it restores it.

## More than one instance

The first installation sets the instance the CLI talks to by default. Installing again from another instance does not change it.

- `aria instance` shows the instance in use and where that setting comes from.
- `aria instance https://<another-instance>` makes another instance the default.
- `--api-url https://<another-instance>` uses another instance for a single command.

## Manual download

You can also download the file for your system from the [Releases](../../releases) page. Compare its checksum with the matching line of `SHA256SUMS`:

```sh
sha256sum aria-linux-x64.gz            # Linux
shasum -a 256 aria-darwin-arm64.gz     # macOS
```

```powershell
Get-FileHash aria-windows-x64.zip -Algorithm SHA256   # Windows
```

Then unpack it (`gunzip` and `chmod +x` on macOS and Linux, extract the zip on Windows).

The binaries are not signed yet, so a file downloaded with a browser needs one more step; the installer does not.

- macOS: clear the quarantine with `xattr -d com.apple.quarantine <file>`.
- Windows: if SmartScreen blocks the first run, choose **More info**, then **Run anyway**.

A CLI installed by hand does not update itself: `aria update` prints the install command instead.

---

© evoseed. All rights reserved.
