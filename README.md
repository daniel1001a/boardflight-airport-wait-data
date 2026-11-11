# US Airport Security Wait Timing — Dataset

The data behind [**The Worst Time to Fly, Airport by Airport**](https://www.boardflight.com/data),
a study of how much security wait times swing *within a single day* at US airports.

Published by [BoardFlight](https://www.boardflight.com), which computes when to arrive at the
airport for a specific flight. Free to use with attribution (CC BY 4.0).

**Scope.** This dataset covers 11 airports. The boardflight.com/data page shows the 5 that also
pass the schedule-derived check: BOS, JFK, SAN, SNA and TPA. BNA, DCA, FLL, IAD, MSP and PHL
passed only the historical-seed check; they stay in the data files here but have no hourly
numbers on the site. Unless a line says otherwise, the examples below use the 5 site-aligned
airports.

---

## The finding

"Arrive two hours early" is one number for every airport, every day, every hour. It isn't true
anywhere.

At most of the 5 site-aligned airports, **when you arrive within the day matters more than which day you
fly.** The gap between the quietest travel hour and the worst hour, at the same airport on an
ordinary weekday, runs as high as **15×**:

| Airport | Worst weekday hour | Quietest travel hour | Swing |
|---|---|---|---|
| SNA — John Wayne Orange County | 15 min at 6 AM | 0 min at 9 PM | **15×** |
| TPA — Tampa | 29 min at 6 AM | 2 min at 10 PM | **14.5×** |
| SAN — San Diego | 39 min at 9 AM | 5 min at 10 PM | **7.8×** |
| BOS — Boston Logan | 15 min at 6:45 PM | 2 min at 10 PM | **7.5×** |

In the full 11-airport historical dataset the largest swing is MSP (23×), followed by DCA (20×);
neither is shown on the site.

Two more results that cut against the usual advice:

- **The worst hour is the morning, not the evening rush.** 6 AM is the weekday worst hour at 3 of
  the 5 site-aligned airports (JFK, SNA, TPA), more than any other hour.
- **The weekend is not reliably worse.** At 1 of the 5 site-aligned airports the weekend peak is
  *lower* than the weekday peak: at SAN by 8 minutes. Across the full 11-airport historical
  dataset it is 5 of 11 (adding DCA by 9, FLL by 10, PHL by 3 and IAD by 1).

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

**Why only 11 airports.** BoardFlight has scored 33 airports against this check on the historical-seed curves used in this dataset (the site now profiles 116; the other 83 have not been scored on that basis; the site's own schedule-derived gate is a separate run with its own counts, shown on boardflight.com/methodology). Each airport's modelled all-day *shape* is
checked against an independent, external source — TSA's published hourly checkpoint throughput —
for whether the peak lands at the right time and the daily profile has the right form. Only the
11 airports that pass that check appear here. **The other 22 scored airports are excluded rather than published
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

All 11 airports stay in the data files here. The BoardFlight site (www.boardflight.com) shows
hour-by-hour wait figures on its /data page for 5 of them: BOS, JFK, SAN, SNA and TPA. The other 6
(BNA, DCA, FLL, IAD, MSP and PHL) are kept in the data files but show no hourly figures
anywhere on the site, including their own airport pages, because they have not passed the
schedule-based chart check.

## Reproducing and citing

The figures for the 5 site-aligned airports are also rendered in prose at <https://www.boardflight.com/data>, generated from
the same computation at build time. Nothing in this repository is transcribed by hand.

> BoardFlight, *The Worst Time to Fly, Airport by Airport* (2026).
> <https://www.boardflight.com/data>

Licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, including
commercially, with attribution.

## Corrections

If a number here looks wrong, open an issue. Getting told we are wrong about an airport is more
useful to us than not being told, and the study is regenerated rather than edited, so a fix lands
in the model rather than in the file.
