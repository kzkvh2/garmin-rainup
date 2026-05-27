# RainUp — User Guide

A Connect IQ data field for Garmin Edge cycling computers that tells
you what the rain is doing **right now**, what's **coming up along the
direction you're heading**, and **when it'll clear** — all in one
glance from your handlebars.

---

## Why RainUp is different

Most weather fields show a single forecast snapshot at your *current*
location. RainUp is built differently:

- **Direction-aware, not just location-aware.** RainUp queries the
  weather at the actual positions you'll be in 30 and 60 minutes
  from now (dead-reckoned from your heading and pace). The answer
  accounts for where you'll *be*, not where you're standing now.
- **One reading rule.** Top line is now. Lines below are what's
  coming. No exceptions, no jargon to learn.
- **Honest signal.** A small icon on Row 3 always tells you whether
  the forecast you're reading is "along your path" (▲) or "at your
  spot" (⊙). The icon flips instantly when your motion changes, and
  the underlying data resamples within about a minute — so a stop
  never leaves stale path-ahead text on the screen.

## How to install

1. Open the **Connect IQ app** on your phone.
2. Search for **RainUp** and tap install.
3. The app will sync to your Edge over Bluetooth (keep Garmin Connect
   Mobile open in the foreground during the transfer).
4. On the Edge: open a ride profile → **Data Screens** → pick a screen
   → choose a slot → **Connect IQ Field** → **RainUp**.
5. **A half-screen slot is the best starting point** (the field has
   the most room to show full info there). Try a full-screen Large
   tile for your first ride to learn the language, then move to
   half-screen for normal use.
6. **Keep your phone paired** and Garmin Connect Mobile open in the
   foreground while you ride — RainUp needs the phone's internet to
   fetch weather data.

## Supported devices

Edge 530, 540, 550, 830, 840, 850, 1030 (base + Plus), 1040, 1050,
Edge Explore 2, Edge MTB.

---

## How to read the field — one rule

> **Top row is "now / here". Rows below are "what's coming up".**

Every screen follows that rule. The language helps too: top row ends
in `now` (or just says `Dry`); rows below say `in N min` / `Clearing
in N min` / `Rain next 6h` / `Clear next 6h` — the words tell you the
time is in the future. (Times more than an hour out read as rounded
hours — `~2h`, `~3h` — since the forecast isn't minute-accurate that
far ahead.)

### Row 1 — Right now

| If… | Row 1 reads |
|---|---|
| It's not raining on you | `Dry` |
| It's raining on you | `Drizzle now · 0.1mm` / `Light rain now · 0.8mm` / `Moderate rain now · 2.0mm` / `Heavy rain now · 8.0mm` |
| There's a **thunderstorm** where you are | `Storm now` (or `Storm now · 8.0mm` when it's also raining) |
| Something's wrong | `no GPS` / `no phone` / `no data` *(all in red)* |
| App still loading / no data yet | `--` |

The intensity word maps to mm/h (cycling-tuned):

| Word | mm/h |
|---|---|
| Drizzle | any rain up to 0.2 |
| Light rain | 0.2 – 1.0 |
| Moderate rain | 1.0 – 4.0 |
| Heavy rain | over 4.0 |

### Row 2 — What's ahead

Row 2 **always tells you what's coming** — there's no "blank while
moving" case. The wording maps to the situation:

| Row 2 reads | When |
|---|---|
| `<Intensity> rain in N min` | Rain at a different intensity ahead of you (e.g., dry now → wet, or wet now → heavier/lighter) |
| `Clearing in N min` | You're wet, and rain is forecast to end at your location / along your path |
| `Rain next 6h` | You're wet, and the forecast says it continues at the same intensity through the lookahead |
| `Clear next 6h` | You're dry, nothing expected in the next 6 hours |
| `Storm in N min` / `Storm in ~Nh` | A **thunderstorm** is forecast ahead within the next 6 hours — overrides the normal rain wording |

**Thunderstorms get their own word.** A storm is a stop-riding call, not
just heavier rain — so when the model flags a thunderstorm, RainUp
overrides the normal text. If it's storming where you are, Row 1 reads
`Storm now` (with the rate, e.g. `Storm now · 8.0mm`). If a storm is
forecast ahead within 6 hours, Row 2 reads `Storm in ~2h`. (Drawn from
the official WMO weather code on the same fetch — no extra battery cost.)

If you're in the middle of a long monsoonal ride and just see
"Light rain now · 0.4mm" + "Rain next 6h" + a 6-hour sparkline of
small bars, the field is telling you exactly what it sees: it's light,
it's continuing, no break expected.

### Row 3 — The data attribution row

Row 3 always carries a **small icon** that tells you where the
forecast came from — along your projected path, or at your current
spot. Numeric detail (distance + probability + intensity) appears
alongside the icon when there's something quantitative to report.

| Icon | Meaning |
|---|---|
| **▲ (compass arrow)** | The forecast was sampled **along the direction you're heading**. When there's a rain anchor ahead, the arrow rotates to point at it and Row 3 shows the km. When the path ahead is clear, the arrow stays pointed straight up — "your path, nothing to point to." |
| **⊙ (ring + dot — "here")** | The forecast was sampled at **your current location**. You see this when you're stationary, or when nothing on Row 2 needs a projection anchor. |

**What else Row 3 contains depends on the situation:**

- **▲ + km + % + mm** — projection ahead has a bucket change (e.g., dry → moderate rain). The km is to where the change happens; % and mm describe that incoming weather.
- **▲ + km** (only) — same-intensity continuing along the path, OR clearing (the anchor confirms wet continues, or marks where rain ends).
- **▲** (icon only) — moving, fully dry along the projected path.
- **⊙ + % + mm** — stationary outlook with rain in the hourly view.
- **⊙** (icon only) — stationary, fully dry (or wet-but-clearing, where Row 2 already carries the news).

**Why two icons?** The icon tells you whether what you're reading
depends on your direction (**▲**) or your location (**⊙**). That
matters in one specific scenario: when you're moving, **▲** might say
"Dry — Clear next 6h" because you're outpacing a system that *is*
about to hit your current spot. If you stop at a light, the icon
switches to **⊙** and the forecast resamples where you stand — the
message can flip to "Rain in ~1.5h" without anything else on the
screen needing to change. **The icon transition is itself the
signal:** "what you're reading just changed because your motion did."

When you take a turn, the projection re-samples along your new
heading and the **▲** rotates to match. Row 2 may shift accordingly
— the arrow is your visual confirmation that the field is tracking
your actual direction.

### Sparkline (the six bars at the bottom)

A six-bar sparkline at the very bottom of the field shows the rainfall
forecast for the **next 6 hours at your current location** — one bar
per hour. Bar height tracks rainfall intensity:

- Tiny pip = dry hour
- Short bar = Drizzle (~10–25% of full)
- Medium bar = Light rain (~25–50%)
- Tall bar = Moderate rain (~50–75%)
- Full bar = Heavy rain (75%+)

Use it for: *"is this clearing?" / "is it going to get worse?" / "what's
the next hour or two going to look like?"*

The sparkline also acts as your **current-location backstop while riding**:
if rain is forecast for where you are but not on your projected path, the
sparkline will show it even though row 2 stays blank. (This is by design
— rows 2/3 are for what will reach you; the sparkline is for what's
happening here.)

### Status icons

- **Small red phone-with-slash icon, top-right corner** — your phone
  has lost Bluetooth connection. Without the phone, no fresh weather
  data. Check your phone is on, has Bluetooth, has internet, and
  Garmin Connect Mobile is open in the foreground.

### Colour cues

- **Black or white text** (matches your device theme) = data is fresh
- **Yellow text** = the last fetch was over 10 minutes ago; what you
  see is stale but probably still roughly right
- **Red text** = an error is active. Look at row 1 for which:
  `no phone`, `no GPS`, or `no data` (service unreachable)

---

## Per-slot layout

The field adapts to slot size, but the **data is the same** on Large
and Medium tiles — only the typography (font sizes + icon size) differs.
Small tiles drop the % and mm precision to keep the icon + km readable.

| Slot | What you see |
|---|---|
| **Large** (full-screen) | Row 1 + Row 2 + Row 3 (full: icon + km + % + mm) + sparkline. Big fonts, icon sized to match the number font. |
| **Medium** (half-screen, **and full-width thin strips**) | Same data as Large in smaller fonts. The full-width "strip" tile across the top/bottom of a multi-field screen also uses this layout. |
| **Small** (~quarter-screen, narrow tiles) | Row 1 (with "now" omitted to save room) + Row 2 + compact Row 3: **▲ Xkm** if projection with rain anchor, **⊙ % · mm** if stationary outlook with rain, **icon only (▲ or ⊙)** when fully dry. The "Row 1 = now" rule covers the dropped word. |

**Recommendation: a Medium slot (half-screen) is the sweet spot** —
full data payload, the field has room to breathe.

---

## Limitations (read this before you decide the field is "wrong")

- **Phone in foreground.** Garmin's HTTP bridge only works when Garmin
  Connect Mobile is the foreground app on your phone. Background it and
  weather data goes stale (yellow), then errors out (red).
- **Open-Meteo accuracy.** The weather data is free and hyperlocal but
  not perfect. Forecasts can lag a fast-moving front by 10–15 minutes.
  Heavy localised showers can be entirely missed by the model.
- **Projection follows your turns, with a brief lag.** When you commit
  to a new direction (a turn of more than ~45° held for a few seconds),
  RainUp re-projects along the road you're now on within about a minute.
  In the gap between the turn and that refetch, the path-ahead reading
  still reflects your previous heading — the rotating ▲ is your cue.
- **Stops re-resolve quickly.** When you stop, the icon flips to ⊙
  within a second and Row 2/3 fall back to your current-location
  forecast. The underlying data refreshes within about a minute (a
  fresh fetch is forced for your standing location), so a stop never
  leaves stale path-ahead text on the screen for long.
- **Sparkline is your current location, not your route.** It shows what
  the weather will be where you are *standing* over the next 6 hours.
  Use it for "will it stop?" / "will it intensify?", not "is it raining
  at km 25 of my route?".
- **Projection only fires at cycling speed.** If you're below ~7 km/h,
  the field treats you as stationary — Row 3 shows ⊙, and Row 2 falls
  back to the current-location outlook.
- **Indoor / trainer rides aren't supported.** The projection assumes
  real-world movement. On a smart trainer with the bike stationary but
  speed reading from the trainer, the field thinks you're moving and
  plans against locations you're not actually going to. Use a different
  weather field for indoor sessions.
- **Fetch interval is 5 minutes** (Garmin platform limit for data
  fields). The forecast can't update faster than that on a steady ride.
  Three exceptions where the field forces an earlier refetch:
    - When your phone **reconnects** after a Bluetooth drop.
    - When you **start moving** after a fetch happened while you were
      stationary — so the projection re-plans against your new heading
      and pace.
    - When you **stop** after a fetch happened while you were moving —
      so Row 2/3 resample at your standing location.
  The rolling ETA (minutes-to-hit) updates every second from your
  current speed and GPS, so as you speed up or slow down the timing
  reflects it.

---

## Attribution & privacy

- Weather data is provided by **[Open-Meteo](https://open-meteo.com/)**
  under their free non-commercial license. RainUp sends only your GPS
  coordinates (and the projected coordinates ahead of you) to
  Open-Meteo to fetch the forecast. No personal information is
  collected or transmitted.
- **No analytics. No tracking. No remote storage.** Everything happens
  between your Edge, your phone, and Open-Meteo.
- Full privacy policy: <https://github.com/kzkvh2/garmin-rainup/blob/main/PRIVACY.md>

## Feedback & issues

Bug reports, feature ideas, or general feedback:
<https://github.com/kzkvh2/garmin-rainup/issues>

Photos of the screen (when something looks wrong) are the single most
useful thing — they capture fonts, layout, state, and freshness in one
shot.

---

## Appendix — Full scenario matrix

Every state the field can show. **Motion** is *moving* when you're riding
above ~7 km/h, otherwise *stationary*. The sparkline (six bars at the
bottom) is present on every non-error state — it shows the next-6-hour
hourly forecast at your current location regardless of what's on Rows 2/3.

| # | Scenario | Motion | Row 1 | Row 2 | Row 3 |
|---|---|---|---|---|---|
| 1 | No GPS lock | any | `no GPS` *(red)* | — | — |
| 2 | Phone disconnected | any | `no phone` *(red)* | — | — |
| 3 | Service unreachable | any | `no data` *(red)* | — | — |
| 4 | Field loading first fetch | any | `--` | — | — |
| 5a | Dry, no rain in horizon | **moving** | `Dry` | `Clear next 6h` | `▲` |
| 5b | Dry, no rain in horizon | stationary | `Dry` | `Clear next 6h` | `⊙` |
| 6 | Dry, rain projected ahead (bucket transition) | **moving** | `Dry` | `Mod rain in 30min` | `▲ 9km · 80% · 2.0mm` |
| 7 | Dry, rain in hourly outlook | stationary | `Dry` | `Mod rain in 60min` | `⊙ 80% · 2.0mm` |
| 8 | Wet, same bucket continuing through projection | **moving** | `Light rain now · 0.4mm` | `Rain next 6h` | `▲ 27km · 95% · 0.6mm` |
| 9 | Wet, clearing within projection horizon | **moving** | `Light rain now · 0.4mm` | `Clearing in 45min` | `▲ 18km` |
| 10 | Wet, intensity escalating ahead (transition) | **moving** | `Light rain now · 0.4mm` | `Heavy rain in 30min` | `▲ 9km · 90% · 5.5mm` |
| 11 | Wet, same bucket continuing (stationary outlook) | stationary | `Light rain now · 0.4mm` | `Rain next 6h` | `⊙ 95% · 0.6mm` |
| 12 | Wet, clearing forecast (stationary) | stationary | `Light rain now · 0.4mm` | `Clearing in 45min` | `⊙` |
| 13 | Thunderstorm where you are | any | `Storm now · 8.0mm` | normal (e.g. `Rain next 6h`) | per motion |
| 14 | Thunderstorm forecast ahead | any | `Dry` / `Light rain now · 0.4mm` | `Storm in ~2h` | per motion |

**Reading the matrix:**

- **▲ (compass arrow) on Row 3** — the forecast was sampled along your
  projected path. When there's a rain anchor ahead, the km value is
  the distance to that GPS spot and the arrow rotates to point at it.
  When the path is clear (Scenario 5a), the arrow stays pointed up —
  "your path, nothing to point to."
- **⊙ (HERE icon) on Row 3** — the forecast was sampled at your current
  location. Stationary or no projection-relevant signal to point at.
- **Row 3 always shows an icon when the field is working** — the icon
  attributes the forecast to a path or a spot. Row 3 is icon-less only
  in error/loading states (Scenarios 1–4).
- **Why the icon matters even when it's "all clear"** — when you're
  moving with **▲ Dry — Clear next 6h** and stop at a light, the icon
  flips to **⊙** and the forecast resamples where you stand. The
  message can change ("Rain in ~1.5h") even though nothing else on
  the screen has updated. The icon transition is itself the signal.
- **Row 2 always has text** — even "good news" gets a line
  (`Clear next 6h`). The field never goes silent on you.

### Quick visual reference

Four example tiles, large slot, to anchor your eye. Note Row 3 always
carries an icon (▲ moving / ⊙ stationary) even in the all-clear tile:

```
Dry rider, all clear (moving):   Dry rider, rain ahead:          Wet rider, continuing:        Wet rider, clearing (stopped):
┌─────────────────────┐          ┌─────────────────────┐         ┌─────────────────────┐       ┌─────────────────────┐
│         Dry         │          │         Dry         │         │    Light rain       │       │    Light rain       │
│    Clear next 6h    │          │  Mod rain in 30min  │         │    now · 0.4mm      │       │    now · 0.4mm      │
│          ▲          │          │ ▲ 9km · 80% · 2.0mm │         │    Rain next 6h     │       │  Clearing in 45min  │
│ ▁▁▁▁▁▁              │          │ ▁▃▆▅▂▁              │         │ ▲ 27km · 95% · 0.6mm│       │          ⊙          │
└─────────────────────┘          └─────────────────────┘         │ ▃▃▃▃▃▃              │       │ ▆▃▂▁▁▁              │
                                                                  └─────────────────────┘       └─────────────────────┘
```
