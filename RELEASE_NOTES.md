# PogoDex Tracker — Release Notes

## v1.9.0 Beta

**New: Hero feature card on the Events tab**
- The most relevant event (the first current one, or the first upcoming one if nothing's running right now) gets a large featured treatment at the top: big header image, headline, an "Happening Now" / "Coming Up" eyebrow label, dates, and the same structured breakdown the event modal shows (Community Day spawns/bonuses, raid bosses/shinies, Research Breakthrough or Spotlight species, when that data exists for the event type).
- Ends with a clear "Read the full write-up on LeekDuck.com" button, same as the modal, since the actual article text isn't reproduced, only structured facts from the feed.
- The featured event is removed from the regular Current/Upcoming lists below it so it isn't shown twice.

**Noted, not built**: an AI-written original article for the hero (a real, legally sound option since original generated prose isn't the same as copying LeekDuck's text) was discussed and intentionally deferred. It needs a small backend (a serverless function holding the API key privately, generating and caching one article per event) rather than anything client-side, since a public static site can't safely hold an API key. Future project, not part of this release.

## v1.8.6 Beta

**Fixed: Stray horizontal lines between region cards**
- `.region-section` (the wrapper around each region card) still had a `border-bottom` left over from the old plain accordion-list design, before the gradient cards existed. With the new cards providing their own visual separation, that line just showed up as a stray sliver at the top edge of the next card. Removed.

## v1.8.5 Beta

**Added: Vertical divider between Wanted and Offering**
- A thin border now separates the two Trade Builder columns, since both can show a card for the same region with the same gradient (e.g. Kanto on both sides), making it hard to tell at a glance which list you're looking at. On narrow/mobile widths where the columns stack, this becomes a horizontal divider instead.

## v1.8.4 Beta

**Fixed: Wanted and Offering cards were different sizes**
- Wanted's region cards and Pokémon thumbnails were deliberately shrunk down in v1.8.3 ("compact" sizing) to fit the half-width column, but side by side with Offering's full-size cards it just looked inconsistent. Both panels now use the exact same card and region-card styling, no size difference.

**Fixed: Scrollbars inside each panel**
- Removed the fixed-height scroll containers on both Wanted and Offering. Regions now expand inline and the page itself scrolls, rather than a cramped scrollbox inside each column.

**Changed: "Generate Trade Card" moved to the top**
- The button now sits above both columns instead of below them, so it's visible without scrolling past the whole Wanted/Offering layout first.

## v1.8.3 Beta

**Reworked: Trade Builder "Wanted" panel — same browsing pattern as Offering, no more search**
- Replaces the search box and type-ahead results entirely with the same region-card component used for Offering, sized down to fit the half-width column.
- Browses the full National Dex (since you can want anything, not just what you own), grouped by region, same starter art and gradient theme as everywhere else.
- Tapping a Pokémon adds it straight to your Wanted list; tapping it again removes it, no separate remove step needed for the initial pick.
- The Wanted list below the browser (with the per-item shiny checkbox and remove button) is unchanged, still where you fine-tune shiny status for each wanted Pokémon before generating a card.

Both sides of the Trade Builder now work the same way: browse a region grid, tap to toggle, generate the card.

## v1.8.2 Beta

**Removed: Inventory tab**
- Caught status already shows directly on the Pokédex grid via the existing checkmark/sparkle toggles, so a separate list view was redundant. Un-marking something is done the same way it's marked, toggling it off on the Pokédex tab.
- The "Export to Google Sheets" button moved to the Pokédex tab's toolbar so that feature isn't lost.

**Reworked: Trade Builder Offering panel — no more searching, no more pre-marking**
- Replaces the previous version entirely. The Offering side is now a mirror of the Pokédex grid (same region cards, same card style) but filtered to only what you've actually caught, not the full National Dex and not a search box.
- Marking something tradeable is now a single click right on the card, one flag that's both the toggle and the selection, instead of a separate pre-marking step done elsewhere.
- Caught variants (Alolan, Galarian, costumes, etc.) show as their own individual cards, flat alongside the base species, not nested behind a "view variations" panel.
- "Generate Trade Card" reads whatever is currently marked tradeable at that moment. Confirming a completed trade still works exactly as before, clearing the tradeable flag on exactly the items that were on the generated card.
- Thumbnails still only switch to shiny art once the shiny toggle is actively selected for that card, same rule as before.

This completes the v1.8 batch: region-card redesign (v1.8.0), Trade Builder rebuild (v1.8.1, now superseded by this version), Inventory removal and the collection-based Offering panel (this release).

## v1.8.1 Beta

**New: Trade Builder "Offering" panel rebuilt to match the region cards**
- Replaces the old search-and-checkbox flat list entirely, no more searching for Pokémon you already know you have.
- Same region-card treatment as the Pokédex tab (gradient background, starter artwork, tap to expand), but scoped to only species/forms you've actually marked Trade Available, everything else stays hidden.
- One combined card per species, normal and shiny share a single thumbnail — the sprite only switches to the shiny art once the shiny toggle is selected for that offer, not just because a shiny is available.
- Region cards show a "X tradeable" count instead of a caught fraction, and only regions with at least one tradeable Pokémon appear at all.
- Selecting offers still feeds the same trade card generation and "Confirm Trade Completed" flow as before — only how you browse and select changed.

## v1.8.0 Beta

**New: Redesigned region cards on the Pokédex tab**
- Replaces the plain accordion toggle bar with a full-width gradient card per region, each with a distinct color theme and the region's three starter Pokémon shown as overlapping artwork.
- Shows a caught/total fraction for regions still in progress, or a "Complete! ⚫" badge with a small pokeball icon once every Pokémon in that region is caught.
- Tapping a card still expands/collapses it in place to reveal the region's Pokémon grid below, same interaction as before, just a much more visually distinct front door into each region.
- Starter artwork is pulled from fixed National Dex numbers (no new data source needed) — the same sprite pipeline already used everywhere else in the app.

Still queued for the v1.8.x batch: the region-exclusive Pokémon showcase, and the Trade Builder "Offering" panel rebuild (region-grouped thumbnails of owned/tradeable Pokémon, replacing the search-based flat list).

## v1.7.2 Beta

Builds on v1.7.1 (Events as the primary/default tab, first in the tab bar).

**New: Event detail modal**
- Tapping an event card now opens a modal instead of leaving the app.
- Built entirely from ScrapedDuck's structured `extraData`, never from LeekDuck's article text: Community Day shows featured spawns and bonus icons, raid weekends show bosses and shiny debuts, Research Breakthrough and Spotlight Hour show the featured species and shiny flag.
- Event types without structured data (most general announcements) show the image, category, name, and dates already on the card, with a clear note that more detail lives in the full write-up.
- Every modal ends with a "View full write-up on LeekDuck.com" link for the actual article, bonuses list, and research steps, since that content isn't pulled into the app.

## v1.7.1 Beta

**Changed: Events is now the primary tab**
- Moved Events to the first position in the tab bar and made it the tab the app opens to, instead of Pokédex.

## v1.7.0 Beta

**New: Events tab**
- Pulls the current and upcoming Pokémon GO event calendar from ScrapedDuck's events feed (which scrapes LeekDuck.com with their permission).
- Split into "Current Events" (running now, sorted by which ends soonest) and "Upcoming Events" (sorted by which starts soonest). Already-ended events are filtered out entirely.
- Each card shows the event artwork, its category (Community Day, Raid Hour, Spotlight Hour, etc.), name, and a formatted date range in your own local time. Tapping a card opens the full writeup on LeekDuck.com.
- Cached for an hour, refreshed automatically after that, per ScrapedDuck's request to wait at least 5 minutes between fetches.
- Non-fatal on failure, same pattern as Raids: if the feed can't be reached, only the Events tab shows a retry banner.

**Housekeeping**
- Footer credit updated to include ScrapedDuck alongside pokemon-go-api and LeekDuck.com.

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
