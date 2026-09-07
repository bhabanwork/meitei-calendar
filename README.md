# Meitei calendar data

Machine-readable Meitei (Manipuri) calendar data, served over HTTPS to the
**Meitei Calendar** Android app so that new years and corrected holiday lists
reach installed apps without an app update.

## Layout

- `public/index.json` — the manifest: every available year with the SHA-256 and
  byte size of its file. Clients fetch this first (it is small, and answers
  `304 Not Modified` when nothing changed) and download only the years whose
  digest they do not already have.
- `public/meitei_calendar_<year>.json` — one file per year. Each day carries the
  Gregorian date, weekday, Meitei month/day/year, tithi, `tatnaba`,
  `thasi_maikei`, and a `holiday` name where one applies.

Base URL:

```
https://raw.githubusercontent.com/bhabanwork/meitei-calendar/main/public/
```

## Updating

The files are generated, not hand-edited. In the app project:

```bash
python3 tools/scrape_meitei_calendar.py <year>
python3 tools/scrape_holidays.py <year> && python3 tools/merge_holidays.py <year>
python3 tools/publish_data.py --repo bhabanwork/meitei-calendar
```

then copy `public/` here and push. Digests change, and clients pick the new
files up within a day.

## Source

Calendar dates and holidays are derived from
[manipuricalendar.in](https://www.manipuricalendar.in), which publishes one year
at a time — hence the per-year files and the yearly refresh.
