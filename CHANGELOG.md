# RainUp — Changelog

A user-facing record of what changed between releases. Internal/historical names appear in parentheses where they help — the app was developed under the working name "RCMC Weather" and renamed to **RainUp** at v2.1 for the Connect IQ Store.

---

## v2.2.3 — 2026-05-27

**Storm warning (new):**

- RainUp now calls out **thunderstorms** specifically. When a storm is happening — or coming up within the next few hours — the top row reads **"Storm now"** and the "what's coming" row reads **"Storm in ~2h"**, overriding the normal rain wording. A storm is a stop-riding call, not just heavier rain, so it gets its own state. (Drawn from the official WMO weather code on the same forecast fetch — no extra battery cost.)

**The forecast follows the road you turn onto:**

- Before, the "what's ahead" forecast and the direction arrow were locked to the way you were pointing at the last fetch — up to 5 minutes ago. Turn onto a new road and they'd lag. Now, when you commit to a new direction, RainUp re-checks the forecast along the road you're *actually* on, within about a minute.
- A brief swerve or overtake no longer throws the projection off — it only re-aims when you've genuinely changed direction for a few seconds.

**Honest forecast horizon:**

- Dropped the 90-minute look-ahead. At riding speed that point sat ~40 km down a dead-straight line from where you were — which is rarely where a real, winding ride actually takes you, so it often described the weather somewhere you'd never go. RainUp now looks 30 and 60 minutes ahead, the range it can actually get right. The "now" and "next" rows you rely on are unchanged.

**Readability:**

- Times further than an hour out now read as hours ("Clearing in ~3h") instead of hard-to-parse minutes ("Clearing in 238min"). Beyond an hour, the forecast isn't accurate to the minute anyway.
- Larger "what's coming" row and a rebalanced bottom row on the larger screen layouts, so the middle line is easier to catch at a glance.

**Under the hood (reliability):**

- If a single forecast point drops out over a flaky phone connection, RainUp now keeps the fresh "right here, right now" reading instead of discarding the whole update.
- Pace used for the look-ahead now ignores stale readings left over from a long stop, so the projection doesn't over-shoot when you set off again.
- After a restart mid-ride, RainUp no longer briefly shows look-ahead data aimed along your *old* direction.

**Unit tests:** 111 passing (96 → 111; new coverage for the heading debounce, storm detection, time rounding, and the time-decayed pace buffer).

---

## v2.2.2 — 2026-05-21 *(first store submission)*

**Launcher icon:**

- **Real launcher icon designed and shipped.** A clean two-tone mark: a white raindrop silhouette with a black up-arrow cut out of its centre. Captures "rain + up" in a single shape that reads at every device size. Replaces the placeholder icon that v1 → v2.2.1 carried.
- **Per-device native sizes.** Each Edge model now bundles a launcher icon at its native pixel size — 35×35, 36×36, 40×40, 56×56, and 68×68 — instead of relying on the system's auto-scaling. No more blur on smaller (530 / 540 / 830 / 840) or larger (550 / 850 / 1050) screens.
- Build pipeline updated: `monkey.jungle` declares per-device resource paths; each `resources-<deviceId>/drawables/` carries a native-size `launcher_icon.png` plus a matching `drawables.xml`. `.iq` package build is now warning-free across all 12 devices.

**Unit tests:** 96 passing (no behavioural code change since v2.2.1).

---

## v2.2.1 — 2026-05-21 *(superseded by v2.2.2 before first store submission)*

**Bug fix:**

- **Icon no longer flicks at walking pace.** The ▲ / ⊙ icon flips on the rider's *current* speed (introduced in v2.2). At ~4 km/h walking, GPS jitter could briefly push the speed reading above the 7 km/h motion threshold and flip the icon to ▲ for a tick before reverting. v2.2.1 adds a 3-second consecutive-sample debounce: the icon flips only after 3 seconds of sustained state change, so a one-off jitter spike is ignored. Traffic-light-stop responsiveness for real cycling is unchanged.

**Unit tests:** 96 passing (4 new for the debounce helper).

---

## v2.2 — 2026-05-21 *(superseded by v2.2.1 before first store submission)*

**Bug fixes:**

- **Icon now tracks your current motion instantly.** Previously, the ▲ / ⊙ icon reflected your motion at the moment of the last weather fetch (up to 5 minutes ago). Now it flips the instant you stop or start, so the icon transition is a real, real-time signal.
- **Forecast resamples within ~1 minute of stopping.** When you stop after a moving fetch, the field forces a fresh single-point fetch at your standing location within a minute instead of waiting out the 5-minute cadence.
- **Projection uses your recent pace, not your activity-wide average.** Previously, after an hour of riding plus a 30-minute stop, the field could still plan projection coordinates kilometres ahead of you because Garmin's activity-wide average speed was still above the threshold. Now planning uses a 3-minute rolling pace, so projection coords reflect where you'd actually be.
- **Mid-ride GPS loss now shows red.** If you lose GPS for more than 5 minutes mid-ride (long tunnel, deep cover), the field shows `no GPS` in red instead of continuing to display geographically-stale data under a fresh colour.

**Polish:**

- Limitations section in the user guide rewritten to reflect the new motion-aware behaviour. New limitation called out: indoor / trainer rides aren't supported (the field assumes real-world movement).
- Code cleanup: removed ~200 lines of unused functions that survived earlier refactors (compass-eight-point text, the multi-point URL helpers, the pre-anchor `findFirstRainHit`). No user-visible change.
- Doc comments synced with current behaviour.

**Unit tests:** 92 passing.

---

## v2.1 — 2026-05-21 *(never publicly shipped)*

> Built and packaged for the Connect IQ Store, but a pre-release critique surfaced several bugs (motion-state mismatch, wrong speed signal for projection, mid-ride GPS-loss gap). Fixes rolled into v2.2 without v2.1 reaching any user.

**Renamed app from "RCMC Weather" to "RainUp"** in the Connect IQ launcher and store listing. Same manifest GUID kept so any sideloaded testers from the RCMC era upgrade in place.

**Icon attribution rule unified:**

- Row 3 icon (▲ moving / ⊙ stationary) is now shown in **every working state**, including dry-clear cases where Row 3 previously had no icon at all. The icon's presence is itself information: it tells you whether you're reading a path-ahead forecast or a current-spot forecast.
- New scenario coverage: dry-and-clear-moving (▲ static up-arrow), dry-and-clear-stationary (⊙ icon-only).
- Wet-clearing-stationary case (Scenario 12) now also shows ⊙ for consistency with the "icon attributes the source" rule.

**Icon proportion fix:**

- LARGE-slot icon shrunk ~15% so it matches the MEDIUM slot's icon-to-text proportion (was tied to the chunky number font and looked oversized).
- SMALL-slot icon clamped so it doesn't end up larger than MEDIUM's icon when the SMALL font-fit loop selects a bigger font.

**Manual:**

- Repositioned from "Tester Manual" to end-user "User Guide."
- Drizzle example value fixed (was `0.8mm`, which is actually Light rain per the intensity table; now `0.1mm`).
- Feedback channel moved from personal email to a public GitHub issue tracker.

---

## v2 — 2026-05-20

**Layout polish + motion-triggered refetch.**

- Visual layout refined across LARGE / MEDIUM / SMALL slots: better font scaling, icon sizing, sparkline placement.
- New behaviour: when the field's last fetch happened while the rider was stationary, but the rider has since started moving, the field forces an earlier refetch (within ~60s) so the projection re-plans against the new heading and pace.
- Anchor-based arrow: the ▲ on Row 3 now rotates to point at the rain anchor along the projected path (not just "forward").
- Tester pack distributed to the Cleveland Recreational Mountain Cycling club for real-ride feedback.

---

## v1.2 — earlier

**Anchor-based arrow + HERE icon dispatch.**

- Introduced the two-icon design: ▲ (compass arrow) when Row 3 data attributes to a projection point along the path; ⊙ (HERE) when Row 3 data is from the current location.
- Arrow rotates with the heading to indicate bearing to the rain anchor.

---

## v1.1 — earlier

**Motion-aware Row 2/3 silencing.**

- When moving, the current-location bucket (e.g., "rain in 60 min at this spot") was nulled out so the path-relevant anchor data could win Row 2/3 without competing signals.
- Caller-side gate (in DataSource) to stay under the 9-argument cap enforced by older Edge devices.

---

## v1 — baseline

**Universal Row 1 / Row 2 / Row 3 structure.**

- Top row: current state ("Dry" / "Light rain now · 0.4mm" / etc).
- Middle row: what's coming up ("Light rain in 12 min" / "Clearing in 45 min" / "Rain next 6h" / "Clear next 6h").
- Bottom row: spatial detail (km · % · mm).
- Six-bar sparkline at the bottom: next 6 hours of rainfall at the rider's current location.
- Error states with red text ("no GPS" / "no phone" / "no data").
- Freshness colour cues: black/white when fresh, yellow when >10 min stale.
- Phone-disconnected red icon in the top-right corner.
- Edge family support: Edge 530, 540, 550, 830, 840, 850, 1030 (base + Plus), 1040, 1050, Edge Explore 2, Edge MTB.
- Open-Meteo as the weather data source (free, no API key, hyperlocal).

---

*Internal development history is in the git log of the source repository (private). This changelog captures only what's relevant for someone using RainUp on a Garmin Edge.*
