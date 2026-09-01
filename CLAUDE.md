# Ibiza 50th Birthday Trip Guide — Project Context

## Overview
A single-file HTML trip guide for a 50th birthday trip to Ibiza, September 2–9 2026. Five people: two 50-year-olds (Neil and wife), 21-year-old son, two friends. Staying at a villa near San Rafael (between Sant Antoni and Ibiza Town on the main road). Style: early-to-sunset rather than hardcore late night, mixing clubbing with walking, eating and exploring.

**Live site:** https://ibiza-50.uk  
**Repo:** https://github.com/NeilStoneman/ibiza-50  
**File:** `index.html` in repo root  
**Deployment:** Cloudflare Pages (project `ibiza-50`), auto-deploys on push to main  
**Google Maps API key:** AIzaSyBdLpBtavnzfsbNPIsBgXInzUMRnhhDi9Q (restrict to ibiza-50.uk in Google Cloud Console)

---

## Architecture

Single HTML file — vanilla JS, no frameworks, no build step.  
Tabler icons loaded from cdnjs.cloudflare.com.  
Nine tabs: **Map | Clubs | Eat | Walks | Beaches | Sights | Parking | Shops | Links**

### Card design
Each item is a card with:
- **Left column:** numbered circle (`.ex-c`) with a CSS chevron (`.ex-arr`) below it. Circle changes colour on hover/open. Chevron rotates 180° on open. Both stay pinned via `align-self:stretch` + `margin-top:auto`.
- **Centre:** card name, sub-line, pills row
- **Right:** nothing (pills are the last element in card-meta)

Cards expand/collapse via `toggleCard(id)`. The card body uses `height:0 → auto` with CSS transition.

### Deep links
Format: `https://ibiza-50.uk/#eat/e-canpilot`  
On page load, `readHash()` fires on `DOMContentLoaded`, `load` and `hashchange` events.  
It switches to the correct tab, then uses a retry loop (`tryOpen`) to find the element and expand it with `transition:none` + `height:auto` (no animation for deep links).

### Pills system
Each item has a `pills` array: `[{t:"Text", c:"type"}]`  
Types: `price` (amber) | `warn` (yellow) | `free` (green) | `genre` (purple) | `time` (green) | `info` (blue)  
Pills are sorted at render time: price → warn → free → genre → time → info  
Badge field (if present and not already in pills) is prepended as a pill automatically.

### Rich card content
Items support two content formats:
- **Simple:** `note:"string"` — rendered as a plain paragraph
- **Rich:** `desc:"string"` + `sections:[{title, items:[{key, val}]}]` — rendered with section headers and bullet points

The `linkify()` function auto-converts URLs and bare domains in section text to `<a>` links opening in new tabs.

### External link buttons
Cards show buttons at the bottom of the expanded section:
- **Google Maps** (`cl-maps`) — always shown if `maps` field present
- **Website** (`cl-web`) — shown if `website` field present
- **Book tickets** (`cl-tix`) — shown if `tickets` field present
- **Copy link for Wanderlog** (`cl-wl`) — always shown, copies deep link URL to clipboard

### Wanderlog integration
The guide is used alongside Wanderlog for mapping/logistics. Each card's "Copy link for Wanderlog" button copies the deep link URL. That URL is pasted into the Wanderlog place notes. Tapping from Wanderlog opens the guide directly to that card, expanded.

### JavaScript safety
- All string content must be escaped before inserting into JS: replace `"` with `'`, newlines with spaces
- The `e()` function handles this: `str.replace(/"/g, "'").replace(/\n/g, ' ')`
- Always run `node --check index.html` equivalent after edits to verify JS syntax
- `linkify()` has a null guard: `if (!t) return t || ''`

---

## Current State of Each Tab

### Clubs (10 entries — COMPLETE)
Ordered as specified. All entries have: rich desc + sections, accurate pricing, website/tickets links, pills.

Order:
1. `c-ushuaia` — Ushuaïa (Calvin Harris, Sep 4, already booked)
2. `c-akasha` — Akasha at Las Dalias
3. `c-kasbah` — Kasbah Ibiza Sunset
4. `c-cafemambo` — Café Mambo
5. `c-cafedelmar` — Café del Mar
6. `c-pikes` — Pikes Hotel
7. `c-pacha` — Pacha Ibiza
8. `c-covasanta` — Cova Santa
9. `c-benirras` — Cala Benirràs
10. `c-wom` — Word of Mouth (WOM)

Key notes:
- Sep 4 is Ushuaïa night — nothing else can be planned that evening
- Pikes: free entry ONLY if guestlist pre-registered at pikesibiza.com — not walk-in free
- Amnesia Glitterbox is Fridays only; only Friday in trip is Sep 4 (Ushuaïa clash) — removed from list
- Café Mambo and Café del Mar both have "How to visit without the big spend" sections covering: arrive before 6pm (no minimum spend), bar option, promenade as free listening spot
- Las Dalias night market moved to Eat tab (not clubs)
- Aluna Funky Room, Ground Zero, Club Chinois removed from list

### Eat (22 entries — IN PROGRESS)
First 9 positions locked. Working through in order adding research and websites.

Current order (first 10):
1. `e-canpilot` — Can Pilot (your local, San Rafael, closed Wed)
2. `e-lapaloma` — La Paloma (orange grove dining, Sant Llorenç)
3. `e-kumharas` — Kumharas (sunset bar, no min spend, ~€5 drinks) — website: kumharas.org/en/inicio-english/
4. `e-ritas` — Rita's Cantina (harbour, Sant Antoni, no reservations, Dutch-Indonesian menu) — website: ritasibiza.com
5. `e-barcosta` — Bar Costa (Santa Gertrudis, bocadillos, very cheap)
6. `e-aubergine` — Aubergine by Atzaró (farm-to-table, near Sant Miquel) — website: aubergineibiza.com
7. `e-alayoun` — Al Ayoun Sunset Ibiza (Arabic/Med fusion, no min spend, sunset strip) — website: alayounsunsetibiza.com
8. `e-camibalafi` — Restaurant Camí de Balàfia (farm BBQ, cash only, Sant Joan de Labritja)
9. `e-canmimosa` — Can Mimosa (farmhouse garden, Santa Gertrudis) — website: canmimosaibiza.com
10. **NEXT: Can Guimó Sant Joan de Labritja** — research needed, add at position 10

Remaining entries (positions 11+, not yet ordered/researched):
- `e-elclo` — El CLO (San Rafael village square)
- `e-labodega` — La Bodega Eivissa (Ibiza Town, foot of Dalt Vila)
- `e-escantonet` — Es Cantonet (Ibiza Town, very cheap)
- `e-elzaguan` — El Zaguán (Ibiza Town, Basque pintxos, open Mon Sep 7)
- `e-estapnou` — Es Tap Nou (Ibiza Town, breakfast/lunch, very cheap)
- `e-canterra` — Can Terra (Ibiza Town, pintxos under €2, open until 2am)
- `e-monkey` — Monkey (Sant Antoni, Mediterranean)
- `e-rinconpepe` — El Rincón de Pepe (Sant Antoni, since 1962)
- `e-bamboo` — Bamboo (Sant Antoni, tiki bar)
- `e-royalty` — Restaurant Royalty (Santa Eulalia main square, since 1933)
- `e-canaribes` — Ca na Ribes (Santa Eulalia, since 1926, best paella)
- `e-lasdalias` — Las Dalias night market (Sat/Sun/Mon/Tue 7-11:30pm, free entry)
- Various others (Es Pins, Can Cosmi, El Rincón de Pepe, etc.)

### Beaches (4 entries — COMPLETE, added 2026-08-30)
Curated, max-4-entry tab for quiet, non-family, snorkel-friendly beaches — deliberately not the full "every beach on the island" list that Sights' Beach category covers. Each entry uses the `BEACHES` array (`b-` id prefix) and shows on the overview Map tab as its own filter chip/colour (`#0891B2`, teal).

Entries: Punta Galera · Cala Xarraca · Cala d'en Serra · Es Portixol

Pills vocabulary specific to this tab: `Quiet` (free/green), `Snorkelling` (genre/purple), `Walk` (info/blue), `Sunset` (time/green) — reused existing pill types rather than adding new CSS classes. Not every entry gets all four; only what's true.

Cross-referencing: each Beach entry links out to its matching Walks entry (all four already existed as Walks before this tab was added) and, where genuinely nearby, an Eat entry. The reverse links were added too — the four Walks entries, plus the Sights entries for Punta Galera and Cala Xarraca, and the Al Ayoun / Aubergine by Atzaró Eat entries, all got a `slot-link` back to the relevant Beaches card. Follow this same pattern (`goTo('beaches','b-xxx')` via the existing `slot-link` convention) for any future cross-referencing.

### Walks (10 entries — basic content, not yet deep-dived)
All have distance, difficulty, best-time chips. Content needs more route detail and practical tips.

Entries: Punta Galera · Cala Bassa loop · Cala d'Hort clifftop · Sa Talaia · Port Sant Miquel circuit · Es Portixol · Cala Xarraca · Punta Moscarter · Cala d'en Serra · Ses Salines

### Shops (5 entries — basic content)
Lidl Sant Antoni · Hiper Centro · SPAR San Rafael · Próxim Sant Antoni · Decathlon Ibiza (Puig d'en Valls — sports/beach gear, incl. snorkelling kit)

### Sights (15 entries — basic content, category filter)
Dalt Vila · Es Vedrà · Ses Salines · Cala Benirràs · Cala Bassa · Punta Galera · Cala Xarraca · Santa Gertrudis · Sant Joan · Kumharas · Formentera day trip · Ibiza Town Marina · Cala Tarida · Las Dalias Hippy Market · Cala Comte (Platges de Comte, added 2026-09-01 — iconic but genuinely crowded west-coast beach/Sunset Ashram, deliberately kept out of the Beaches tab for that reason; cross-linked with the Cala Bassa walk it ends at)

### Links (3 entries — COMPLETE, added 2026-09-01)
A simple, deliberately map-free tab for reference content that isn't tied to a specific place. Unlike every other tab, entries here are NOT rendered via `renderCard` — no expand/collapse, no pills, no numbered circle. Just a static heading and either a plain list of links or a short info paragraph, rendered directly by `renderLinks()` from the `LINKS` array (`lk-` id prefix). Each entry is `{id, nm, links:[{label, url}]}` for a set of URLs, or `{id, nm, note:"..."}` for plain advice text — `renderLinks()` branches on which field is present. No `lat`/`lng`, so it's not in `MAP_DATA`/`OVERVIEW_CATS`/`overviewFilters` — never appears on the overview Map or gets an embedded map. Pane markup skips the usual `.tab-split`/`.tab-map-col` wrapper — just `.tab-list-col.links-list` (CSS: `max-width:700px`, plus `.links-card` for padding).

Entries: Driving & Parking Tips (links to Ibiza Spotlight's transport guide) · Walking Guides (links to GPSmyCity and Smart Guide self-guided Ibiza Town tours) · Withdrawing Cash (plain-text ATM advice — use major-bank ATMs, avoid standalone Euronet machines, always withdraw in EUR)

Add future entries the same way: `links` for a set of URLs, `note` for plain advice text — heading only, no descriptions or pills either way.

### Plan tab — removed (2026-08-21)
The Plan tab (day-by-day itinerary cards) was built, iterated on, and then dropped entirely at the user's request. There is no Plan tab, no `ITINS` data, and no itinerary-related rendering code in `index.html` — don't reintroduce it without being asked. The site is Map | Clubs | Eat | Walks | Sights | Parking | Shops.

---

## Key Design Decisions Made

- No map in the guide — Wanderlog handles all mapping
- Blue summary banners at top of tabs removed (clubs has none; eat/walks/sights also have none)
- Badge field removed from card title row — badge info folded into pills instead
- Pills sorted consistently: price first, then warn, free, genre, time, info
- Numbered circles (1–N per tab) on the left, CSS chevron below
- Only the circle changes colour on hover/open — arrow stays neutral grey
- Arrow is a CSS border chevron (not Unicode character) so it rotates cleanly
- `align-self:stretch` on `.ex` column + `margin-top:auto` on arrow aligns arrow with pills row
- Deep links use `DOMContentLoaded` + `load` + `hashchange` events plus a retry loop

---

## Workflow for Future Edits

1. Edit `index.html` directly in the repo
2. Verify JS syntax: `node --check index.html`
3. `git add index.html && git commit -m "description" && git push`
4. Cloudflare Pages auto-deploys in under a minute
5. Test deep links at `https://ibiza-50.uk/#tab/item-id`

---

## Trip Key Facts

- **Dates:** Sep 2–9, 2026
- **Villa:** Near San Rafael (Sant Rafel de sa Creu), between Sant Antoni and Ibiza Town
- **Group:** 5 people — two 50-year-olds, 21-year-old son, two friends
- **Style:** Early-to-sunset, mix of clubbing and exploring. Not hardcore late-night.
- **Sep 4 is booked:** Ushuaïa (Calvin Harris). Nothing else that evening.
- **Pacha nights available:** Thu Sep 3 (Purple Disco Machine), Sat Sep 5 (Flower Power — recommended), Sun Sep 6 (Solomun+1), Mon Sep 7 (Sonny Fodera)
- **Cova Santa available:** Sun Sep 6 (Pantheøn), possibly Tue Sep 8 (PIV — check)
- **Cala Benirràs drum circles:** Sunday sunset — conflicts with Solomun at Pacha

