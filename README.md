# gentoo-wm-configs

Clean default configs for a handful of Wayland compositors (and one X11 WM),
all paired with [Noctalia](https://github.com/noctalia-dev/noctalia-shell) as
the shell/bar, and a login-shell template that launches whichever one you
picked.

## Layout

```
gentoo-wm-configs/
├── niri/config.kdl          # scrollable-tiling compositor
├── hyprland/hyprland.conf   # animated tiling compositor
├── sway/config               # i3-compatible tiling compositor
├── scroll/config              # Sway fork, PaperWM-style single scrolling layout
├── labwc/rc.xml                # stacking WM, Openbox-style config
├── mangowc/config.conf          # tiling compositor
├── triad/config.kdl              # layout manager running inside River (Nim)
├── dwl/config.h                    # suckless-style compositor — config.h, not a runtime file
└── bash_profile.tmpl                  # login-shell template, execs the chosen session on tty1
```

Every preset starts `xwayland-satellite` (X11 app support) and `noctalia`
(shell/bar) on its own — except `dwl`, which has no exec-on-startup
mechanism of its own and instead launches Noctalia via dwl's `-s`
startup-command flag from `bash_profile.tmpl`/`LAUNCH_CMD` (see the comment
at the top of `dwl/config.h`).

`noctalia/config.toml` is the shared shell config used by every WM/compositor
here — same file regardless of which one is running under it.

## Install

Pick one, e.g. niri:

```bash
mkdir -p ~/.config/niri
cp niri/config.kdl ~/.config/niri/config.kdl
mkdir -p ~/.config/noctalia
cp noctalia/config.toml ~/.config/noctalia/config.toml
```

For `dwl`, copy `dwl/config.h` over dwl's own `config.def.h` as `config.h`
before `make clean install` — it's compiled in, not a runtime config file.

### `bash_profile.tmpl`

Template for `~/.bash_profile`: on tty1 login it execs `{{LAUNCH_CMD}}`
(replace with the actual launch command for whichever session you picked,
e.g. `niri-session` or `dwl -s noctalia`) instead of dropping to a shell.

```bash
sed 's/{{LAUNCH_CMD}}/niri-session/' bash_profile.tmpl > ~/.bash_profile
```

## Notes

- Each compositor's own wiki/docs are linked at the top of its config file.
- These are deliberately *clean defaults* — a starting point for a fresh
  install, not a fully personalized dotfiles dump.
