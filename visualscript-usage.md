# VISUALSCRIPT Operating Instructions

This document explains how to use `visualscript.html` to build a package plan, sequence shots, add reporter script, and export a 4-column script.

## 1. Open the page

- Open `visualscript.html` in your browser.
- The page is a planner for building a visual script package with drag-and-drop shot blocks and live timing.

## 2. Enter package details

- Fill in `Reporter Name`.
- Fill in `Slug` (assignment title).
- These values are used in the preview and the exported document.

## 3. Add blocks

- Drag block tiles from the palette into the board area.
- Available block types:
  - `Wide Shot`
  - `Medium Shot`
  - `Close-Up`
  - `PTC`
  - `Unusual Angle`
  - `Library`
  - `Graphic`
  - `Interview`

## 4. Arrange and edit the sequence

- Reorder blocks by dragging them inside the board.
- Use the arrow buttons on each block card to move a block up or down.
- Remove a block with the `✕` button.

## 5. Complete each block

- Non-interview blocks:
  - Add a visual description.
  - Add `NAT Sound` if needed.
  - Enter block script text.
  - Use `Full Nat` to force a fixed natural sound duration.
  - Use `Overlay Gfx` to attach graphic text.

- Interview blocks:
  - Add lower-third `Name` and `Job Title`.
  - Edit the `MCU` speech text.
  - Add split segments and cutaways using `+ Add Split` and `+ Add shot`.

## 6. Use the reporter script editor

- Enter the full reporter script in the right-hand script editor.
- Click `Done` to switch from editing mode to viewer mode.
- Select text in the viewer and use the paste button on a shot block to insert the selected text into that block.

## 7. Use script timing tags

Use these tags inside block scripts to control timing and visuals:

- `[NATPOP=x]` — pause for `x` seconds of natural sound.
- `[SOT=x:Words]` — add `x` seconds of a soundbite.
- `[PAUSE=x]` — insert a silence break of `x` seconds.
- `[GFX=Text]` — insert a graphic instruction at that point.

## 8. Review the live preview

- The preview panel shows a 4-column layout:
  - Visuals
  - Audio: Sync
  - Audio: NAT
  - Duration
- Total runtime updates automatically as you edit.

## 9. Export the script

- Once all required fields are filled and at least one block exists, the `Export Script (.doc)` button is enabled.
- Click it to download a Word-compatible `.doc` file.

## 10. Reset the page

- Use `Start New Assignment` to clear the form and begin again.

## 11. Open help

- Click `Help` in the top right for a summary of how the page works and the supported script tags.
