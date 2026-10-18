[README.md](https://github.com/user-attachments/files/33638238/README.md)
# Handoff: Headwind — Ride & Run Weather (Home)

## Overview
A single responsive home screen that answers one question: *should I go for a bike ride or run right now, and if not, when?* It shows a verdict (Go / OK / Skip) with a 0–100 conditions score for the selected city, current conditions, the best 2-hour window today, kit advice, and a 12-hour scored forecast strip.

Target repo: `asaplogic/for_testing` (branch `master`, currently empty). Commit the implementation there.

## About the Design Files
The files in `design/` are **design references built in HTML** — a working prototype showing intended look and behavior, not production code to ship. Recreate it in a proper app environment. The repo is empty, so pick the framework: suggested **Vite + React + TypeScript**, plain CSS (or CSS modules) with the tokens below as CSS variables. The prototype runs on a custom component runtime (`support.js`, `.dc.html`) — don't port that; read `Ride Weather.dc.html` for markup and the `class Component` logic.

To view the reference: serve the `design/` folder (`npx serve design`) and open `Ride Weather.dc.html`.

## Fidelity
**High-fidelity.** Final colors, type, spacing, rules and behavior. Recreate pixel-faithfully.

Design language ("Modernist"): flat, architectural, zero border radius everywhere, strong 2px rules, everything flush-left (including button labels), one red accent used sparingly, Archivo for all text.

## Screen: Home

### Layout
- Page bg `#f3f2f2`, text `#201e1d`. Content container `max-width: 1120px`, centered, horizontal padding `clamp(16px, 4vw, 40px)`.
- Vertical stack of 5 bands, separated by rules:
  1. Header — bottom border `2px solid #201e1d`
  2. City row — bottom border `2px solid` divider
  3. Verdict + conditions grid — bottom border `2px solid #201e1d`
  4. Best window + kit — bottom border `2px solid` divider
  5. Next 12 hours
- Divider color: `color-mix(in srgb, #201e1d 40%, transparent)`.

### 1. Header
- Flex, space-between, wrap, gap 16px, padding 20px 0.
- Left: wordmark "Headwind" (Archivo 800, 20px, letter-spacing -0.02em) + tagline "RIDE & RUN WEATHER" (12px, uppercase, letter-spacing 0.08em, `#605d5d`), baseline-aligned, gap 10px.
- Right: activity toggle "Ride" | "Run". Box with `2px solid #201e1d` border, no gap; second button has `border-left: 2px solid #201e1d`. Buttons: Archivo 800 13px, padding 8px 16px, text-align left. Active = bg `#201e1d`, text `#f3f2f2`; inactive = transparent bg, text `#201e1d`.

### 2. City row
- Flex, space-between, align flex-end, wrap, gap 16px, padding 28px 0 20px.
- Left column (gap 4px):
  - Date line, e.g. "SUNDAY, OCTOBER 18" (12px, uppercase, 0.08em, `#605d5d`). Use `toLocaleDateString({weekday:'long', month:'long', day:'numeric'})`.
  - City name H1: Archivo 800, `clamp(40px, 7vw, 72px)`, line-height 0.95, letter-spacing -0.03em. "Your location" when using geolocation.
- Right: city chips (Portland, Amsterdam, Boulder, London) + "My location" button, gap 8px, wrap.
  - Chip: Archivo 600 13px, padding 6px 10px, 1px border. Selected = bg/border `#201e1d`, text `#f3f2f2`. Unselected = transparent, border divider, text `#201e1d`; hover → border `#201e1d`.
  - "My location": same sizing, 1px `#201e1d` border, Lucide `navigation` icon 14px (stroke 2.2) + label, gap 6px; hover bg `#fff2ef`.

### 3. Verdict + conditions
CSS grid `repeat(auto-fit, minmax(min(100%, 420px), 1fr))` → two columns on desktop, stacked on mobile.

**Verdict panel** (left), padding `clamp(20px, 4vw, 36px)`, min-height 300px, flex column, space-between, gap 28px.
- Background depends on score:
  - ≥75 → accent `#ec3013`, text `#ffffff`
  - 50–74 → ink `#201e1d`, text `#f3f2f2`
  - <50 → surface `#eae9e9`, text `#201e1d`
- Top row: "RIGHT NOW · BIKE RIDE" / "· RUN" (12px, uppercase, 0.1em, 600) left; "{score}/100" (Archivo 800 15px) right.
- Verdict word: "Go." / "OK." / "Skip." Archivo 800, `clamp(64px, 11vw, 128px)`, line-height 0.85, letter-spacing -0.045em.
- Advice sentence: 17px, 600, line-height 1.4, max-width 34ch, `text-wrap: pretty`. Copy rules in Behavior.

**Conditions grid** (right): 2×2, `border-left: 2px solid #201e1d`. Each cell padding 20px 24px, flex column gap 6px. Inner rules: 2px divider on bottom of the top row and right of the left column.
- Cell label: 12px uppercase 0.08em `#605d5d`.
- Cell value: Archivo 800 48px, line-height 1, letter-spacing -0.03em.
- Cell subline: 13px `#605d5d`.
- Cells:
  1. **Temperature** — "14°"; sub "Feels 12° · Partly cloudy"
  2. **Wind** — speed number + compass: 36×36 circle, 2px `#201e1d` border, Lucide-style arrow-up 18px stroke 2.5, rotated `(windDirection + 180) % 360` deg so it points where the wind blows *to*. Sub "km/h from SW · gusts 22"
  3. **Rain next 3h** — max precip probability over next 3 hours, "20%"; sub "Humidity 64%"
  4. **Daylight left** — "5h 49m" or "Dark". Value font `clamp(32px, 4vw, 48px)`, `white-space: nowrap`. Sub "Sunset 6:24 PM"

### 4. Best window + kit
Grid `repeat(auto-fit, minmax(min(100%, 300px), 1fr))`.
- Left cell (padding 24px 24px 24px 0, right border 2px divider): label "BEST WINDOW TODAY" (12px uppercase 0.08em 600, `#ae1800`); value "2 PM – 4 PM" (Archivo 800 32px, lh 1.05, -0.02em); note "Score 86 · 16°, 9 km/h wind, 5% rain" (14px `#605d5d`).
- Right cell (padding 24px): label "WHAT TO WEAR" (`#605d5d`); value e.g. "Jersey + arm warmers, rain shell" (Archivo 800 22px, lh 1.2).

### 5. Next 12 hours
Padding 28px 0 48px, gap 16px.
- Heading row: H2 "Next 12 hours" (Archivo 800 24px) left; source note right (12px `#605d5d`): "Live forecast · Open-Meteo" or "Sample data — live forecast unavailable".
- Strip: `border-top: 2px solid #201e1d`, horizontally scrollable; inner grid `repeat(12, minmax(64px, 1fr))`, min-width 768px.
- Hour column: padding 12px 10px 14px, right border 1px divider, gap 10px.
  - Time label 12px 600 ("Now" for first, then "3 PM"...).
  - Score bar: 72px tall track bg `#eae9e9`, fill from bottom, height = `max(4, score)%`. Fill: ≥75 `#ec3013`, 50–74 `#201e1d`, <50 `#bab6b6`.
  - Temp: Archivo 800 18px.
  - "9 km/h" / "10% rain" 12px `#605d5d`, line-height 1.3.
  - The two columns of the best window: bg `#fff2ef`, time label `#ae1800`.
- Legend row (12px `#605d5d`, gap 16px): 12px squares in accent "Great", ink "Fine", `#bab6b6` "Skip", then "Bar height = conditions score".

## Interactions & Behavior
- **Activity toggle** Ride/Run: re-scores everything instantly (no refetch); changes advice and kit copy.
- **City chip**: select city → refetch forecast.
- **My location**: `navigator.geolocation.getCurrentPosition` → refetch with coords, city name "Your location", no chip selected. Silently ignore denial. (Future: reverse-geocode to a real city name.)
- **Loading**: verdict "…", advice "Loading forecast", values "–", surface bg.
- **Error/offline**: fall back to generated sample data and show the "Sample data" source note.
- **Units** (setting, default metric): imperial converts °C→°F (`c*9/5+32`) and km/h→mph (`×0.621`). Not on screen in the prototype; expose as a setting.
- Hover states: chips border darkens; "My location" tints `#fff2ef`. Focus: `outline: 2px solid #ec3013; outline-offset: 2px`. No default blue ring.
- No animations.

### Data
Open-Meteo, no API key:
```
https://api.open-meteo.com/v1/forecast?latitude={lat}&longitude={lon}
 &current=temperature_2m,apparent_temperature,wind_speed_10m,wind_gusts_10m,wind_direction_10m,weather_code,relative_humidity_2m
 &hourly=temperature_2m,precipitation_probability,wind_speed_10m,wind_gusts_10m,weather_code
 &daily=sunset&forecast_days=2&timezone=auto
```
City coords: Portland 45.52,-122.68 · Amsterdam 52.37,4.9 · Boulder 40.01,-105.27 · London 51.51,-0.13.

**Timezone (important):** with `timezone=auto`, hourly/sunset times come back as city-local strings with no offset. Parse as `Date.parse(t + 'Z') - utc_offset_seconds*1000`, and format display times with `timeZone: data.timezone`.

Start index = first hour whose time > now − 1h. Show 12 hours from there.

WMO code → label: 0 Clear; 1–2 Partly cloudy; 3 Overcast; ≤48 Fog; ≤57 Drizzle; ≤67 Rain; ≤77 Snow; ≤82 Showers; else Thunderstorm.

### Scoring (0–100, clamp & round)
```
ride: ideal temp 12–24°C, wind limit 14 km/h, wind penalty 2.5/kmh, rain factor 0.6
run:  ideal temp 6–18°C,  wind limit 22 km/h, wind penalty 1.5/kmh, rain factor 0.4
s = 100
if temp < lo: s -= (lo - temp) * 4
if temp > hi: s -= (temp - hi) * 5
if wind > limit: s -= (wind - limit) * penalty
if gust > limit + 20: s -= 10
s -= rainProbability * rainFactor
if weather_code >= 95: s -= 40
```
Current score uses current temp/wind/gust/code and the max rain probability over the next 3h.
Verdict: ≥75 "Go.", ≥50 "OK.", else "Skip."
Best window: the 2 consecutive hours in the 12-hour strip with the highest average score. Label "{start} – {start+2h}".

### Advice copy (dir = 8-point compass from wind_direction; windy = wind > limit)
- ≥75: "Prime conditions. " + (windy ? "Start into the {dir} wind, ride home with it." : "Light wind, dry roads.")
- 50–74: rain3h > 40 → "Rain likely within 3 hours — keep it short." / windy → "Breezy from the {dir}. Plan a sheltered route." / else "Rideable, not ideal. Dress for it."
- <50: rain3h > 50 → "Wet roads ahead. Wait for the window below." / else "Rough out there. Check the best window below."

### Kit copy (t = feels-like °C)
- Ride: <5 "Thermal jacket, full gloves, overshoes"; <12 "Long sleeves, vest, light gloves"; <20 "Jersey + arm warmers"; else "Short sleeves, extra water". Append ", rain shell" if rain3h > 40; ", lights" if daylight left < 2h.
- Run: <5 "Tights, thermal top, gloves"; <12 "Long sleeves, shorts or tights"; <20 "T-shirt and shorts"; else "Singlet, cap, hydrate". Append ", light shell" if rain3h > 40.

## State
- `activity: 'ride' | 'run'` (default ride)
- `cityIndex: number` (−1 = geolocation), `coords`
- `forecast` (API response or sample), `isLive: boolean`
- `units: 'metric' | 'imperial'`
- Derived: score, verdict, advice, kit, hours[], bestWindow.

## Design Tokens
Colors:
- bg `#f3f2f2` · surface `#eae9e9` · text `#201e1d` · accent `#ec3013`
- divider `color-mix(in srgb, #201e1d 40%, transparent)`
- neutral 400 `#bab6b6` · 700 `#605d5d`
- accent 100 `#fff2ef` · 600 `#dd2b0f` (pressed) · 700 `#ae1800` (accent text on light)

Type: Archivo (Google Fonts, weights 400/600/800) for everything. Headings 800, letter-spacing -0.015em to -0.045em at display sizes. Labels 12px uppercase 0.08em.

Spacing scale: 4, 8, 12, 16, 24, 32 px.
Radius: 0 everywhere. Shadows: none on this screen. Rules: 2px for section/cell dividers, 1px for hour columns and chips.

## Assets
- Font: Archivo (Google Fonts).
- Icons: Lucide (`navigation`, `arrow-up`). Use `lucide-react` or similar.
- No images.

## Files
- `design/Ride Weather.dc.html` — the prototype (template markup + `class Component` logic with scoring, data fetch, copy rules).
- `design/support.js` — prototype runtime only (don't port).
- `design/_ds/.../styles.css` — Modernist token sheet and component CSS (source of the tokens above).

## Suggested Claude Code prompt
> Read design_handoff_ride_weather/README.md. Scaffold a Vite + React + TS app in this repo, implement the Home screen pixel-faithfully, then commit and push to master.
