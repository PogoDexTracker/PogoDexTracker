# POGO Dex Tracker — v1.4.2 Beta

Initial release: July 20, 2026
Last updated: July 20, 2026 (v1.4.2 — see changelog below)

## What's in this build

**Sign-in & storage**
- Google Sign-In gates access to the app.
- Your dex data (caught, shiny, forms, trade quantities, trainer code) saves to a single JSON file in your own Google Drive using the `drive.file` scope. The app never sees or touches anything else in your Drive.
- Writes are debounced (~1.2s after your last change) so it's not hammering the Drive API on every click.

**Pokédex tab**
- Full National Dex coverage, #1–1025.
- Search by name or dex number.
- Region filter and auto-grouping (Kanto through Paldea), matching how Pokémon GO itself groups regions — Hisuian species are folded into Galar since GO never released Hisui as its own region.
- Per-species: mark caught, mark shiny, set a duplicate count for trading — caught/shiny toggle directly on each card, no need to open the detail view for the common case.
- Regional/alternate forms pulled in on demand and cached locally, each with its own caught/shiny toggle (still via the detail view).

**Inventory tab**
- Flat, filterable view of everything you've caught, including forms, with shiny and trade-duplicate tags.

**Trade Builder tab**
- "Wanted" list: search and add anything, with a shiny toggle.
- "Offering" list: pulled from your caught inventory, check off what you're willing to trade.
- Generate Trade Card: renders a shareable card in the classic blue trade-list style, includes your Trainer Code if set, and downloads as a PNG.

**Profile tab**
- Holds your Trainer Code (moved here from Trade Builder).
- First sign-in of a session prompts you to add it if it's not set yet, with a "Skip for now" option.

**Branding**
- App favicon, header badge, and sign-in screen now use the PogoDex Tracker logo.
- Typeface standardized on Roboto / Roboto Mono throughout.

## Known limitations in this beta

- **Forms data source**: alternate/regional forms currently come from PokeAPI, which reflects the mainline games. Some forms shown may not actually exist in Pokémon GO yet (or vice versa — GO-specific things like Dynamax/Gigantamax/Shadow aren't reflected). A GO-accurate data source has been identified for a future build.
- **Trade Builder selections don't persist** — your Wanted/Offering picks reset if you reload mid-session. Everything else does persist.
- **No offline support** — needs a connection for sign-in, PokeAPI lookups, and Drive sync.
- Requires a one-time Google Cloud OAuth Client ID setup before first use (see setup notes provided separately).

## Planned for future betas
- GO-accurate form/shiny/availability data
- Persisted Trade Builder state
- Possible CP/IV or move tracking, if wanted

## Changelog

### v1.4.2 Beta
- Rotated the header's lightning bolt watermark 45° clockwise, per follow-up request. Same low-opacity treatment and tiling, just angled.

### v1.4.1 Beta
- **Fixed a regression from v1.3**: the tightened "only show PoGoAPI-confirmed Alolan/Galarian forms" filter had the side effect of hiding *all* of Pikachu's variations (and any other species whose real GO variants are event costumes/caps rather than regional forms), since those don't match the Alolan/Galarian naming pattern the filter checked for. Reverted to showing every PokeAPI variety again. PoGoAPI confirmation now shows as an "(unconfirmed in GO)" badge on the form instead of hiding it outright, informational rather than exclusionary.
- Also fixed a cache key mismatch introduced in that same change (forms were being saved under one key and read from another, so the fix wouldn't have taken effect on repeat visits without this).

### v1.4 Beta — Simplified interaction model (walked back v1.0's quantities)
Direct response to feedback that number entry was tedious. This walks the data model back from v1.0's quantity counters to simple on/off toggles, while keeping a safe trade-confirmation flow.

- **Caught/Shiny are checkmarks again**, not number fields, tap to toggle, same as the original design. Independent for normal vs. shiny.
- **Trade Available is now a simple toggle** ("do I have a spare of this to offer?"), not a quantity. Existing data with actual quantities migrates automatically: any prior quantity greater than zero becomes "available," zero becomes "not available."
- **Detail modal removed entirely**, replaced by an inline **"View Variations"** expand right on each dex card. No more popup, everything happens in place: Trade Available toggles for the base Pokémon, plus each regional/alternate form with its own Caught/Shiny/Trade-Available toggles.
- **New sparkle icon** for shiny status, an original design (not a copy of the game's own icon asset, that stays their IP) inspired by the general "sparkle badge" convention used across countless apps.
- **Trade Builder Offering list** simplified to checkboxes (anything flagged Trade Available shows up, check what you want on this card).
- **Confirm Trade Completed reworked around a snapshot**: Generate Trade Card now locks in exactly what was selected at that moment. Confirm only acts on that locked-in snapshot, flipping Trade Available off for those specific items, so changing your selections after generating a card can't accidentally clear the wrong things.
- Google Sheets export simplified to match (Shiny / Trade Available columns as Yes/No, no quantity columns).

### v1.3 Beta — PoGoAPI.net data layer
Data accuracy layer added from PoGoAPI.net; images continue to come entirely from the existing PokeAPI-based sprite pipeline, this integration only adds GO-specific facts on top.

- **Not-yet-released Pokémon** now show visibly dimmed in the Pokédex grid with a "Not in GO yet" label, based on PoGoAPI's `released_pokemon.json`. If PoGoAPI fails to load for any reason, the app falls back to treating everything as released rather than dimming the whole dex incorrectly.
- **Shiny availability note** added to the Pokédex detail modal (e.g. "Shiny available: Wild, Raid" or "Shiny not yet available in GO"), from `shiny_pokemon.json`.
- **Alternate forms tightened to confirmed GO content**: the Forms list now only shows Alolan/Galarian variants that PoGoAPI's `alolan_pokemon.json`/`galarian_pokemon.json` actually confirm exist in GO, rather than every mainline-game variety PokeAPI happens to list (which included things like Mega/Totem/Hisuian/Paldean forms that aren't real GO content, or in some cases weren't actually confirmed released). Local form cache bumped again to clear out the old broader list.
- **Shadow added as a form option** where PoGoAPI's `shadow_pokemon.json` confirms it, since Shadow is GO-exclusive and never existed as a PokeAPI variety. Known simplification: there's no distinct Shadow artwork source available yet, so it currently reuses the base species art rather than a Shadow-specific sprite.
- PoGoAPI data is cached locally and refreshed once every 24 hours.

### v1.2 Beta
- Swapped the header's paw-print watermark for a subtle lightning bolt watermark instead, per follow-up request. Same low-opacity slate blue-gray treatment, same tiling approach, just a different shape. Original bolt artwork drawn for this, not a copy of any specific reference image.

### v1.1 Beta — Visual refresh
- Swapped the bright phosphor-green accent for a more muted, understated slate blue-gray throughout the app chrome (buttons, tabs, highlights). Deep navy base kept as-is.
- Replaced the diagonal pinstripe texture in the header with a subtle, generic paw-print watermark, echoing the footprint marker already in the app's own logo, rather than the busier stripe pattern.
- Note: a literal Pikachu silhouette was requested for this but wasn't used since that would reproduce a specific copyrighted character design; the generic paw-print keeps the "tracking" theme without that issue.
- Trade card (Master Ball holo) styling left as-is, that was a deliberate, separate look from the main app chrome.

### v1.0 Beta — Inventory quantity rework
This is a genuine architecture change: caught/shiny went from a single boolean per species to real quantity counters, split independently for normal and shiny. Your existing data migrates automatically on first load after updating, nothing is lost.

- **New data model per species (and per form)**: Owned (Normal), Owned (Shiny), Trade Available (Normal), Trade Available (Shiny) — all independent counts instead of one caught/shiny flag plus a single "extra" number.
- **Pokédex detail modal**: replaced the Caught/Shiny toggle switches with four quantity fields (Owned Normal/Shiny, Trade Available Normal/Shiny). Trade Available is automatically capped at whatever you own.
- **Quick-toggle buttons on dex cards**: kept as a fast single-copy shortcut (toggle 0/1), independent for normal and shiny now — marking shiny no longer requires or implies marking normal caught. Full quantity management lives in the modal.
- **Inventory tab**: shows owned count and trade-available count per entry; delete button now removes just that specific normal/shiny stack rather than the whole species.
- **Trade Builder Offering list**: now shows only entries with a Trade Available quantity set, and lets you pick exactly how many of that pool to include in a specific trade (previously all-or-nothing).
- **New "Confirm Trade Completed" button**: appears next to Download Image after generating a card. Only this action actually decrements Owned and Trade Available counts, by the quantities you included. Generating or downloading a card never touches your inventory on its own — nothing changes until you explicitly confirm a trade went through.
- Google Sheets export updated with separate Owned Qty and Trade Available Qty columns.

### v0.5.1 Beta
- Reverted the Base/Shiny/Alternate section split from v0.5 back to a single flat inventory list, per feedback. Delete (🗑) buttons per row are kept. Forms still show as text (no image) since that data source limitation hasn't changed.

### v0.5 Beta
- Inventory tab now split into three sections: **Base**, **Shiny**, and **Alternate** (forms), each with its own count badge.
- Alternate (forms) currently list as plain text (name only, shiny marked with a ★) rather than sprite images, until a more reliable form-image data source is in place.
- Added a delete (🗑) button to every inventory row, letting you un-mark a caught Pokémon, shiny, or form directly from the Inventory tab instead of only through the Pokédex detail modal.

### v4.0 Beta
- Added a search/filter bar to the Offering side of the Trade Builder tab, filtering your caught inventory by name as your collection grows.
- **Fixed alternate/regional forms not displaying properly**: PokeAPI's official-artwork field is frequently empty for regional/alternate form varieties (a known gap in their sprite data). Forms now fall back to the variety's regular sprite before falling back to base species artwork, so forms that previously showed broken or incorrect images should now display correctly. Local form cache was invalidated so this takes effect immediately rather than serving old cached data.
- **Fixed trade card downloads missing Pokémon artwork**: html2canvas was silently dropping cross-origin images (the sprite artwork) since neither the `<img>` tags nor the export call were configured for CORS. Added `crossorigin="anonymous"` to trade card images and enabled CORS mode in the export, so downloaded PNGs now actually include the Pokémon art.

### v0.3.2 Beta
- **Fixed a bug**: the account dropdown menu (Profile/Sign out under your name in the header) was getting clipped by the header panel's `overflow:hidden` (used to round off the diagonal stripe background pattern), so it appeared cut off instead of floating above the page. Fixed by giving the stripe pattern its own rounded corners instead of relying on clipping the whole header.

### v0.3.1 Beta
- Added "Export to Google Sheets" on the Inventory tab. Creates a new spreadsheet in your Drive with a row per caught Pokémon (including forms): sprite image (via Sheets' native `=IMAGE()` formula), Dex #, Name, Shiny, and Trade Qty columns, with column/row sizing set so images display at a reasonable size. Opens the new sheet in a new tab when done.
- This reuses the existing `drive.file` scope already granted, no new permission prompt required.

### v0.3 Beta
- **Fixed a bug**: typing in the Trade Builder search box lost cursor focus after every character, since each keystroke was re-rendering the whole tab (including the input itself). Fixed by only re-rendering the search results dropdown on input, leaving the input element untouched. Found and fixed the same underlying bug on the Pokédex tab's search box while in there, even though it wasn't reported, since it was the identical pattern.
- Redesigned the Trade Card background from a generic blue gradient to a Master Ball-inspired holo look: deep purple-to-black base, pink/magenta swirl accents, and a diagonal holo-foil sheen. Text and sparkle accent colors updated to match.

### v0.2.3 Beta
- Replaced the "Load More" pagination button with collapsible region sections in the Pokédex tab. Each region (Kanto through Paldea) is now its own accordion, collapsed by default, with a caught-count badge (e.g. "42/151 caught") and a ▸/▾ chevron. Click a region's header to expand or collapse it.
- Region filter (dropdown) and search still show a flat list, bypassing the accordion, since those already narrow things down.

### v0.2.2 Beta
- Moved Profile out of the main tab bar. It's now reached via a dropdown under your Google account name/photo in the header (marked with a ▾ caret), which also holds Sign out.

### v0.2.1 Beta
- Fixed Trainer Code field not applying the "1234 5678 9012" grouping format to entered text (was only shown in the placeholder). Now auto-formats live as you type.
- Added a copy-to-clipboard icon button next to the Trainer Code field on the Profile tab.

### v0.2 Beta (since initial v0.1 Beta release)

- Added region-based sorting/filtering to the Pokédex tab; corrected Galar/Hisui boundary to match Pokémon GO's own region grouping (confirmed against pokemongohub.net's Pokédex).
- Added Trainer Code field, originally on the Trade Builder tab.
- Added, then removed, a QR code encoding the Trainer Code on the trade card (kept as text-only per request).
- Moved Trainer Code to a new **Profile** tab; added a first-load popup prompting new sign-ins to set it.
- Added inline Caught/Shiny toggle buttons directly on each Pokédex grid card (previously required opening the detail modal for every change).
- Integrated the PogoDex Tracker logo as favicon, header badge, and sign-in screen icon; enlarged both placements per follow-up request.
- Switched typeface from Baloo 2 / Manrope / IBM Plex Mono to Roboto / Roboto Mono throughout.
- Updated color accent from blue to a phosphor-green "terminal" accent (trade card output kept blue to match the Pokémon GO in-app style).
- Rebuilt storage layer from Claude-artifact `window.storage` to Google Sign-In + Google Drive (`drive.file` scope) JSON file, since the app is now externally hosted.

