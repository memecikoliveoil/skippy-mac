# skippy-mac

A fork of [skippy-xd](https://github.com/felixfung/skippy-xd) by Felix Fung, tuned to feel closer to macOS: an exposé that works together with a real dock, cleaner labels and a few behaviour fixes. Built and used daily on a MacBook Air (mid 2012) running Xubuntu 24.04 with Plank and picom.

Everything that is general enough is sent upstream as a pull request first. This fork carries the rest, plus upstream PRs that are still open.

The program, its command (`skippy-xd`) and its config (`~/.config/skippy-xd/skippy-xd.rc`) keep their original names, so existing setups and scripts keep working.

## What's different

### Dock stays real and clickable

```
[panel]
passthrough = plank
```

Panels whose WM_CLASS matches `passthrough` (same syntax as `--wm-class`) are not drawn as a copy. The real dock stays visible and clickable on top of the exposé, like the Dock during Mission Control. Clicking it closes skippy and the click goes to the dock as usual: open an app, raise its windows, and so on. Works best with `pseudoTrans = false`. Use `show = false` to hide the remaining panels, for example the top panel.

The dock sits on skippy's own background: windows behind its transparent parts (Plank's window is much larger than the dock it draws) do not show through, wherever the dock is.

With xfwm4, set `easy_click` to something other than the key that holds switch open (usually Alt), or xfwm4 takes clicks on the dock during Alt-Tab for moving windows: `xfconf-query -c xfwm4 -p /general/easy_click -s Super` (or `none`).

### Limit the reserved panel space

```
[panel]
maxThickness = 80
```

With `reserveSpace = true`, at most this many pixels are reserved per panel (0 = no limit). Useful when a dock window is much taller than what it draws, as with Plank.

### "No Available Windows"

```
[label]
emptyText = No Available Windows
```

When there is no window to show, this text is shown in the middle of the screen as a label, like macOS. Empty by default (off).

### Label padding

```
[label]
cornerRadius = 6
padding = 6
```

`cornerRadius` rounds the corners of the label box; the border is drawn along the curve. `padding` adds even space around the text on all sides and centers it (0 = tight, as upstream).

### Closes when a new window opens

If a new window is mapped while the exposé is open, skippy closes, so the new window is not hidden behind it.

### Other changes

- Windows moved partly off the screen (their middle outside every monitor) are laid out on the nearest monitor instead of being left out with a stale preview.
- Panel geometry is remembered while a panel is auto-hidden.
- Window capture, focus and layout fixes from Tomas M's pull requests, among them: cached previews for unmapped windows, switch and expose laid out on the current monitor, real focus recency for the first Alt-Tab selection, and a stable merge sort in place of bubble sort.

## Upstream status

At the time of writing (upstream 0.12.1, 11 Nov 2026):

| Change | Upstream |
|---|---|
| Label `show = all / focused / none`, `position = top / mid / bottom` | merged |
| skip-taskbar / skip-pager in any `_NET_WM_STATE` position | merged (#579) |
| Crop `_GTK_FRAME_EXTENTS` from previews of CSD windows | merged |
| Panel geometry from the input shape | merged (#565) |
| Rounded labels (`cornerRadius`) | merged (#581) |
| Exit only when the last shown window goes away | merged (#582) |
| Label padding | merged (#586), adds to the default spacing there |
| Windows partly off the screen laid out on the nearest monitor | open PR |
| Tomas M's fixes | his open PRs, #567 merged here |
| Dock passthrough (with its background), `maxThickness`, `emptyText`, close on new window | only in this fork |

## Build

```
git clone https://github.com/memecikoliveoil/skippy-mac.git
cd skippy-mac
meson setup build --prefix=/usr/local
meson compile -C build
sudo meson install -C build

skippy-xd --start-daemon
skippy-xd --expose
```

For everything else (other options, key bindings, paging, switch mode) see the [upstream README](https://github.com/felixfung/skippy-xd#readme) and [wiki](https://github.com/felixfung/skippy-xd/wiki).

## Credits

skippy-xd is maintained by [Felix Fung](https://github.com/felixfung), with contributions from many people over the years. Thanks to Felix for the reviews and to Tomas M for his fixes. Licensed under the GPL, like upstream.
