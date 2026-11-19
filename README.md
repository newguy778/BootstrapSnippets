# BootstrapSnippets

Small, self-contained config snippets for bootstrapping a dev machine. Each
folder is a drop-in piece of configuration with a README explaining what it does
and how to install it — no dotfile framework, no plugin manager required.

| path | what |
|---|---|
| [`starship/`](starship/) | Lean single-line [Starship](https://starship.rs) prompt — 256-colour palette, no separators, p10k-inspired |

## Using a snippet

Each folder has its own README with an install command. In general: copy the file
into place, then source or restart your shell.

```sh
git clone https://github.com/newguy778/BootstrapSnippets
cp -r BootstrapSnippets/starship ~/.config/
```

## Notes

- Everything here is meant to be readable and forkable — read a snippet before you
  install it, especially if it touches your shell startup.
- Release artifacts (binaries) are published on the `v2` branch, not `v1`.
- No licenses are implied by this repository unless a snippet says otherwise.
