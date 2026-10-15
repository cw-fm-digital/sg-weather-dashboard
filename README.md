# Singapore Weather & Outdoor Work Dashboard

**Version:** 2.7.0 RC1  
**Release status:** Release candidate  
**Created by:** Kelvin Siow

## Overview

The Singapore Weather & Outdoor Work Dashboard is a browser-based decision-support tool for monitoring weather and environmental conditions across Singapore.

The dashboard combines live data from NEA and Meteorological Service Singapore datasets available through data.gov.sg. It supports regional reference points, ITE College West, custom Singapore coordinates, and the current device location.

The dashboard does not replace official alerts, approved procedures, risk assessments, or supervisor instructions.

## Current Data Sources

The dashboard processes eight live feeds:

1. Wet Bulb Globe Temperature (WBGT)
2. Meteorological Service Singapore lightning observations
3. 24-hour Pollutant Standards Index (PSI)
4. 1-hour PM2.5
5. Air temperature
6. Relative humidity
7. Rainfall
8. 2-hour weather forecast

## Main Functions

### Location Selection

- West, East, North, South, and Central reference locations
- ITE College West preset
- Custom Singapore coordinates
- Current device location, subject to browser permission
- Automatic regional inference for PSI and 1-hour PM2.5
- Nearest valid station or observation selection for location-based feeds

### Outdoor Work Decision Support

The dashboard provides:

- Current-condition banner
- Cause-based outdoor work advisory
- Primary hazard identification
- Supporting conditions
- Affected activities
- Operational actions
- Control priority
- Decision basis
- Risk matrix for heat, lightning, air quality, weather, and overall conditions

### Data Readiness and Health

- Refresh progress
- Last successful refresh by location
- Data freshness status
- API health by feed
- Dashboard health summary
- Schema and payload validation logging
- Five-minute automatic refresh
- Manual refresh control
- API timeout and rate-limit handling

## Air Quality Logic

### 24-Hour PSI

The dashboard uses the inferred Singapore region for the selected location.

| PSI | Dashboard classification |
|---:|---|
| 0 to 100 | Good / Moderate |
| 101 to 200 | Unhealthy |
| 201 to 300 | Very Unhealthy |
| Above 300 | Hazardous |

The 24-hour PSI supports planning and broader haze-condition assessment.

### 1-Hour PM2.5

The dashboard uses the inferred Singapore region for the selected location and applies the following operational bands:

| 1-hour PM2.5 | Dashboard classification | Operational focus |
|---:|---|---|
| Up to 55 µg/m³ | Normal | Routine monitoring |
| 56 to 150 µg/m³ | Elevated | Reduce strenuous outdoor activity for the next hour |
| 151 to 250 µg/m³ | High | Avoid strenuous outdoor activity for the next hour |
| 251 µg/m³ and above | Very High | Minimise outdoor activity for the next hour |

PM2.5 is used for immediate outdoor activity decisions. PSI remains available for planning and broader haze assessment.

## Lightning Advisory and Clearance Logic

### Distance Bands

| Nearest observation | Dashboard response |
|---:|---|
| 8 km or less | Suspend exposed outdoor activities |
| More than 8 km and up to 15 km | Maintain suspension or prepare to suspend exposed activities |
| More than 15 km | Continue monitoring, subject to any active clearance state |
| Distance unavailable | Do not treat conditions as clear; verify official information |

### 30-Minute Clearance Process

After a lightning suspension is activated:

1. A successful lightning check must confirm no lightning within 15 km before the countdown starts.
2. The dashboard starts a 30-minute clearance countdown.
3. Any new lightning observation within 15 km resets the clearance.
4. A lightning feed failure or unresolved distance blocks clearance.
5. After service recovery, a new successful clear check starts a full 30-minute period.
6. When the countdown reaches zero, the dashboard changes to **Eligible for Resumption Review**.
7. A supervisor must verify conditions and select **Supervisor Confirm Resumption** before the local suspension state is closed.

The dashboard stores lightning control records by selected location in browser local storage. Records include the latest red detection, amber detection, clearance start, successful check, and supervisor confirmation.

## Heat, Rain, and Forecast Logic

- WBGT uses the nearest valid reading to the selected location.
- WBGT values at or above 31.0°C, or a Moderate source classification, trigger heat controls.
- WBGT values at or above 32.0°C, or a High source classification, trigger high heat-stress controls.
- Rainfall above 0 mm triggers wet-weather controls.
- Rain, showers, or thunder in the 2-hour forecast trigger forecast monitoring controls.

## Developer Test Mode

Developer Test Mode supports controlled interface and decision-logic checks.

- Open or hide the panel with `Ctrl + Shift + D`.
- Add `?debug=true` to the page address to open the panel without activating a simulation.
- Simulated values are marked and must not be used for operational decisions.
- Live refresh is paused while a simulation is active.
- Exiting test mode restores live data.

Available scenarios include lightning red, lightning amber, high WBGT, heavy rain, combined hazards, lightning distance unavailable, and clear conditions.

## Local Storage

The dashboard uses browser local storage for:

- Selected location
- Custom location details
- Last successful refresh by location
- Lightning suspension and clearance state by location

Clearing browser storage removes these local records.

## Use

1. Open `SG_Weather_Dashboard_v2.7.0_RC1.html` in a modern browser.
2. Select a location or use the device location option.
3. Allow the dashboard to complete all eight feed checks.
4. Review the banner, advisory, risk matrix, data freshness, and system health.
5. Apply official alerts, approved procedures, risk assessments, and supervisor instructions before making operational decisions.

## Release Validation

The v2.7.0 RC1 source file was checked for:

- JavaScript syntax
- Version references
- PM2.5 API inclusion
- PM2.5 display and advisory elements
- Eight-feed refresh sequence
- Lightning clearance countdown
- Location-specific clearance storage
- Supervisor resumption confirmation

## Limitations

- The dashboard depends on browser access to live data.gov.sg endpoints.
- API availability, payload changes, rate limits, and network restrictions can affect results.
- Current device location requires browser permission and supported Singapore coordinates.
- Browser local storage is device- and browser-specific.
- The dashboard is a decision-support display and is not an official warning system.

## File

- Dashboard source: `SG_Weather_Dashboard_v2.7.0_RC1.html`
