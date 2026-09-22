# Build: OOV/SOTS Script Builder

Single-file HTML tool, sibling to `visualscript.html` in this folder. Read
`visualscript.html` first and reuse its mechanisms wherever this spec says
so — same Tailwind CDN approach, same general code style, same function
naming conventions where they map cleanly. This is **not** a stripped-down
package planner. The document shape is different (a linear reading script
with cue headers, not a full-document split-page table), so treat the
export function as a rewrite, not a trim, even though most of the UI
mechanics carry over directly.

Output filename: `oov-sots-builder.html`.

## What this teaches

OOV/SOTS is the fixed building block that comes before the package:
presenter reads to camera, hands off to a reporter voiceover with shots
logged against it, cuts to one clean soundbite, and stops. No return to
script. The tool exists to force two disciplines: shot variety under the
VO, and hitting a target running time using the actual arithmetic a
newsroom uses (word count ÷ 3 for anything read, real clocked seconds for
anything that was recorded) — not vibes.

## Fixed structure (do not make this an open/reorderable sequence)

1. **ON CAM** — presenter-to-camera copy.
2. **TAKE VO** — reporter voiceover, script + shot-logging.
3. **TAKE SOT** — one soundbite clip.
4. **Out** — nothing follows. No OOV-SOT-OOV, no return to script.

Unlike `visualscript.html`'s `sequence` array (an open, drag-anywhere
board of arbitrary block types), these three zones are fixed and always
present in this order. Only the *shots inside* the TAKE VO zone are
drag-and-drop reorderable — the zones themselves are not.

## Reporter / slug details

Same as the original: Reporter Name and Slug fields at the top, same
validation-on-input pattern (`app.validate()` equivalent).

## ON CAM

- Plain textarea, no shot logging (nothing to log — presenter is in
  vision, there's no directed shot to choose).
- Word count displayed live.
- Counts toward the running-time estimate at word-count ÷ 3.
- On export, auto-generates the `TAKE VO` cue line immediately after it.

## TAKE VO

- Script textarea, same edit/view toggle mechanism as the original
  Reporter Script panel (`toggleScriptMode`, `scriptEdit`/`scriptView`,
  `handleSelection`).
- Shot palette: **Wide, Medium, Close-Up, Unusual Angle, Library,
  Graphic** only. Drop **PTC** and **Interview** from this palette —
  PTC doesn't belong in OOV/SOTS, and Interview becomes the fixed TAKE
  SOT zone below, not a draggable block type.
- Library keeps its attribution/credit field from the original
  (`item.data.attribution`) — sourcing is assessed here.
- Graphic keeps full-screen overlay support from the original
  (`isOverlay`, `overlayText`) — this is distinct from the SOT's
  lower-third, which is handled separately (see TAKE SOT below) and is
  never built via a Graphic block.
- Same drag-and-drop mechanics as the original board
  (`dragStartPalette`, `allowDrop`, `drop`, `dragStartBoard`,
  `dropOnItem`, `dragLeave`) but scoped to this one VO shot board, not
  a whole-document sequence.
- Same "paste selected script text into shot" mechanism
  (`pasteToShot`, `currentSelection`) and per-shot word-count-based
  duration as the original.
- Same "Full Nat" toggle for pure-sound shots with a manual duration
  instead of scripted VO (`isFullUpsot`, `upsotDur`).
- Script tags: **only `[NATPOP=x:Desc]` and `[PAUSE=x]`**. Drop
  `[SOT=x:Words]` entirely (redundant — the SOT is its own fixed zone
  below, never embedded mid-script) and drop `[UPSOT=x:Desc]` unless
  you judge the Full Nat toggle above already covers that case (it
  should — don't duplicate the same feature two ways). Keep
  `[GFX=Text]` if useful for supers logged within the VO itself
  (distinct from the SOT's burnt-in lower-third).
- Counts toward the running-time estimate at word-count ÷ 3, same
  formula as ON CAM, same rule: this is *scripted delivery* (whether
  read to camera or read as VO), so it's always word-count-estimated,
  never treated as real timed footage.
- On export, auto-generates the `TAKE SOT` cue line immediately after
  it.

## TAKE SOT

Fixed single block, not draggable, always follows the VO board directly.

### Fields

- **Name**, **Pronouns**, **Title** — plain text fields.
- These are documented on export as **"Lower-Third (burnt in): Name,
  Title"**, not "Lower-Third:" — students deliver OOV/SOT footage as a
  single file with the lower-third already burnt in, so these fields
  are a record of what's on the tape, never an instruction to graphics
  to add one. Show a short static explainer to this effect in the UI
  near these fields (not a tooltip — visible by default).
- **Full transcript** — textarea, paste the soundbite as spoken.
  **No punctuation/filler cleanup of any kind.** A messy transcript
  stays messy; that's part of what gets assessed.
- **In words / Out words** — auto-extracted from the transcript: first
  ~6–8 words for In, last ~6–8 for Out. Both are editable text fields
  after auto-generation (auto-extraction will sometimes land mid-clause
  on a comma; the student needs to be able to nudge the boundary).
  Recompute the auto-extraction whenever the transcript changes, but
  don't clobber an edit the student has already made to In/Out words —
  only re-run extraction if those fields are still at their
  last-auto-generated value (track this the same way you'd track a
  "dirty" flag).
- **Runs=** — plain manual number input, seconds. **Never
  word-count-estimated.** This is real footage the student has actually
  watched and timed; word-count-per-3 assumes scripted delivery pacing,
  which doesn't hold for spontaneous speech. Label it clearly as a
  manual entry, e.g. "Runs= (actual clip length, as timed by you)".

### Ending technique

Three-option selector: **Hold / Dip & Goldfish / Cutaway.**

- Hold and Dip & Goldfish: no extra fields. (Optional, worth including:
  a short one-line "why this ending?" reflection field on all three
  options, not just Cutaway — cheap to add, stops Hold becoming the
  lazy zero-effort default. Include this if it's not extra scope you
  don't want; flag it as optional in your summary if you skip it.)
- Cutaway only: reveals one shot-logging field (shot type picker from
  the same six-item palette used in the VO board, visual description,
  NAT sound). No script/duration fields on this — it's a resolve at
  the end of the clip, not something with its own timing budget.

### Mid-interview cutaways (separate feature from the ending technique)

This is **distinct** from the ending-technique Cutaway above. The
ending Cutaway is what the piece resolves *to* after the SOT finishes.
This feature is an over-sync visual cutaway *during* the soundbite —
audio keeps running continuously (it's still the same speaker, same
continuous quote) while the visual cuts away and back, typically to
cover an edit point or illustrate what's being said.

- Open-ended list of cutaways per SOT (student can add as many as they
  judge the clip needs).
- Mechanism: once the transcript is in "view" mode (same
  edit/view toggle pattern as the VO script and the original's
  Reporter Script panel), the student selects a span of the transcript
  text (`window.getSelection()` inside the read-only view div, same
  general approach as `handleSelection`/`currentSelection` in the
  original) and marks it as a cutaway. This reveals the same
  shot-type/visual/NAT field set as the ending Cutaway above, attached
  to that span.
- Store each cutaway as `{ start, end, shotType, desc, nat }` using
  **character offsets into the transcript**, not the selected
  substring alone — a substring match is ambiguous if the same phrase
  occurs twice in the transcript.
- **Reject overlapping cutaways.** If a new selection's `[start, end]`
  range overlaps an existing cutaway, show an inline error and don't
  add it. Don't attempt to merge or auto-resolve overlaps.
- **No per-cutaway duration field.** These are a visual layer over the
  existing Runs= timing, not a re-timing of the clip. Keep Runs= as the
  single source of truth for SOT duration.
- **In/Out word extraction ignores cutaway markers entirely.** It
  always runs against the full transcript text regardless of what's
  marked as a cutaway.
- Rendering (both live preview and export): split the transcript into
  segments at the cutaway boundaries. Plain segments render as
  continuous SOT audio; a cutaway segment renders as a row where the
  Visual column shows the shot type/description/NAT and the Audio
  column shows something like "(SOT continues under)" in italic,
  keeping the audio understood as unbroken underneath.

## Live timing readout

Visible and updating continuously (not just checked at export) as a
**compact single-line bar**, not a full card: current total, "/ 50s ±3"
target, a short horizontal progress indicator with the target band
shaded and a marker for the current total, and a short status pill
(e.g. "within range" / "Ns over" / "Ns under").

Formula: `(word_count(ON CAM) + word_count(TAKE VO script)) / 3 +
SOT_Runs_value`. Round the word-based portion up (`Math.ceil`, matching
the original's duration math) same as the original tool does per-shot.
The 50-second target and ±3 tolerance are **hardcoded constants** at the
top of the script (e.g. `const TARGET_SECONDS = 50; const
TOLERANCE_SECONDS = 3;`) — not a settings UI. This makes adjusting for a
different brief (e.g. a 30-second assignment next term) a one-line edit,
without building a per-assignment configuration panel nobody but the
lecturer would use.

## Live script preview

A panel that mirrors the actual export shape, updating live as the
student edits — not a decorative summary. Shape, top to bottom:

- Reporter name — slug, small header line.
- `{ON CAM}` label, then the ON CAM text as prose.
- `TAKE VO` cue line.
- `{TAKE VO}` label, then a two-column Visual/Audio table built from
  the VO shot board and script (same visual-grouping-by-adjacent-shot
  logic the original uses for its DOC export can inform this, scoped
  to just this section).
- `TAKE SOT` cue line.
- `{TAKE SOT}` label, then: Name (pronouns) / Title, "Runs= Ns · Ends:
  [technique]", In words = "…", Out words = "…", and — only if any
  mid-interview cutaways exist — the transcript rendered as segments
  per the rule above.
- If the ending technique is Cutaway, show that shot noted at the very
  end of the SOT block.

The ending-technique selector and any mid-interview cutaways must be
reflected in this preview immediately on change — this is the thing
that makes it a "live" preview rather than two views of the same data
that can silently drift apart from each other.

## Export (.doc)

Generate a linear reading-script document, not the original's
full-document split-page table:

- `{ON CAM}` header, ON CAM text as plain prose.
- `TAKE VO` cue line.
- `{TAKE VO}` header, then — **only this section** — the two-column
  split-page table (Visual | Audio), same general HTML-table-as-.doc
  approach as the original's `exportDoc()` (`Blob(['\ufeff', html], {
  type: 'application/msword' })`).
- `TAKE SOT` cue line.
- `{TAKE SOT}` header, then the SOT metadata block: Name (Pronouns) /
  Title labelled "Lower-Third (burnt in)", Runs=, ending technique, In
  words / Out words, and any mid-interview cutaway rows per the
  rendering rule above.
- Total running time shown at the bottom against the 50±3 target,
  matching what the live readout showed.
- Filename convention: same pattern as the original
  (`${safeReporter}_${slug}.doc`).

## Validation

Adapt the original's `validate()`/missing-fields pattern to check: Name,
Slug, ON CAM text present, at least one VO shot logged, SOT transcript
present, ending technique selected. Disable export until satisfied, same
pattern as `btnDoc`/`btn-disabled`.

## Help modal

Full rewrite, not a trim — explain the ON CAM → TAKE VO → TAKE SOT → Out
shape, the two tags that remain (`[NATPOP=x]`, `[PAUSE=x]`), the
lower-third-is-burnt-in convention, the difference between the ending
Cutaway and a mid-interview cutaway, and the 50±3 timing rule
("script word count ÷ 3, plus your actual timed SOT — real newsroom
arithmetic, not a guess").

## Explicitly out of scope (do not build)

- No open/reorderable top-level sequence — the three zones are fixed.
- No interview splits/MCU/Part-numbering system from the original — the
  mid-interview cutaway feature above replaces it, and is simpler by
  design (single continuous speaker, offset-based markers, no
  part-numbering).
- No PTC block.
- No `[SOT=x:Words]` inline tag.
- No per-cutaway duration field.
- No settings UI for the target duration/tolerance — constants only.
- No OOV-SOT-OOV / return-to-script structure.
