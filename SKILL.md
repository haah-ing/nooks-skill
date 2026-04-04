---
name: nooks
description: Personal place intelligence — save and find cafes, coworking spaces, libraries, restaurants, and other spots worth revisiting. Stores one markdown file per place in nooks/<city>/. Use when saving a new place, looking up a spot, or asking "where should I work/eat/meet in [city]?".
metadata: { "openclaw": { "emoji": "📍", "os": ["linux", "darwin", "win32"] } }
---

# 📍 Nooks — local place intelligence

## Data

Files live in `nooks/<city>/`. City folders use short lowercase slugs: `hk`, `sf`, `sg`, `london`, `bkk`.
Create on first use: `mkdir -p nooks/<city>/`.

```
nooks/
├── hk/
│   ├── blue-bottle-central.md
│   └── amber-lan-kwai-fong.md
└── sf/
    └── sightglass-soma.md
```

File names: `<place-slug>.md`. Include neighborhood when the same place has multiple locations: `blue-bottle-central.md` vs `blue-bottle-admiralty.md`.

---

## Place File

```markdown
# Place Name

- **Type:** cafe / coworking / library / park / restaurant / bar / museum / etc.
- **Area:** station/neighborhood
- **Maps:** https://maps.app.goo.gl/...
- **Price:** $ / $$ / $$$ / $$$$
- **Vibe:** quiet / moderate / buzzy
- **Good for:** focus, calls, client meetings, leisure
- **Features:** #wifi #charging #outdoor #healthy-food #coffee #24h #ac #3dprinter

## Notes

4 Apr 2026: upper floor is quieter, fills up fast after 10am
```

**Field guidance:**

- **Maps** — use the share link from Google Maps mobile (`maps.app.goo.gl/...`). Exact, compact, opens to the pin.
- **Good for** — your standing assessment of what this place suits. Comma-separated. Update if your opinion changes.
- **Notes** — dated personal log. Observations, tips, surprises, things that shift over time. Format: `4 Apr 2026: ...`
- **Features** — use standard tags so grep works: `#wifi` `#charging` `#outdoor` `#food` `#coffee` `#quiet` `#24h` `#reservations` `#alcohol` `#coworking`

**Good for vs Notes — the distinction:** Good for is the verdict; Notes is the evidence. One is a label, the other is a log.

---

## Saving a Place

1. **Search the web** (name + city/area) — pre-fill Type, Maps link, Price, Vibe, and Features from what's publicly known.
2. **Show what you found**: "Found Sightglass — specialty coffee roaster in SoMa SF, $$. Wifi confirmed, communal tables, no time limit."
3. **Ask as a group** (skip anything already answered):
   - What's it good for? — focus, calls, meetings, catch-up, date, leisure?
   - Vibe? — quiet / moderate / buzzy (if unclear from search)
   - Features to correct? — confirm or fix what you found
   - Any notes? — first impressions, tips, things to remember

---

## Finding Places

Use `grep` with expanded terms. Search city folder or all of `nooks/` cross-city.

```bash
# Quiet wifi spots in HK
grep -rl "#quiet" nooks/hk/ | xargs grep -l "#wifi"

# Focus-friendly places in SF
grep -ril "focus\|deep.work\|laptop\|coworking" nooks/sf/

# All cafes across cities
grep -ril "Type:.*cafe" nooks/

# Charging spots anywhere
grep -rl "#charging" nooks/
```

**Always read the full file after grepping** — the matched line is a signal, not the full picture.

**Keyword expansion — always broaden the query:**

- "work / focus" → `focus\|deep.work\|laptop\|coworking\|quiet`
- "meeting / call" → `meeting\|client\|calls\|zoom`
- "coffee" → `cafe\|coffee\|espresso\|specialty`
- "chill / relax" → `leisure\|chill\|relax\|casual\|catch.up`

If local results are thin and Haah skill is installed, suggest dispatching to a circle.

---

## Core Behavior

- User mentions a place → check if a file exists, offer to create or update
- User asks "where can I work/eat in [city]?" → search `nooks/<city>/` first
- User shares a Maps link → offer to save the place
- User mentions visiting somewhere in passing → ask if worth saving
- User adds an observation ("terrible wifi there") → append a dated note to the file

**Examples:**
- "Had a great session at Sightglass" → ask if anything worth logging, update if so
- "We should go back to Amber" → check if saved, offer to create if not
- "That place had no charging" → update `#features` or add a note

Save places as they come up naturally. Enrich over time. Don't wait for a data entry session.

---

## Every Morning

Check a random nook file. Surface something worth knowing:

- "You haven't been to Sightglass in a while — still your go-to for focus in SF?"
- "Common Ground has no Maps link yet — worth adding?"
- "Amber only has one note — anything new from your last visit?"

If nothing worth mentioning, skip.

---

## What NOT to Suggest

- Syncing with Google Maps saved places — different purpose, keep separate
- Crowd-sourced ratings or reviews — Nooks is personal signal, not Yelp
- Opening hours in the file — always goes stale, check Google Maps
- Automated "you're near X" alerts — too intrusive
