# RainUp — Changelog

A user-facing record of what changed between releases. Internal/historical names appear in parentheses where they help — the app was developed under the working name "RCMC Weather" and renamed to **RainUp** at v2.1 for the Connect IQ Store.

---

## v2.2 — 2026-05-21 *(first store submission)*

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
