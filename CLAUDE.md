# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal dotfiles repo, intended as the single source of truth (SSOT) for shell
and editor config. There is no build, test, or lint step — files here are meant
to be symlinked into `$HOME` and consumed by the shell / VS Code.

## Layout & how it's consumed

- `.zshrc` — **macOS** shell (oh-my-zsh `robbyrussell`, plugins=(git)). Loads
  zoxide, fzf, nvm, Docker completions, Tailscale on PATH, zsh-autosuggestions.
- `.bashrc` — **Ubuntu / homelab** shell (Debian default template + starship,
  zoxide, cargo/rust env). Assumes `eza`, `batcat`, `~/.cargo`.
- `setup.sh` — symlinks `~/.zshrc` and `~/.bashrc` back to this repo (`ln -sf`).
  This is the mechanism that makes the repo the SSOT. It does **not** touch VS
  Code config.
- `.vscode/` — **project-local** config for a Rust `parser` binary, not global
  editor settings. `launch.json` runs `cargo build --bin=parser` under lldb;
  `settings.json` here uses the oxc formatter + clippy. This directory is a
  leftover/example scope, not Joel's global editor prefs.
- `Library/Application Support/Code/User/{settings.json,keybindings.json}` —
  the **actual global VS Code** settings, tracked here but **not** symlinked by
  `setup.sh`. Editing these files in the repo does nothing until they are copied
  or symlinked to the real `~/Library/Application Support/Code/User/` location.

## Key things to know when editing

- Two shells diverge by platform: put macOS-only changes in `.zshrc`, Ubuntu/
  server changes in `.bashrc`. Shared aliases (`l`/`l2`/`l3` = `eza` trees,
  `c=clear`) are duplicated in both — update both intentionally.
- Because `~/.zshrc`/`~/.bashrc` are symlinks into this repo, edits take effect
  on the next shell reload; no copy step needed for the shell files.
- The repo `.vscode/settings.json` and the `Library/.../User/settings.json`
  overlap in intent but target different scopes and different formatters
  (repo = oxc/rust; User = prettier/ruff/rust). Don't assume one mirrors the
  other.

## Note on the parent CLAUDE.md

`~/repos/CLAUDE.md` (one level up) describes the whole `~/repos/` workspace and
its instruction to read `_REPO_ADMIN/` applies there, not to this standalone
dotfiles repo.
