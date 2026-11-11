# klarix

The Klarix command-line tool. It records which lines AI coding tools (Claude Code, Cursor) write and reports, per commit, how many of them were committed. No code, prompts or line contents leave your machine: only commit SHAs, file paths and line counts.

## Install

macOS or Linux, with Homebrew:

```sh
brew install kodaxatech/tap/klarix
```

Or with the install script, which checks the download against the release's checksums (and their signature, when `cosign` is installed) before installing:

```sh
curl -fsSL https://github.com/kodaxatech/klarix-cli/releases/latest/download/install.sh | sh
```

It installs to `/usr/local/bin`, or `~/.local/bin` when that isn't writable. Set `KLARIX_INSTALL_DIR` to choose the folder, or `KLARIX_VERSION` (for example `v0.1.0`) to pin a release.

Windows: download `klarix_windows_amd64.zip` or `klarix_windows_arm64.zip` from [Releases](https://github.com/kodaxatech/klarix-cli/releases) and put `klarix.exe` on your `PATH`.

Check the installed version with `klarix version`.

## Set up

```sh
klarix login   # connect this laptop to your Klarix workspaces in the browser
klarix init    # in each repository: add the hooks and choose its workspace
klarix status  # see what is installed, connected and queued
```

`klarix init --user` puts the Claude Code and Cursor hooks in your user settings instead of the repository, so they cover every repository on this machine.

## Verify a release

Each release's `checksums.txt` is signed by the Klarix release workflow with [Sigstore](https://www.sigstore.dev/). With [cosign](https://docs.sigstore.dev/cosign/system_config/installation/) installed, download `checksums.txt`, `checksums.txt.sigstore.json` and the archive, then:

```sh
cosign verify-blob \
  --certificate-identity-regexp '^https://github.com/kodaxatech/software-analytics/.github/workflows/release-agent.yml@refs/tags/agent-v' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  --bundle checksums.txt.sigstore.json checksums.txt
sha256sum --check --ignore-missing checksums.txt   # or: shasum -a 256 -c --ignore-missing checksums.txt
```

---

This repository holds releases only; the source isn't public.
