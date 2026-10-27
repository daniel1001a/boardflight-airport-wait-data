# US Airport Security Wait Timing — Dataset

The data behind [**The Worst Time to Fly, Airport by Airport**](https://www.boardflight.com/data),
a study of how much security wait times swing *within a single day* at US airports.

Published by [BoardFlight](https://www.boardflight.com), which computes when to arrive at the
airport for a specific flight. Free to use with attribution (CC BY 4.0).

**Scope.** This dataset covers 12 airports. The boardflight.com/data page shows the 8 that also
pass the schedule-derived check: AUS, BNA, BOS, DCA, JFK, SAN, SNA and TPA. FLL, IAD, MSP and PHL
passed only the historical-seed check; they stay in the data files here but have no hourly
numbers on the site. Unless a line says otherwise, the examples below use the 8 site-aligned
airports.

---

## The finding

"Arrive two hours early" is one number for every airport, every day, every hour. It isn't true
anywhere.

At most of the 8 site-aligned airports, **when you arrive within the day matters more than which day you
fly.** The gap between the quietest travel hour and the worst hour, at the same airport on an
ordinary weekday, runs as high as **20×**:

| Airport | Worst weekday hour | Quietest travel hour | Swing |
|---|---|---|---|
| DCA — Reagan National | 20 min at 7 AM | 1 min at 10 PM | **20×** |
| SNA — John Wayne Orange County | 15 min at 6 AM | 0 min at 9 PM | **15×** |
| TPA — Tampa | 29 min at 6 AM | 2 min at 10 PM | **14.5×** |
| BNA — Nashville | 10 min at 12 PM | 1 min at 9:45 PM | **10×** |

In the full 12-airport historical dataset the largest swing is MSP (23×), which is not shown on
the site.

Two more results that cut against the usual advice:

- **The worst hour is the morning, not the evening rush.** 6 AM is the weekday worst hour at 3 of
  the 8 site-aligned airports (JFK, SNA, TPA), more than any other hour.
- **The weekend is not reliably worse.** At 2 of the 8 site-aligned airports the weekend peak is
  *lower* than the weekday peak: at DCA by 9 minutes, at SAN by 8. Across the full 12-airport
  historical dataset it is 5 of 12 (adding FLL by 10, PHL by 3 and IAD by 1).

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
historical-seed hour-by-hour security-demand analysis, read at the median (p50) for a traveler entering security at that time. They are not TSA-published wait
times, and they are not crowdsourced reports. At airports that pass the schedule-derived check,
the airport pages on the site now use a separate schedule-derived curve, so their figures can
differ from the historical-seed figures in this file.

**Why only 12 airports.** BoardFlight has scored 33 airports against this check (the site now profiles 61; the other 28 have not been scored yet). Each airport's modelled all-day *shape* is
checked against an independent, external source — TSA's published hourly checkpoint throughput —
for whether the peak lands at the right time and the daily profile has the right form. Only the
12 airports that pass that check appear here. **The other 21 scored airports are excluded rather than published
with a caveat**: a shape we cannot independently verify does not get published as a finding.
That check is on *shape only* — TSA throughput is used to validate when the peak occurs, never to
set how many minutes the wait is.

**The sample days.** A single weekday (Thursday) and a single weekend day (Sunday). The underlying
model keys off day-of-week, not calendar date, so these are representative of an ordinary week —
they are not "the data as of" a particular date, and they carry no holiday or seasonal effect.

**What this is good for**, honestly: comparing hours *within* one airport, which is what the study
is about and what the validation covers. It is weaker for comparing absolute minutes *between*
airports, since only the shape has been externally checked.

## Site coverage

All 12 airports stay in the data files here. The BoardFlight site (www.boardflight.com) shows
hour-by-hour wait figures on its /data page for 8 of them: AUS, BNA, BOS, DCA, JFK, SAN, SNA and
TPA. The other 4 (FLL, IAD, MSP and PHL) are kept in the data files but show no hourly figures
anywhere on the site, including their own airport pages, because they have not passed the
schedule-based chart check.

## Reproducing and citing

The figures for the 8 site-aligned airports are also rendered in prose at <https://www.boardflight.com/data>, generated from
the same computation at build time. Nothing in this repository is transcribed by hand.

> BoardFlight, *The Worst Time to Fly, Airport by Airport* (2026).
> <https://www.boardflight.com/data>

Licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, including
commercially, with attribution.

## Corrections

If a number here looks wrong, open an issue. Getting told we are wrong about an airport is more
useful to us than not being told, and the study is regenerated rather than edited, so a fix lands
in the model rather than in the file.
