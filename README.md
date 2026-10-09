# DIT — Done in Git

Local-first project management kept as Markdown in git.

This repository publishes DIT's releases: the macOS app, the command-line
binaries and the installer. It holds no source code.

## Install

**macOS app (menu bar), with Homebrew:**

```bash
brew install --cask faridlab/tap/dit
```

This installs `DIT.app` and puts `dit` on your `PATH`.

**Command line, macOS or Linux:**

```bash
curl -fsSL https://github.com/faridlab/dit/releases/latest/download/install.sh | bash
```

The installer downloads the release binary for your platform and checks its
SHA-256 digest.

**By hand:** download the archive for your platform from the
[latest release](https://github.com/faridlab/dit/releases/latest), check it
against its `.sha256` file, and put `dit` on your `PATH`.

| Platform | Archive |
|---|---|
| macOS, Apple silicon | `dit-aarch64-apple-darwin.tar.gz` |
| macOS, Intel | `dit-x86_64-apple-darwin.tar.gz` |
| Linux, x86_64 | `dit-x86_64-unknown-linux-gnu.tar.gz` |
| Linux, arm64 | `dit-aarch64-unknown-linux-gnu.tar.gz` |
| macOS app | `DIT-macos.zip` |

## Update

```bash
dit upgrade          # the command line installed by the installer or by hand
brew upgrade --cask dit
```
