# POGO Dex Tracker — v0.1 Beta

Release date: July 20, 2026

## What's in this build

**Sign-in & storage**
- Google Sign-In gates access to the app.
- Your dex data (caught, shiny, forms, trade quantities, trainer code) saves to a single JSON file in your own Google Drive using the `drive.file` scope. The app never sees or touches anything else in your Drive.
- Writes are debounced (~1.2s after your last change) so it's not hammering the Drive API on every click.

**Pokédex tab**
- Full National Dex coverage, #1–1025.
- Search by name or dex number.
- Region filter and auto-grouping (Kanto through Paldea), matching how Pokémon GO itself groups regions — Hisuian species are folded into Galar since GO never released Hisui as its own region.
- Per-species: mark caught, mark shiny, set a duplicate count for trading.
- Regional/alternate forms pulled in on demand and cached locally, each with its own caught/shiny toggle.

**Inventory tab**
- Flat, filterable view of everything you've caught, including forms, with shiny and trade-duplicate tags.

**Trade Builder tab**
- "Wanted" list: search and add anything, with a shiny toggle.
- "Offering" list: pulled from your caught inventory, check off what you're willing to trade.
- Generate Trade Card: renders a shareable card in the classic blue trade-list style, includes your Trainer Code if you've set one, and downloads as a PNG.

## Known limitations in this beta

- **Forms data source**: alternate/regional forms currently come from PokeAPI, which reflects the mainline games. Some forms shown may not actually exist in Pokémon GO yet (or vice versa — GO-specific things like Dynamax/Gigantamax/Shadow aren't reflected). A GO-accurate data source has been identified for a future build.
- **Trade Builder selections don't persist** — your Wanted/Offering picks reset if you reload mid-session. Everything else does persist.
- **No offline support** — needs a connection for sign-in, PokeAPI lookups, and Drive sync.
- Requires a one-time Google Cloud OAuth Client ID setup before first use (see setup notes provided separately).

## Planned for future betas
- GO-accurate form/shiny/availability data
- Persisted Trade Builder state
- Possible CP/IV or move tracking, if wanted
