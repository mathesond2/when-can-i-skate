# When Can I Skate This Week?

One static page that answers one question: **when can I skate this week?**

It pulls the next 7 days of hourly weather from [Open-Meteo](https://open-meteo.com/) (no API key) and marks each daylight hour green when it is free on your schedule, dry, comfortable, and storm-free.

## Use it

Open `index.html` in a browser, or visit the GitHub Pages site. It is set to Astoria Park, Queens. To change it, edit the `CONFIG` block at the top of the `<script>` (home lat/lon, free hours, temperature and wind limits) and reload.

## How an hour turns green

1. **Daylight:** between sunrise and sunset.
2. **Free:** inside that day's weekday or weekend window and not inside a `blocked` range.
3. **Dry:** no rain in the hour, and enough hours since the last rain hour (`dryHoursSunny` when radiation ≥ `sunnyRadiation`, otherwise `dryHoursCloudy`).
4. **Comfortable:** temperature between `minTempF` and `maxTempF`, and `max(wind, 0.7 × gust)` ≤ `maxWindMph`.
5. **No thunder:** weather code is not 95, 96 or 99 in this hour or the one before.

Green hours that are 55–82 °F with wind under 10 mph are **prime** (dark green), the rest are **ok** (light green). Hours outside your free time are dimmed, so you can see what you are missing.

Consecutive green hours become windows, clipped to your free time; windows shorter than `minWindowMinutes` are dropped.

Open-Meteo reports precipitation and radiation as totals for the hour *ending* at each timestamp, so the hour from 6 to 7 PM is read from the 7 PM value.

## Offline

The forecast is cached in `localStorage` for 20 minutes. If a fetch fails, the page shows the last cached forecast and says so.

## Publishing on GitHub Pages

Settings → Pages → Build and deployment → Source: **Deploy from a branch**, Branch: `main` / `(root)`.

## Next

Use it for two weeks and note every time the "dry" call was wrong. If the guesses are too crude, move to the full spec: a continuous wetness model and hourly scores first, then calendar sync and alerts.
