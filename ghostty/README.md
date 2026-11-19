# ghostty — macOS terminal config

A small, opinionated [Ghostty](https://ghostty.org) config for macOS: Safari-style
tabs merged into the titlebar, Catppuccin Mocha, slight transparency + blur, tight
build-up of the window padding, and splits that inherit the working directory.

Tested on **Ghostty 1.4.0** (macOS, Apple silicon). `ghostty +validate-config`
reports no problems.

## What's in it

| setting | value | why |
|---|---|---|
| `macos-titlebar-style` | `tabs` | tabs replace the titlebar, Safari-style (no separate tab strip) |
| `macos-titlebar-proxy-icon` | `hidden` | removes the redundant proxy icon next to the title |
| `theme` | `Catppuccin Mocha` | shipped with Ghostty (`ghostty +list-themes`), no file needed |
| `window-colorspace` | `display-p3` | wide-gamut colour on P3 displays |
| `background-opacity` | `0.96` | barely-there transparency; text stays crisp |
| `background-blur` | `true` | blur behind the window |
| `window-padding-x/y` | `10` / `8` | breathing room around the grid |
| `window-padding-balance` | `true` | splits the leftover cells evenly instead of dumping them right/bottom |
| `window-padding-color` | `background` | padding is painted with the background colour, not a hard border |
| `window-save-state` | `always` | windows and tabs come back after a restart |
| `window-new-tab-position` | `current` | new tabs open next to the current one |
| `tab-inherit-working-directory` | `true` | a new tab starts in the same directory |
| `split-divider-color` | `#585b70` | Catppuccin surface1, so split borders match the theme |

The macOS 26+ native glass look is included as a comment — swap
`background-blur = true` for `background-blur = macos-glass-regular` and drop the
opacity to ~0.7.

## Two variants

| file | what |
|---|---|
| `config` | the baseline macOS config (this README describes it) |
| `config-updated` | newer take: macOS 26+ Liquid Glass instead of plain blur (`background-blur = macos-glass-regular`, `background-opacity = 0.7`), glass-matched Dock icon, padding `12/10`, `unfocused-split-opacity`, `font-thicken`, plus commented-out extras — Option-as-Alt, notify-on-command-finish, and a global quick-terminal hotkey |

Both are self-contained; pick one (keep it as `~/.config/ghostty/config`) or merge
the blocks you want. Note that `background-opacity` and quick-terminal settings
only take effect after a full quit and relaunch, not a config reload.

## Install

```sh
mkdir -p ~/.config/ghostty
curl -fsSL https://raw.githubusercontent.com/newguy778/BootstrapSnippets/v1/ghostty/config \
  -o ~/.config/ghostty/config
```

Then reload inside any Ghostty window with <kbd>cmd</kbd>+<kbd>shift</kbd>+<kbd>,</kbd>
or:

```sh
ghostty +reload-config
```

Check it before you keep it:

```sh
ghostty +validate-config --config-file=~/.config/ghostty/config
ghostty +show-config      # what Ghostty actually resolved
```

## Font

`font-family` (and `font-size`, `font-thicken`) are intentionally **left
commented out**, so Ghostty uses the system monospace with per-glyph fallback —
which is fine for plain text but makes Nerd Font glyphs (icons, Powerline
separators, prompt symbols) come from whichever fallback font happens to cover
them, so their size and shape vary.

Pin one instead if your prompt uses glyphs:

```
font-family = MesloLGS Nerd Font Mono
font-size = 14
font-thicken = true
```

`ghostty +list-fonts | grep -i nerd` shows what is installed.

## Requirements

- Ghostty **1.2+** for `window-padding-color` and `split-divider-color`; 1.4.0 is
  what this was validated against.
- macOS — `macos-titlebar-style`, `macos-titlebar-proxy-icon` and
  `window-colorspace` are macOS-only keys and are ignored elsewhere.
- No personal data in this file: no paths, hostnames, user names or credentials.
