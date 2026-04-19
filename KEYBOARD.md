# Ferris Sweep — Technical Reference

## Hardware

| Component | Detail |
|-----------|--------|
| PCB | Ferris Sweep (34-key split, Pro Micro footprint) |
| Controller | nice!nano v2 (nRF52840, 256KB RAM, 1MB Flash) |
| Connectivity | BLE + USB |
| Halves | Left and right, each with a nice!nano |
| Reset | Double-tap reset button on nice!nano to enter bootloader |
| Bootloader | UF2 — mounts as `NICENANO` USB drive |

## ZMK Config

| Setting | Value |
|---------|-------|
| ZMK shield | `cradio_left` / `cradio_right` |
| ZMK board | `nice_nano_v2` |
| ZMK version | `v0.3` |
| Config file | `config/cradio.conf` |
| Keymap file | `config/cradio.keymap` |
| ZMK Studio | Enabled (`-DCONFIG_ZMK_STUDIO=y`) |
| Studio locking | Disabled (`CONFIG_ZMK_STUDIO_LOCKING=n`) |

## Key Layout

34 keys: 3 rows × 5 columns per half + 2 thumb keys per half.

```
 0   1   2   3   4      5   6   7   8   9
10  11  12  13  14     15  16  17  18  19
20  21  22  23  24     25  26  27  28  29
               30  31     32  33
```

### Thumb positions
| Position | Location | Default binding |
|----------|----------|-----------------|
| 30 | Left inner thumb | `LT(NAV, TAB)` |
| 31 | Left outer thumb | `ENTER` |
| 32 | Right inner thumb | `LT(NUM, SPACE)` |
| 33 | Right outer thumb | `LT(SYM, BSPC)` |

## Layers

| # | Name | Activated by |
|---|------|--------------|
| 0 | Default | — |
| 1 | NAV | Hold pos 30 (left inner thumb) |
| 2 | OTHER | — (unused) |
| 3 | NUM | Hold pos 32 (right inner thumb) |
| 4 | SYM | Hold pos 33 (right outer thumb) |

## Homerow Mods

Behavior: `tap-preferred`, `tapping-term-ms = 200`

| Key | Tap | Hold |
|-----|-----|------|
| A (pos 10) | `A` | `LGUI` (CMD) |
| S (pos 11) | `S` | `LALT` |
| D (pos 12) | `D` | `LCTRL` |
| F (pos 13) | `F` | `LSHFT` |
| J (pos 16) | `J` | `RSHFT` |
| K (pos 17) | `K` | `RCTRL` |
| L (pos 18) | `L` | `LALT` |
| `'` (pos 19) | `'` | `LGUI` |

## Combos

| Combo | Positions | Action |
|-------|-----------|--------|
| ZMK Studio unlock | 30 + 33 (left inner + right outer thumb) | `&studio_unlock` |

Combo timeout: 100ms

## Flashing

1. Double-tap reset on the nice!nano
2. `NICENANO` mounts in Finder
3. Flash via terminal (do NOT drag-and-drop in Finder — too slow on macOS):
   ```
   cp ~/Downloads/zmk-ferris/firmware/cradio_left-nice_nano_v2-zmk.uf2 /Volumes/NICENANO/
   cp ~/Downloads/zmk-ferris/firmware/cradio_right-nice_nano_v2-zmk.uf2 /Volumes/NICENANO/
   ```
4. Device reboots automatically when done
5. Flash each half separately

## Building

Firmware is built via GitHub Actions on every push to `main`.

```
gh run list --repo dzegna/zmk-config --limit 5
gh run download <run-id> --repo dzegna/zmk-config --dir ~/Downloads/zmk-ferris
```
