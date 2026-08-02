# When should I run?

A small planning tool that answers one question: when is the best time to run
today?

It takes the hourly weather forecast, the daylight hours, and your actual
schedule (work, commute, coffee, shower) and turns them into a scored list of
run windows that are realistic rather than theoretical. Then it tells you when
to get going.

**[Open it here](https://evelinehansen.github.io/when-should-i-run/)**

## What it does

- **Scores your run options** for any day in the 10-day forecast: before work,
  run to work, lunch, run from work, after work, and weekend slots.
- **Tells you when to leave.** Pick any option card to see its get-going time.
  The best one is tagged Recommended.
- **Checks your own preferred time.** Set one and see the conditions for that
  slot first, rather than only the one the tool likes best.
- **Packing lists** for run-to-work and run-from-work days.
- **Settings for everything**: location, paces, work hours, travel, coffee,
  shower, warm-up and cool-down times, which blocks each option includes, how
  the score is weighted, and your temperature thresholds.

## Running it

Open it at [the link above](https://evelinehansen.github.io/when-should-i-run/).
There is nothing to install, no build step, and no account. The whole tool is a
single `index.html` file, so that file also opens straight from disk if you have
cloned the repo.

On an iPhone, open it in Safari and use Share, then Add to Home Screen, which
gives it its own icon.

## Where your data lives

Your preferences are stored in your own browser, on your own device. There is no
server, no account, no cookies, and no analytics. Your daily inputs are never
stored at all.

The tool makes network requests in two places, both to
[Open-Meteo](https://open-meteo.com/): the hourly forecast, and the city search
in Settings. The first sends the coordinates of your chosen location, the second
sends your search text. Neither carries anything else about you.

Browser storage is readable by other pages served from the same address and by
software running on your device, and it is not encrypted, so do not keep
anything sensitive in here. The browser can also clear it on its own, in which
case you set your preferences again. There is nothing here worth backing up.

## How it's built

One self-contained `index.html` file. No frameworks, no build step, and no
packages pulled in from anywhere else, so that one file is the whole tool: what
you can read here is what runs in your browser. 

## Credits

Idea and direction by Eveline, coding by Claude. Built for my own practice,
learning and use.

Weather data by [Open-Meteo](https://open-meteo.com/) (CC BY 4.0), based on
national weather services such as MET Norway.

Personal project, shared as is.

