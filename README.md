# Yorkshire Line Editor

A graphical 2D editor for route JSON files consumed by the
[Yorkshire Train Simulator](https://github.com/BrendanJamesLynskey/Airedale-Wharfedale-Sim).

[Use the editor here](https://brendanjameslynskey.github.io/Yorkshire-Line-Editor/)

The editor is a single-file HTML app with no build step, mirroring the sim's
deployment pattern. Layouts persist to `localStorage` so refreshing won't lose
work; exporting produces a JSON file that drops straight into the sim via its
`LOAD FROM FILE…` / `LOAD FROM URL…` buttons.

## What you can place

| Tool | What it does |
|------|--------------|
| **＋ Station** | Drag onto the canvas. Auto-numbered along the route in placement order. Editable name, code, platform length, side (Left / Right / Island), elevation, **station type** (Halt / Station / Terminus / Junction), **platform count** (1–6), **style** (Modern / Victorian / Rural) and a **Footbridge** checkbox in the sidebar. Halts skip the canopy; terminus stations get a buffer stop at the route end; junction stations get a 60 m branch rail stub at 30°; platform counts above 2 add proper "wing" platforms with their own rails. Style picks the shelter look — Modern is the default grey slab, Victorian is an ornate dark-green canopy with a brick back wall and cast-iron posts, Rural is a small wooden lean-to. The Footbridge checkbox adds a pedestrian overpass with parapet-railed staircases dropping to each primary platform. Every non-halt platform face automatically gets a ticket-machine kiosk (no UI needed). |
| **＋ Bridge / Tunnel / Viaduct / Mill** | Drag onto the canvas. Snaps to the nearest point on the track. Length and (for mills) side editable. |
| **＋ River pt** | Click to extend a polyline that becomes the river. The exporter converts each point to a `{km, lateral-offset-metres}` pair against the track. |
| **＋ Limit** | Drop a speed-limit marker onto the track. Default 60 mph, editable in the sidebar. The route colours itself along the curve by the active limit (green ≥60, amber 40–60, orange 25–40, red <25). |
| **▣ Select** | Default tool. Click to select, drag to move (including the speed-limit / feature / river markers — they re-snap to the track on drop). |

Speed limits / features / river points all stay snapped to the live track as
stations are added, moved or reordered. A station list in the sidebar lets
you reorder (▲ ▼), delete or quickly select.

## Track curve

The track is a Catmull-Rom curve through stations in array order. Re-order
stations in the sidebar with ▲ ▼; the route redraws and `km` values along
the whole line update. Feature / limit / river snap-targets resample on each
change so nothing drifts.

## Coordinate system

The canvas is in metres, +X east, +Y north. The grid is 1 km minor / 5 km
major; the scale bar at the bottom-left always reads 5 km. Pan by dragging
the background; scroll-wheel to zoom; `F` (or the **Fit** button) frames the
whole route.

On export, world coordinates are converted to plausible WGS84 lat/lon by
mapping (x, y) metres to small offsets from Leeds (53.7958 °N, -1.5479 °W) —
the sim's projection origin. This means a saved file loads into the sim with
the route projected into the sim's existing world frame. You don't have to
pick "real" locations for new lines; arbitrary positions on the canvas work.

## Keyboard

| Key | Action |
|-----|--------|
| `Esc` | Drop the current tool / clear selection |
| `Delete` / `Backspace` | Delete the selected item |
| `F` | Fit zoom to the entire route |

## File format

The exported JSON has the same shape as the
[sample line](https://github.com/BrendanJamesLynskey/Airedale-Wharfedale-Sim/blob/main/sample-line.json)
that ships with the sim. The sim's README documents the schema; the editor
fills in `version`, `displayName`, `subtitle`, sorted `features` /
`limits`, and an empty `shapePoints` array. Reopening a file in the editor
reverses the projection so you can keep editing.

## Repo

[BrendanJamesLynskey/Yorkshire-Line-Editor](https://github.com/BrendanJamesLynskey/Yorkshire-Line-Editor)

## Licence

MIT.
