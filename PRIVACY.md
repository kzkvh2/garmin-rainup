# RainUp — Privacy Policy

*Last updated: 2026-05-21*

RainUp is a Connect IQ data field for Garmin Edge cycling computers, developed by Peter Lizan as an independent project. This document describes what data RainUp uses, where it goes, and what we do (and don't do) with it.

## What data RainUp uses

While RainUp is active on your Edge, it reads the following from your device's standard activity sensors:

- **Your current GPS coordinates** (latitude and longitude)
- **Your heading** (the direction you're facing)
- **Your current speed and average speed**

These values are read locally from `Toybox.Activity.Info` — the same data source any data field uses. They are not transmitted to any server beyond what's described below.

## What data RainUp sends, and to whom

To produce a weather forecast, RainUp makes HTTP requests to **[Open-Meteo](https://open-meteo.com)** — a free, open-data weather API.

Each request contains:

- A latitude / longitude pair (either your current location, or a future projected location 30 / 60 / 90 minutes ahead of you along your heading)
- No identifiers, no user IDs, no device serial numbers, no account information

Open-Meteo's own privacy policy applies to those requests: <https://open-meteo.com/en/terms>. As of the date above, Open-Meteo states it does not collect personal data and does not require an API key. RainUp does not send any other information.

Requests are routed through the **Garmin Connect Mobile** app on your paired phone (Garmin's standard Connect IQ bridge). RainUp does not connect to the internet directly from the Edge.

## What data RainUp stores

- **On your Edge device**: a small cache of the most recent weather response (timestamps, precipitation values, and the projected coordinates the response was queried at), via `Application.Storage`. This cache is discarded after 30 minutes. Nothing else is persisted locally.
- **On any server, anywhere**: nothing. RainUp does not operate a backend, does not have a user database, and does not run analytics or telemetry of any kind.

## What RainUp does NOT do

- No analytics
- No tracking
- No advertising
- No account creation
- No telemetry
- No third-party SDKs (other than Garmin's own Connect IQ runtime and Open-Meteo for weather)
- No selling, sharing, or licensing of your data to anyone

## Permissions

RainUp requests the following Connect IQ permissions (declared in its `manifest.xml`):

- **Communications** — required to fetch weather data from Open-Meteo via the phone bridge
- **Positioning** — required to read your GPS coordinates so the forecast is for the right location

These are the only permissions the app uses.

## Children's privacy

RainUp is not designed for or directed at children under 13. It collects no personal information from anyone.

## Changes to this policy

If this policy changes, the updated version will be posted at this same URL and the "Last updated" date above will be bumped. Material changes will be called out in the release notes for the next published version of RainUp.

## Contact

Questions, concerns, or requests:

- **GitHub Issues**: <https://github.com/kzkvh2/garmin-rainup/issues> *(preferred)*
- **Email**: lizanpeter@gmail.com
