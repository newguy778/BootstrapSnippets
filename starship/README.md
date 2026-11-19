# starship.toml — lean, single-line prompt

A single-line [Starship](https://starship.rs) prompt with **no segment separators,
no backgrounds, no powerline**, and a **256-colour palette** so every colour is a
fixed ANSI index instead of a hex guess. It is a port of a Powerlevel10k *lean*
setup, which is why every colour below maps 1:1 to a p10k variable.

```
~/code/project  main !1 ?1  ❯
```

## What it renders

| side | module | notes |
|---|---|---|
| left | `directory` | full path, bold, `~` for `$HOME`, lock glyph when read-only |
| left | `git_branch` | branch name, truncated at 32 chars with `…` |
| left | `git_status` | `~` conflicted, `*` stashed, `+` staged, `!` modified, `?` untracked, `⇡`/`⇣` ahead/behind — each in its own colour |
| left | `character` | `❯` green on success / red on failure, `❮` in vicmd, `V` in visual |
| right | `status` | `✘` + exit code, only when the last command failed |
| right | `cmd_duration` | shown after 3 s, no milliseconds |
| right | `jobs` | background job count, only when > 0 |
| right | `kubernetes` | **disabled by default** (see below) |

## Colours

Every value in the `p10k` palette is the raw 256-colour index the original
Powerlevel10k config used, so the look is reproducible on any terminal with a
256-colour palette:

| meaning | index | source |
|---|---|---|
| directory / path | `31` | `POWERLEVEL9K_DIR_FOREGROUND` |
| path anchor (bold) | `39` | `POWERLEVEL9K_DIR_ANCHOR_FOREGROUND` |
| shortened path | `103` | `POWERLEVEL9K_DIR_SHORTENED_FOREGROUND` |
| clean git / branch | `76` | `POWERLEVEL9K_VCS_CLEAN_FOREGROUND` |
| modified git | `178` | `POWERLEVEL9K_VCS_MODIFIED_FOREGROUND` |
| untracked git | `39` | `POWERLEVEL9K_VCS_UNTRACKED_FOREGROUND` |
| conflicted git | `196` | VCS conflicted state |
| error status | `160` | `POWERLEVEL9K_STATUS_ERROR_FOREGROUND` |
| command duration | `101` | `POWERLEVEL9K_COMMAND_EXECUTION_TIME_FOREGROUND` |
| background jobs | `70` | `POWERLEVEL9K_BACKGROUND_JOBS_FOREGROUND` |
| kubernetes | `134` | `POWERLEVEL9K_KUBECONTEXT_DEFAULT_FOREGROUND` |

## Two variants

| file | what |
|---|---|
| `starship.toml` | the lean baseline (this README describes it) |
| `starship-full.toml` | same look plus `$git_commit` (detached HEAD hash), `$git_state` (rebase/merge/bisect with step counts), `renamed`/`deleted` in the git status, directory truncation to the last 4 components with a `…/` prefix, and a `kubernetes` segment enabled but gated on `Chart.yaml` / `kustomization.yaml` / `skaffold.yaml` / `k8s/` / `kubernetes/` / `charts/` |

## Install

```sh
# 1. copy it into place
mkdir -p ~/.config
curl -fsSL https://raw.githubusercontent.com/newguy778/BootstrapSnippets/v1/starship/starship.toml \
  -o ~/.config/starship.toml

# 2. hook starship into your shell
echo 'eval "$(starship init zsh)"' >> ~/.zshrc      # or ~/.bashrc
exec zsh
```

Keep the file somewhere else and point `STARSHIP_CONFIG` at it instead:

```sh
export STARSHIP_CONFIG="$HOME/.config/zsh/starship.toml"
```

## Requirements

- **starship 1.20+** (tested on 1.27.0) — uses `palettes`, `right_format` and the
  `read_only` directory variable.
- A **Nerd Font** for two glyphs: the git branch icon `U+F126` and the read-only
  lock `U+F023`. Tested with MesloLGS Nerd Font Mono; any Nerd Font works.
- A 256-colour terminal.

## Things worth knowing

- `add_newline = false` — the prompt does not push a blank line between commands.
  Add `add_newline = true` if you prefer the breathing room.
- The right prompt is intentionally almost always **empty** — it only appears on
  failure, on a slow command, or with background jobs.
- **`kubernetes` is disabled.** Starship cannot reproduce p10k's "show only while
  typing kubectl" behaviour, so an always-on segment prints the context in every
  directory. To enable it, set `disabled = false` and shorten the context with
  `context_aliases = { "my-long-context" = "ctx" }`. Its symbol is `⎈` (U+2388)
  rather than p10k's `U+F10FE`, which is missing from some Nerd Font builds.
- **`os` is disabled.** The Apple logo was removed deliberately; `[os.symbols]`
  is kept so re-enabling is a two-line change (add `$os` to `format`).
- The `directory` module styles the whole path in one colour. p10k drew each path
  component bold and the `/` separators dimmer; starship has no per-component
  style, so `style = "bold anchor"` is the closest equivalent. Use
  `style = "dir"` for the teal-only variant.
- To append language/runtime segments, add them to `right_format`, e.g.
  `right_format = "$status$cmd_duration$jobs$nodejs$python"`. Most are silent
  unless you are inside a matching project.

## No personal data

This file contains no paths, usernames, hostnames, cluster names, profile names or
credentials — only module configuration and colour indexes. The `kubernetes`
module is disabled and carries no context names, so it is safe to publish as is.
