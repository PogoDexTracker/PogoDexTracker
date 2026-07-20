# POGO Dex Tracker — v0.2 Beta

Initial release: July 20, 2026
Last updated: July 20, 2026 (v0.2 — see changelog below)

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

