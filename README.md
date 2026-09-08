# US Airport Security Wait Timing — Dataset

The data behind [**The Worst Time to Fly, Airport by Airport**](https://www.boardflight.com/data),
a study of how much security wait times swing *within a single day* at US airports.

Published by [BoardFlight](https://www.boardflight.com), which computes when to arrive at the
airport for a specific flight. Free to use with attribution (CC BY 4.0).

---

## The finding

"Arrive two hours early" is one number for every airport, every day, every hour. It isn't true
anywhere.

At most of the airports here, **when you arrive within the day matters more than which day you
fly.** The gap between the quietest travel hour and the worst hour, at the same airport on an
ordinary weekday, runs as high as **23×**:

| Airport | Worst weekday hour | Quietest travel hour | Swing |
|---|---|---|---|
| MSP — Minneapolis–Saint Paul | 23 min at 8 AM | 1 min at 9 PM | **23×** |
| DCA — Reagan National | 20 min at 7 AM | 1 min at 10 PM | **20×** |
| SNA — John Wayne Orange County | 15 min at 6 AM | 0 min at 9 PM | **15×** |
| TPA — Tampa | 29 min at 6 AM | 2 min at 10 PM | **14.5×** |

Two more results that cut against the usual advice:

- **The worst hour is the morning, not the evening rush.** 6 AM is the single worst hour at more
  airports here than any other hour of the day.
- **The weekend is not reliably worse.** At 5 of the 12 airports the weekend peak is *lower* than
  the weekday peak — at FLL by 10 minutes, at DCA by 9.

## Files

| File | What it is |
|---|---|
| `data/airport-security-wait-findings.csv` | One row per airport. The table above, complete. |
| `data/airport-security-wait-findings.json` | The same rows, plus the sample dates. |

### Columns

| Column | Meaning |
|---|---|
| `iata` | IATA airport code |
| `airport_name`, `city` | |
| `weekday_worst_local_time` / `_wait_min` | Busiest 15-minute window on an ordinary weekday, local time, and the modelled wait then |
| `weekday_quietest_local_time` / `_wait_min` | Quietest window **during travel hours (5 AM–10 PM)**, and its wait. Overnight hours are quieter still, but nobody plans a 2 AM security arrival, so "best window" is restricted to hours a traveler could actually use |
| `weekend_worst_local_time` / `_wait_min` | The same peak, on a weekend day |
| `within_day_swing_ratio` | `weekday_worst_wait_min ÷ weekday_quietest_wait_min`, denominator floored at 1 |
| `weekend_minus_weekday_worst_min` | Positive = the weekend peak is worse than the weekday peak |

Waits are in minutes to clear the security checkpoint.

## Method, and what these numbers are not

**These are modelled medians, not measured observations.** They come from BoardFlight's
hour-by-hour security-demand model — the same model behind every airport page on the site — read
at the median (p50) for a traveler entering security at that time. They are not TSA-published wait
times, and they are not crowdsourced reports.

**Why only 12 airports.** BoardFlight profiles 33. Each airport's modelled all-day *shape* is
checked against an independent, external source — TSA's published hourly checkpoint throughput —
for whether the peak lands at the right time and the daily profile has the right form. Only the
12 airports that pass that check appear here. **The other 21 are excluded rather than published
with a caveat**: a shape we cannot independently verify does not get published as a finding.
That check is on *shape only* — TSA throughput is used to validate when the peak occurs, never to
set how many minutes the wait is.

**The sample days.** A single weekday (Thursday) and a single weekend day (Sunday). The underlying
model keys off day-of-week, not calendar date, so these are representative of an ordinary week —
they are not "the data as of" a particular date, and they carry no holiday or seasonal effect.

**What this is good for**, honestly: comparing hours *within* one airport, which is what the study
is about and what the validation covers. It is weaker for comparing absolute minutes *between*
airports, since only the shape has been externally checked.

## Reproducing and citing

Every figure here is also rendered in prose at <https://www.boardflight.com/data>, generated from
the same computation at build time. Nothing in this repository is transcribed by hand.

> BoardFlight, *The Worst Time to Fly, Airport by Airport* (2026).
> <https://www.boardflight.com/data>

Licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, including
commercially, with attribution.

## Corrections

If a number here looks wrong, open an issue. Getting told we are wrong about an airport is more
useful to us than not being told, and the study is regenerated rather than edited, so a fix lands
in the model rather than in the file.
