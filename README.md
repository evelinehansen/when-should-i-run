# When should I run?

A small planning tool that answers one question: **when is the best time to
run today?** It combines the hourly weather forecast, daylight hours and your
daily schedule (work, commute, coffee, shower…) into a scored list of
realistic run windows — and tells you when to get going.

**Live:** enable GitHub Pages on this repo (Settings → Pages → main / root)
and it will be served at `https://<username>.github.io/when-should-i-run/`.

## Features

- Scored run options for any day in the 10-day forecast: before work,
  run to work, lunch, run from work, after work, and weekend slots.
- Pick any option card to see its "get going" time; the best one is tagged
  ✦ Recommended.
- Optional preferred start time — see the conditions for *your* time first.
- Packing lists for run-to-work and run-from-work days.
- Fully customizable in ⚙ Settings: location (any city), paces, work hours,
  travel/coffee/shower/warm-up/cool-down times, which planning blocks each
  option includes, score weighting, and temperature thresholds.
- Mobile friendly.

## Privacy

One self-contained `index.html`. No analytics, no cookies, no accounts.
The only network requests go to [Open-Meteo](https://open-meteo.com/) for
the forecast and city search. Preferences are saved in your browser's
localStorage only; daily inputs are never stored.

Weather data by Open-Meteo (CC BY 4.0), based on national weather services
such as MET Norway.
