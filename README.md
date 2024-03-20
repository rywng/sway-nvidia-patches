# Sway patches for Gentoo Linux with Nvidia cards

This patch set does three things on two software:

- SwayWM
  - Remove the "Unsupported GPU" warning entirely
  - Remove the need to specify `--unsupported-gpu` when running `sway`
- wlroots
  - Nvidia driver patch, which allows you to use GL renderer with Nvidia cards with slight performance cost, converted from [wlroots-nvidia](https://aur.archlinux.org/packages/wlroots-nvidia)

You can omit the `sway` patches, but you will need to modify `/usr/share/wayland-sessions/sway.desktop` whenever you re-merge `sway`

I've been daily driving this on a dual-monitor (2k iGPU + vertical 4k dGPU) setup with hybrid graphics (i915 / xe + nvidia) for 3 minor versions now.
Nvidia works on sunshine screen capture, steam and various flatpak apps.

I'm using `~amd64` keyworded `x11-drivers/nvidia-drivers` (`(~)550.67` at the time of writing) and `sys-kernel/pf-sources` (`6.8.0-pf1-ryan`),
so it should be usable on any recent enough OS.

## Usage

Clone this repo, then run

```bash
sudo cp ./patches /etc/portage/ -r
```

and change owner and permissions, it should be like this:

```
# tree -pug /etc/portage/patches

[drwxr-xr-x root     root    ]  patches
├── [drwxr-xr-x root     root    ]  gui-libs
│   ├── [drwxr-xr-x root     root    ]  wlroots-0.16.2
│   │   └── [-rw-r--r-- root     root    ]  nvidia.patch
│   └── [drwxr-xr-x root     root    ]  wlroots-0.17.1
│       └── [-rw-r--r-- root     root    ]  nvidia.patch
└── [drwxr-xr-x root     root    ]  gui-wm
    └── [drwxr-xr-x root     root    ]  sway-1.9
        └── [-rw-r--r-- root     root    ]  nvidia.patch
```

then, run

```bash
sudo emerge -at1 sway wlroots
```

## Environment

The minimal set of environment variables you need is `WLR_NO_HARDWARE_CURSORS=1`,
and hyprland wiki has some more suggestions here: https://wiki.hyprland.org/Nvidia/
