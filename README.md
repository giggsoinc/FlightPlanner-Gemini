# ✈️ FlightPlanner-Gemini

An open-source **flight-planning skill for Gemini**. It interviews you first, locks the goal, then searches live flight data and returns exactly **three options in one table**, with ±3-day price scenarios.

It handles single-origin trips, multi-city itineraries, and **group meetups from several origins** (identical dates for everyone), all under a budget.

> Licensed under [Apache 2.0](LICENSE). © 2026 Ravi Venugopal, Giggsoinc.

---

## Why a skill, not just a prompt?

Plain chat tends to search too early, invent prices, and bury you in paragraphs. This skill enforces discipline:

| Rule | Effect |
|---|---|
| Interview first, one question at a time | No searching until the goal is clear |
| "Goal locked" statement | Every later step must serve that goal |
| Live data only, tagged **Confirmed / Likely / Assuming** | No made-up fares |
| Exactly top 3 options | No overwhelming result dumps |
| ±3-day scenarios | You see what date flexibility is worth |
| Max 2 stops (3 needs your approval) | No painful itineraries by default |
| Flights and logistics only | No sightseeing or day-by-day drift |

---

## How it works

```
Interview ──▶ Lock goal ──▶ Search (live) ──▶ Top-3 table ──▶ "What could make #1 wrong?"
   ▲              │
   └── next question depends on your last answer
```

1. **Interview.** Asks what you're trying to do, then branches:
   - *Destination unknown* → classifies the trip (personal / business / combo), asks length, budget, vibe, date flexibility, then uses **Explore**.
   - *Destination known* → confirms origins, dates, cabin, budget, stops, and whether dates must be synchronized.
2. **Lock.** Restates the goal in one sentence.
3. **Search.** Uses live flight tools. Main Cabin by default (Basic Economy excluded, but mentioned).
4. **Present.** One table: `Option & Locked Date Pair | Route | Stops & Duration | All-In Cost | +/- 3 Day Range | Notes`.

---

## User journey: a chat session

> ⚠️ **All fares, times and routes below are illustrative sample data.** Real runs use live tool results.

**You:** Plan a trip for me and my sister. I'm in Detroit, she's in LA. We want to meet somewhere.

**FlightPlanner:** What are you trying to do with this trip? For example: cheapest meetup city, somewhere warm, a specific destination?

**You:** Cheapest place to meet, a long weekend, doesn't need to be warm.

**FlightPlanner:** Is this personal, business, or a mix?

**You:** Personal.

**FlightPlanner:** What's your total budget per person, flights only?

**You:** Under $450 each.

**FlightPlanner:** Are your dates fixed, or flexible?

**You:** Roughly mid-December, flexible by a few days.

**FlightPlanner:** Should both of you fly on identical departure and return dates, and is Main Cabin fine?

**You:** Yes to both.

**FlightPlanner:**
> **Goal locked:** Find the cheapest synchronized round-trip meetup city for DTW + LAX travelers, mid-December long weekend, Main Cabin, under $450 each. Proceeding on that.

*(The skill now searches live data.)*

| Option & Locked Date Pair | Route | Stops & Duration | All-In Cost | +/- 3 Day Range | Notes |
|---|---|---|---|---|---|
| **1. Denver** · Dec 12 to Dec 15 | DTW to DEN / LAX to DEN | Nonstop · 2h 50m / 2h 10m | $318 + $204 (sample) | $270 to $395 | Confirmed |
| **2. Dallas** · Dec 12 to Dec 15 | DTW to DFW / LAX to DFW | Nonstop · 2h 45m / 2h 55m | $361 + $239 (sample) | $310 to $430 | Confirmed |
| **3. Las Vegas** · Dec 12 to Dec 15 | DTW to LAS / LAX to LAS | Nonstop / Nonstop | $389 + $98 (sample) | $330 to $460 | Confirmed |

- Denver is cheapest overall; shifting 2 days earlier saves about $48 (sample).
- Basic Economy fares exist for less but were excluded.
- Could be wrong if a fare sells out or either of you needs checked bags.

**You:** Go with Denver. Can you check the hotel?

**FlightPlanner:** Destination confirmed. Hotels are now in scope. What nights and budget?

*(Sightseeing and day-by-day plans stay out of scope by design.)*

---

## Install

### Gemini CLI
Place this repo (or just `GEMINI.md` and `skills/`) in your project. Gemini CLI loads `GEMINI.md`, which points to the skill.

```bash
git clone https://github.com/giggsoinc/FlightPlanner-Gemini.git
cd FlightPlanner-Gemini
gemini
```

### Gemini app (Gem) or Google AI Studio
1. Create a new **Gem**, or open AI Studio and set **System instructions**.
2. Paste the contents of [`skills/unified-flight-planner/SKILL.md`](skills/unified-flight-planner/SKILL.md) (without the front matter).
3. Enable flight/hotel search tools or extensions so results are live.

> Without a live flight search tool the skill will say it cannot confirm prices. That is intended.

---

## Repository layout

```
FlightPlanner-Gemini/
├── GEMINI.md                          # Entry point for Gemini CLI
├── skills/unified-flight-planner/
│   └── SKILL.md                       # The skill (rules + workflow)
├── CONTRIBUTING.md
├── LICENSE                            # Apache 2.0
└── NOTICE
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Issues and PRs welcome.

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
