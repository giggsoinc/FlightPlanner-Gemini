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

## User journeys: three sample chat sessions

> ⚠️ **Everything below is fictional.** Names, cities, dates and fares are invented, illustrative sample data. Real runs use live tool results.

### Journey 1: Family vacation in Florida

**You (Priya):** Family vacation in Florida for the holidays. Four of us.

**FlightPlanner:** What are you trying to do: cheapest flights, a specific city, or best value?

**You:** Best value, kids are 6 and 9, so no painful connections.

**FlightPlanner:** Home airport, travel dates, and total flight budget?

**You:** Pittsburgh. Dec 19 to Dec 27, 2026. Under $2,400 total.

**FlightPlanner:** Main Cabin okay? Basic Economy is excluded by default.

**You:** Main Cabin is fine.

**FlightPlanner:**
> **Goal locked:** Best-value round trip for 4 travelers, Pittsburgh to Florida, Dec 19 to Dec 27, 2026, Main Cabin, nonstop preferred, under $2,400 total. Proceeding on that.

| Option & Locked Date Pair | Route | Stops & Duration | All-In Cost (4 travelers) | +/- 3 Day Range | Notes |
|---|---|---|---|---|---|
| **1.** Dec 19 to Dec 27 | PIT to MCO (Orlando) | Nonstop · 2h 25m | $1,860 (sample) | $1,640 to $2,310 | Confirmed |
| **2.** Dec 19 to Dec 27 | PIT to TPA (Tampa) | Nonstop · 2h 30m | $2,040 (sample) | $1,880 to $2,420 | Confirmed |
| **3.** Dec 19 to Dec 27 | PIT to FLL (Fort Lauderdale) | 1 stop · 4h 10m | $1,790 (sample) | $1,610 to $2,250 | Likely |

- Basic Economy fares exist for less but were excluded.
- Could be wrong if holiday fares jump before you book.

---

### Journey 2: Friends reunion from five cities

**You (Jordan):** Reunion for five college friends. We're all in different cities.

**FlightPlanner:** Which cities, and should everyone fly identical dates?

**You:** Chicago, Austin, Seattle, Atlanta, Boston. Yes, same dates for all. Feb 12 to Feb 15, 2027.

**FlightPlanner:** Budget per person, and any city you'd rather avoid?

**You:** $400 each, no preference.

**FlightPlanner:**
> **Goal locked:** Find the cheapest synchronized meetup city for 5 travelers from ORD, AUS, SEA, ATL, BOS, Feb 12 to Feb 15, 2027, Main Cabin, under $400 each. Proceeding on that.

| Option & Locked Date Pair | Route | Stops & Duration | All-In Cost (5 travelers) | +/- 3 Day Range | Notes |
|---|---|---|---|---|---|
| **1. Nashville** · Feb 12 to Feb 15 | 5 origins to BNA | 4 nonstop, 1 with 1 stop | $1,720 (sample) | $1,540 to $2,010 | Confirmed |
| **2. Denver** · Feb 12 to Feb 15 | 5 origins to DEN | All nonstop | $1,905 (sample) | $1,720 to $2,200 | Confirmed |
| **3. Kansas City** · Feb 12 to Feb 15 | 5 origins to MCI | 3 nonstop, 2 with 1 stop | $1,980 (sample) | $1,800 to $2,260 | Likely |

- Everyone stays under $400 in option 1 (highest single fare: $398, sample).
- Could be wrong if the Seattle fare rises; it is the tightest one.

---

### Journey 3: International trip to London

**You (Alex):** Business trip to London. Business class.

**FlightPlanner:** Origin and dates?

**You:** Raleigh. Out Mar 8, back Mar 15, 2027.

**FlightPlanner:** The long-haul leg is likely from New York. Can the short Raleigh to New York positioning flight be Economy?

**You:** Yes, Economy for that part.

**FlightPlanner:** Budget, and are separate tickets okay with a 4 to 5 hour buffer?

**You:** Under $4,500 total. Separate tickets are fine.

**FlightPlanner:**
> **Goal locked:** Raleigh to London round trip, Mar 8 to Mar 15, 2027, Business on the long-haul leg, Economy for the positioning leg, under $4,500 total. Proceeding on that.

| Option & Locked Date Pair | Route | Stops & Duration | All-In Cost | +/- 3 Day Range | Notes |
|---|---|---|---|---|---|
| **1.** Mar 8 to Mar 15 | RDU to JFK (Economy) + JFK to LHR (Business) | 1 positioning + nonstop · 8h 20m | $3,610 (sample) | $3,380 to $4,090 | Confirmed |
| **2.** Mar 8 to Mar 15 | RDU to EWR (Economy) + EWR to LHR (Business) | 1 positioning + nonstop · 8h 35m | $3,740 (sample) | $3,450 to $4,200 | Confirmed |
| **3.** Mar 8 to Mar 15 | RDU to LHR (Business) | 1 stop · 11h 40m | $4,360 (sample) | $4,010 to $4,880 | Likely |

- Separate tickets carry no protection if the positioning flight is delayed.
- Could be wrong if the 4 to 5 hour buffer is cut by a schedule change.

*(Sightseeing and day-by-day plans stay out of scope by design.)*

---

## Install

Works in the Gemini **browser or app**. No CLI needed.

- [Via Gems](docs/install-via-gems.md): side panel, Gems, paste the instructions.
- [Via Skill](docs/install-via-skill.md): side panel, Customize, Skill, upload `SKILL.md`.

> Without live flight search access the skill will say it cannot confirm prices. That is intended.

---

## Repository layout

```
FlightPlanner-Gemini/
├── docs/                              # Install guides (Gems, Skill)
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
