# Ferris Sweep Cardio — Technical Reference

## Hardware

| Component | Detail |
|-----------|--------|
| PCB | Ferris Sweep Cardio (34-key split, Pro Micro footprint) |
| Controller | nice!nano v2 (nRF52840, 256KB RAM, 1MB Flash) |
| Connectivity | BLE + USB |
| Halves | Left (central) and right (peripheral) |
| Reset | Double-tap reset button on nice!nano to enter bootloader |
| Bootloader | UF2 — mounts as `NICENANO` USB drive |

## ZMK Config

| Setting | Value |
|---------|-------|
| ZMK shield | `cradio_left` / `cradio_right` |
| ZMK board | `nice_nano_v2` |
| ZMK version | `main` |
| GitHub repo | `dzegna/zmk-config` |
| Config file | `config/cradio.conf` |
| Keymap file | `config/cradio.keymap` |
| ZMK Studio | Enabled via `studio-rpc-usb-uart` snippet + `-DCONFIG_ZMK_STUDIO=y` |
| Studio locking | Disabled (`CONFIG_ZMK_STUDIO_LOCKING=n`) — connects freely |

## Key Layout

34 keys: 3 rows × 5 columns per half + 2 thumb keys per half.

```
 0   1   2   3   4      5   6   7   8   9
10  11  12  13  14     15  16  17  18  19
20  21  22  23  24     25  26  27  28  29
               30  31     32  33
```

```
Row 0:  Q   W   E   R   T  │  Y   U   I   O   P
Row 1:  A   S   D   F   G  │  H   J   K   L  BSP
Row 2:  Z   X   C   V   B  │  N   M   ,   .   /
Thumb: GUI ALT              │ S/SP ENT
```

### Thumb positions

| Pos | Location | Tap | Hold |
|-----|----------|-----|------|
| 30 | Left outer | LGUI (⌘) | — |
| 31 | Left inner | LALT (⌥) | — |
| 32 | Right inner | Space | Shift |
| 33 | Right outer | Enter | — |

## Layers

| # | Name | How to access |
|---|------|---------------|
| 0 | Default | — |
| 1 | Sym | F+J combo (enter) / G+H combo (exit) |
| 2 | Nav | Z+/ combo (toggle) |
| 3 | BT | Q+P combo (toggle) |

### Layer 1 — Sym
```
Row 0:  !   @   #   $   %  │  ^   &   *   (   )
Row 1:  1   2   3   4   5  │  6   7   8   9   0
Row 2:  `   [   ]   {   }  │  \   -   =   ;   '
```

### Layer 2 — Nav
```
Row 0: F1  F2  F3  F4  F5  │  F6  F7  F8  F9 F10
Row 1: F11 F12  —   —   —  │  ←   ↓   ↑   →   —
Row 2: exit —   —   —   —  │ HOM PGD PGU END exit
```

### Layer 3 — BT
```
Row 0: BT0 BT1 BT2 BT3 BT4 │  —   —   —   —   —
Row 1: CLR USB BLE  —   —   │  —   —   —   —   —
Row 2: exit —   —   —   —   │  —   —   —   —  exit
```

- `BT0–BT4`: select Bluetooth profile (device)
- `CLR`: clear all pairings
- `USB` / `BLE`: switch output between USB and Bluetooth

## Combos

| Combo | Positions | Action |
|-------|-----------|--------|
| Tab | 12 + 13 (D + F) | Tab |
| ESC | 30 + 10 (GUI + A) | ESC |
| CAPS | 30 + 11 (GUI + S) | Caps Lock |
| Enter sym | 13 + 16 (F + J) | Switch to layer 1 |
| Exit sym | 14 + 15 (G + H) | Switch to layer 0 |
| Nav toggle | 20 + 29 (Z + /) | Toggle layer 2 |
| BT toggle | 0 + 9 (Q + P) | Toggle layer 3 |

Combo timeout: 50ms

## Flashing

1. Double-tap reset on the nice!nano — `NICENANO` mounts as a USB drive
2. Copy firmware via terminal (do NOT drag-and-drop — too slow on macOS):
   ```
   cp <firmware>/cradio_left-nice_nano_v2-zmk.uf2 /Volumes/NICENANO/
   ```
3. Device auto-reboots when done — flash each half separately
4. Flash left first, then right
5. Single-tap reset on both halves to re-pair them to each other

## ZMK Studio

- Connect left half via USB
- Open ZMK Studio (native app or studio.zmk.dev in Chrome/Edge)
- Click Connect → select the USB serial device
- No unlock key needed (locking disabled)

## Building Firmware

Firmware builds automatically via GitHub Actions on push to `dzegna/zmk-config`.

```bash
# Check build status
gh run list --repo dzegna/zmk-config --limit 5

# Download latest firmware
gh run download <run-id> --repo dzegna/zmk-config --dir ~/Downloads/zmk-ferris/firmware-new
```
