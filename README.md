[README.md](https://github.com/user-attachments/files/33253653/README.md)
# Zelda3 Texture Tool

View, redraw, export and import the graphics of the snesrev **zelda3** reimplementation.
It edits `zelda3_assets.dat` and the Link `.zspr` that `zelda3.ini` points at, directly. You don't need the ROM or a rebuild of `zelda3.exe`.

## Start

Double-click **`Zelda3 Texture Tool.exe`** in the zelda3 folder. It opens in its own window and closes down by itself when you close that window.

* The exe carries the tool inside it. It runs it with the Python that already ships in `assets\`, so there's nothing to install.
* It finds the game when it sits in the zelda3 folder or up to two folders below it. Anywhere else, it asks you once for the folder.
* The exe isn't code-signed, so Windows may show "Windows protected your PC" the first time. Click **More info → Run anyway**.
* `texture_tool\Texture Tool.bat` starts the same tool with a console window. That's useful if something goes wrong.
* If it misbehaves, there are logs in `%LOCALAPPDATA%\Zelda3TextureTool\` (`output.log`, `error.log`).

## Drawing

Every pixel stores a **color slot number** (0–7 for most sheets, 0–3 for HUD/font, 0–15 for Link). The palette menu only changes the preview colors. The game chooses the real colors depending on where the sheet is used.

| Tool | Key | What it does |
|---|---|---|
| Pencil | B | Left click draws with the **L** slot, right click with the **R** slot |
| Fill | G | Flood-fills same-slot pixels. "Fill in tile" keeps it inside one 8×8 tile |
| Pick | I (or Alt+click) | Takes a pixel's slot as L (left) or R (right) |
| Select | M | Drag a box. "Snap to tiles" selects whole 8×8 tiles |

* **Choose slots:** click a swatch (left click for L, right click for R), press `0`–`9` (`Shift` for R), use `[` `]` to step through, or `X` to swap L and R.
* **Selection tools:** Copy, Cut and Paste (`Ctrl+C/X/V`; paste lands at the selection or under the mouse), Flip ↔ ↕ (`H`/`V`), Clear to slot 0 (`Delete`).
* **Remap… (`R`):** replaces or swaps slot numbers in the selection or the whole sheet, for example "every pixel using slot 3 → slot 5". This is how you recolor a sprite with the colors the game already has.
* **Display:** tick *Numbers* to print each pixel's slot number (zoom 12× and up). Zoom with the menu, `+`/`−`, or Ctrl + mouse wheel.
* Undo/Redo (`Ctrl+Z` / `Ctrl+Y`) is kept per sheet. **Shortcuts** in the top bar lists every key.

Edited sheets get an orange dot. Nothing is written to the game until you click **Save to game** (`Ctrl+S`). After saving, restart zelda3.

## What you can edit

| ID | What | Format |
|---|---|---|
| `spr_000`–`spr_102` | Sprite sheets: enemies, NPCs, items, sword/shield (0x5E/0x5F) | 3bpp, 128×32, 8 slots |
| `spr_103`–`spr_107` | HUD / menu / icon sheets | 2bpp, 128×64, 4 slots |
| `bg_000`–`bg_112` | Background tilesets: overworld and dungeon tiles (97–112 are blank) | 3bpp, 128×32 |
| `bg_113`, `bg_114` | HUD tiles | 2bpp, 128×64 |
| `link` | Link's built-in sprite (`kLinkGraphics`) | 4bpp, 128×448, 16 slots |
| `zspr_link` | The `.zspr` set by `LinkGraphics=` in zelda3.ini. **This is the Link the game actually shows.** | 4bpp, 128×448 |
| `font` | Dialogue font, 256 glyphs (glyph widths stay as they are) | 2bpp, 128×128 |
| `worldmap` | Mode 7 overworld map tiles | 8bpp, 128×128 |

## Using another image editor

Click **Export PNG**, edit the file in Aseprite, GraphicsGale, Photoshop and so on, then use **Import…** or drag the file onto the canvas.

* Keep the exact image size.
* Keeping the image *indexed* works best. RGB images are snapped to the nearest color of the selected palette.

**Export all** and **Import all** do every sheet at once through `texture_tool\export\`.

## Safety

* The first save copies the untouched file to `zelda3_assets.dat.orig`. Every save backs up the previous version to `zelda3_assets.dat.bak`. The `.zspr` gets a `.orig` copy too.
* To undo everything, copy `zelda3_assets.dat.orig` back over `zelda3_assets.dat`.
* Re-running `extract_assets.bat` (or a launcher re-extract) rebuilds `zelda3_assets.dat` from the ROM and **erases your edits**. Export your work first and use **Import all** afterwards.

## Command line

```
assets\python.exe texture_tool\zelda3_texture_tool.py list
assets\python.exe texture_tool\zelda3_texture_tool.py export-all [DIR]
assets\python.exe texture_tool\zelda3_texture_tool.py import-all [DIR]      (imports and saves)
assets\python.exe texture_tool\zelda3_texture_tool.py export spr_023 moblin.png [--palette "NAME"]
assets\python.exe texture_tool\zelda3_texture_tool.py import spr_023 moblin.png
```
