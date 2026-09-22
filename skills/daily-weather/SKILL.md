---
name: "daily-weather"
description: "Provides current weather or the day's forecast (Celsius) for the user's location, based on the timezone/location in USER.md."
---

# Daily Weather Skill

Provides forecast for the weather, either current or for the day.

## What does the agent need?

- Check the user's timezone/location specified in `USER.md` (currently: Mexico City, GMT-6).
- No API key required — uses [Open-Meteo](https://open-meteo.com/) (free, no auth) for geocoding and weather data.

## What actions can it take

- Verify the current weather.
- Check the weather for the day, based on the location and timezone specified in `USER.md`.

## Success definition

- When asked for **current weather**: provides the temperature in Celsius, a summarizing weather title (e.g. "Mostly cloudy"), and brief details (humidity, wind, feels-like).
- When asked for **weather for the day**: provides the day's high/low temperature in Celsius, a summarizing weather title, and brief details (precipitation chance, wind).

## Other skills required

- None

## Usage

Resolve the location once via geocoding, then fetch weather with lat/lon (no API key needed):

```bash
# 1. Geocode the location (skip if lat/lon already known)
LOCATION="Mexico City"
curl -s "https://geocoding-api.open-meteo.com/v1/search?name=${LOCATION}&count=1&language=en&format=json"
# -> read latitude/longitude/timezone from the first result

# 2. Current weather
LAT=19.4326
LON=-99.1332
TZ="America/Mexico_City"
curl -s "https://api.open-meteo.com/v1/forecast?latitude=${LAT}&longitude=${LON}&current=temperature_2m,relative_humidity_2m,apparent_temperature,weather_code,wind_speed_10m&timezone=${TZ}"

# 3. Weather for the day (today's high/low + precipitation chance)
curl -s "https://api.open-meteo.com/v1/forecast?latitude=${LAT}&longitude=${LON}&daily=weather_code,temperature_2m_max,temperature_2m_min,precipitation_probability_max,wind_speed_10m_max&timezone=${TZ}&forecast_days=1"
```

## Weather code → title mapping

Open-Meteo returns numeric WMO `weather_code`s. Translate them to a short human title:

| Code(s) | Title |
|---|---|
| 0 | Clear sky |
| 1, 2 | Mostly clear / Partly cloudy |
| 3 | Mostly cloudy / Overcast |
| 45, 48 | Foggy |
| 51, 53, 55 | Drizzle |
| 61, 63, 65 | Rain |
| 71, 73, 75, 77 | Snow |
| 80, 81, 82 | Rain showers |
| 95, 96, 99 | Thunderstorm |

## Output format

- **Current:** `<title> — <temperature_2m>°C (feels like <apparent_temperature>°C), humidity <relative_humidity_2m>%, wind <wind_speed_10m> km/h`
- **Today:** `<title> — high <temperature_2m_max>°C / low <temperature_2m_min>°C, <precipitation_probability_max>% chance of rain, wind up to <wind_speed_10m_max> km/h`
