# PogoDex Tracker — Release Notes

## v1.6.2 Beta

**Fixed: Raid Card was too wide**
- The card reused the Trade Card's 900px width, which made sense there since it shows a whole grid of Pokémon, but the Raid Card only ever shows one, leaving a lot of empty space on either side. Narrowed to 460px with proportionally smaller title text.

## v1.6.1 Beta

**Fixed: Raid Card search only accepting one letter at a time**
- The search input was being rebuilt from scratch on every keystroke, which meant it lost focus after each character. Fixed by only re-rendering the results list below the input, never the input itself, the same pattern already used by the Trade Builder's search box.

**Changed: Raid Card builder now searches every Pokémon, not just the current live raid rotation**
- The live raid feed only reflects one global snapshot and doesn't cover regional or event-specific bosses (for example, Armored Mewtwo appearing in Tokyo right now wouldn't be in it).
- Search now covers the full species list, same as the Pokédex and Trade Builder.
- Selecting a Pokémon that matches a boss in the current live feed auto-fills tier, CP range, weather-boosted CP range, Shiny Available, and boost weather.
- Selecting a Pokémon with no live match leaves those fields blank and editable, tier dropdown, CP range fields, a Shiny Available checkbox, and weather chips, so regional or event bosses can still get a card.
- All fields stay editable either way, so auto-filled data can be corrected if it's stale.

## v1.6.0 Beta

**New: Raids tab**
- Pulls current raid rotation data from pokemon-go-api (sourced from LeekDuck.com), grouped by tier: 1-Star, 3-Star, 5-Star, Mega, Legendary Mega, Ultra Beast, EX, and Shadow 1/3/5.
- Each boss card shows artwork, normal CP range, a "Shiny Available" badge (data-driven — reflects whether the boss can be shiny at all, not a personal catch record), and a weather-boost badge naming which weather type(s) boost it.
- Refreshes every 6 hours via a local cache so the tab loads instantly on repeat visits without hammering the data source.
- Non-fatal on failure — if the raid feed can't be reached, only the Raids tab shows a retry banner; the rest of the app keeps working.

**New: Raid Card builder**
- Search and select any current raid boss, then generate a shareable card with its artwork, tier, CP range, Shiny Available badge (when applicable), and your Trainer Code from Profile.
- Weather Boosted toggle swaps the displayed CP range to the boosted range and labels which weather is boosting it — pulled from the same raid data already loaded for the tab, no extra fetch.
- Downloads as a PNG via the same method as the existing Trade Card.

**Housekeeping**
- Added a footer disclaimer: unofficial fan project, not affiliated with Nintendo/Niantic/The Pokémon Company, with credit to pokemon-go-api and LeekDuck.com for raid data.

---

## v1.4.3 Beta and earlier
Prior history not tracked in this file. Picking up the changelog starting here.
