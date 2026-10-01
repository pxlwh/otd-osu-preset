# otd-osu-preset

My [OpenTabletDriver](https://opentabletdriver.net/) preset for osu!, tuned for a **One by Wacom CTL-472** played **hovering** (pen above the surface) on a 1920x1080 240 Hz class monitor.

## The preset

| Setting | Value |
|---|---|
| Tablet | Wacom CTL-472 |
| Output mode | Absolute |
| Tablet area | 75 x 42.19 mm (16:9, matches the screen), rotated 180 |
| Display area | one 1920x1080 monitor |

Filters, in order:

| Filter | Settings | Why |
|---|---|---|
| Kuuube's CHATTER EXTERMINATOR (SMOOTH) | strength 15 | Anti chatter. The CTL-472 has no hardware smoothing, so its raw signal jitters, and hovering is noisier than dragging. The author recommends 6 to 7 for dragging and 15 to 16 for hovering. |
| Radial Follow Smoothing (tablet space) | outer radius 1 mm, coefficient 0.995 | Absorbs small hand tremor: the cursor stays put until the pen moves past the radius. |
| Temporal Resampler | prediction 0.5, reverse EMA 1.0, maximize frequency on | The tablet reports 133 times a second; this interpolates between reports so the cursor moves on every frame of a high refresh monitor. Reverse EMA stays at 1.0 (off) because this tablet has no hardware smoothing to undo. |

## Install

1. Install the plugins in OpenTabletDriver (Plugins > Open Plugin Manager, or from their repos):
   * [Kuuube's CHATTER EXTERMINATOR](https://github.com/Kuuuube/Kuuube-s-CHATTER-EXTERMINATOR)
   * [AbstractQbit's Radial Follow Smoothing](https://github.com/AbstractQbit/AbstractOTDPlugins)
   * [Temporal Resampler](https://github.com/shmkle/TemporalResampler) (manual install: put the DLL in a `TemporalResampler` folder under OpenTabletDriver's `Plugins`)
2. Download [`OpenTabletDriver/settings.json`](OpenTabletDriver/settings.json), rename it to `ctl472-osu.json`, and put it in OpenTabletDriver's `Presets` folder (a preset and a settings file share one format):
   * Linux: `~/.config/OpenTabletDriver/Presets/`
   * Windows: `%localappdata%\OpenTabletDriver\Presets\`
3. In the GUI, apply the preset, then **set the display area for your own monitor layout** (the saved one points at my middle screen), Apply and Save.

On a different tablet, copy the filter values by hand rather than loading the file.

## Tuning notes

* If the cursor shimmers while the pen is held still, raise the chatter strength. If small aim corrections feel sluggish, lower it or shrink the radial follow radius.
* Area: the biggest area you can comfortably cover is steadier. Aiming short means shrink it, overshooting means grow it.
* Filter strengths are in tablet millimetres, so they get effectively stronger when you shrink the area.

## Linux notes

On Wayland, OpenTabletDriver recommends Artist mode, with the compositor pinning the virtual tablet to a monitor (on Hyprland: `hl.device({ name = "opentabletdriver-virtual-artist-tablet", output = "DP-1" })`). This preset uses Absolute mode, which also works for osu!stable under [osu-winello](https://github.com/NelloKudo/osu-winello): it detects Absolute mode and enables Wine's absolute tablet workaround. In osu!stable, set "Confine mouse cursor" to Never, or the pen can freeze when a map starts under XWayland.

## How this repo stays current

On my machine `~/.config/OpenTabletDriver` is a symlink to this repo's `OpenTabletDriver/` folder, and git tracks only `settings.json` inside it. So the file here is my live config, not a copy. The link is on the folder, not the file, because OpenTabletDriver saves by deleting `settings.json` and creating a new one, which would replace a file symlink with a plain file.

## License

MIT for the preset and this README. The plugins are separate projects under their own licenses (GPL-3.0) and are not included.
