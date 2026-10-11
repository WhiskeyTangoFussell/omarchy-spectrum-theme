# Rainbow rain patches for omatrix

Stock [omatrix](https://github.com/0x1ocean/omarchy-omatrix) colours the whole rain with one `color`. Spectrum's
`omatrix.toml` uses four extra settings that these patches add:

| Key | Spectrum's value | What it does |
|---|---|---|
| `tail_colors` | 12 hues separated by `\|` | Each stream gets one colour group, picked per column |
| `tail_fade` | `0.85` | Each tail darkens from the head down to 15% brightness |
| `tail_hue_shift` | `3` | Each tail also drifts three steps round the 12-hue wheel |
| `update_delay` / `battery_update_delay` | `9.375` | Fractional delays (stock rounds them to whole numbers) |

The patches also add `justify_columns`, which spaces columns evenly across the screen. They expose the colour
groups to a cloned lock screen, so a customised `omarchy.lock` can show the same rain. Without these patches,
the keys are ignored and the rain falls back to the theme's green.

## Apply

`omatrix-local.patch` holds 4 commits made against upstream commit `fe60537`. It includes the compiled shader,
so nothing has to be built. 19 tests pass with them.

```bash
omarchy plugin add https://github.com/0x1ocean/omarchy-omatrix.git --enable   # if not installed yet
cd ~/.config/omarchy/plugins/io.github.0x1ocean.omatrix
git am /path/to/Omarchy-Theme-Spectrum/extras/omatrix/omatrix-local.patch
omarchy restart shell
```

Check: `omarchy-shell omatrix status | jq '{active, themeOptsIn, tailGroups}'` should list the 12 colour groups
while Spectrum is applied.

## Plugin updates

`omarchy plugin update` only fast-forwards, so with these commits on top it stops. To update, take the patches
off, update, then put them back:

```bash
cd ~/.config/omarchy/plugins/io.github.0x1ocean.omatrix
git reset --hard origin/HEAD && git pull --ff-only
git am /path/to/omatrix-local.patch      # resolve any conflict, then: git am --continue
omarchy restart shell
```

## Remove

```bash
cd ~/.config/omarchy/plugins/io.github.0x1ocean.omatrix && git reset --hard fe60537 && omarchy restart shell
```

License: GPL-3.0-or-later, the same as omatrix.
