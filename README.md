# Singapore Weather & Outdoor Work Dashboard

**Version 2.6.2 | Safe Developer Test Mode**

A browser-based, location-based weather and outdoor-work decision-support dashboard for Singapore.

The dashboard combines nearest available weather observations, regional air quality, forecast information, lightning-distance assessment, cause-based operational advice, system-health monitoring, refresh-readiness information, and a controlled developer test mode.

## Current Release

```text
Version: 2.6.2
Release: Safe Developer Test Mode
Status: Production candidate
```

Version 2.6.2 retains the location engine and hotfix controls introduced in v2.6.0 and v2.6.1. It adds a hidden, session-only developer test mode for validating lightning, heat, rainfall, air-quality, forecast, combined-hazard, and fallback logic without altering successful live-refresh records.

The final v2.6.2 build also corrects the Lightning Distance Unavailable scenario so its Control Priority is:

```text
DATA VERIFICATION REQUIRED
```

## Main Functions

### Location Selection

Users can select:

- West Reference
- East Reference
- North Reference
- South Reference
- Central Reference
- ITE College West
- Custom Location
- Current Device Location

The selected location controls:

- Nearest Temperature station
- Nearest Humidity station
- Nearest Rainfall station
- Nearest valid WBGT reading where coordinates are available
- Nearest MSS lightning observation
- Nearest forecast area where area coordinates are available
- Regional forecast fallback
- Inferred PSI region
- Cause-Based Advisory Decision Basis
- Location-specific successful-refresh history

### ITE College West Preset

```text
Name: ITE College West
Latitude: 1.3764369
Longitude: 103.7523055
PSI Region: West
```

### Custom Location

Users can enter:

- Location name
- Latitude
- Longitude

The dashboard accepts coordinates within its configured Singapore operating range. The custom location is stored in the browser.

### Current Device Location

The **Use My Current Location** button requests browser geolocation permission.

When permission is granted, the dashboard:

1. Reads the device coordinates.
2. Infers the nearest configured PSI region.
3. Selects nearby weather stations.
4. Calculates the nearest lightning distance.
5. Selects a forecast area or regional fallback.
6. Updates the Cause-Based Advisory.

Device location is user initiated. Browser permission or corporate policy can prevent access.

## Location-Based Lightning

The dashboard calculates the distance from the selected location to the nearest geolocated MSS lightning observation.

### Distance Thresholds

```text
Distance ≤ 8 km
RED

Distance > 8 km and ≤ 15 km
AMBER

Distance > 15 km
GREEN
```

### Lightning Distance Unavailable

If lightning observations are returned but their coordinates cannot be resolved, the dashboard shows:

```text
Lightning Distance Unavailable
```

The dashboard does not treat this condition as clear.

Expected result:

```text
Risk Matrix:
Lightning = DISTANCE UNAVAILABLE
Overall = AMBER

Primary Hazard:
Lightning Distance Unavailable

Control:
Data verification required

Control Priority:
DATA VERIFICATION REQUIRED
```

The user should check official lightning information and apply approved site lightning procedures.

## Location-Based Weather Selection

### Temperature, Humidity, and Rainfall

The dashboard selects the nearest available active station to the selected location.

Each card displays:

```text
Station name
Station ID
Distance from selected location
```

Different datasets can select different stations because station availability can differ.

### WBGT

The dashboard first attempts to use the nearest valid geolocated WBGT reading.

Fallback order:

1. Nearest valid geolocated WBGT reading
2. Valid feed reading without usable coordinates
3. Unavailable

### Forecast

Forecast selection order:

1. Nearest forecast area using valid area coordinates
2. Configured fallback area for the inferred region
3. Mapping Unavailable

### PSI

PSI remains a regional dataset. For custom and device locations, the dashboard infers the nearest configured region:

- West
- East
- North
- South
- Central

## Cause-Based Outdoor Work Advisory

The advisory explains the decision through:

- Primary Hazard
- Measurement
- Trigger
- Control
- Supporting Conditions
- Affected Activities
- Operational Actions
- Control Priority
- Decision Basis

### Control Priority Levels

```text
IMMEDIATE ACTION REQUIRED
HIGH ATTENTION REQUIRED
CONTROLS REQUIRED
ROUTINE MONITORING
DATA VERIFICATION REQUIRED
```

### Main Hazard Types

- Lightning Within 8 km
- Lightning Within 15 km
- Lightning Distance Unavailable
- Hazardous Air Quality
- Very Unhealthy Air Quality
- Unhealthy Air Quality
- High Heat Stress
- Moderate Heat Stress
- Rain Detected
- Rain, Shower, or Thunder Forecast
- Live Data Incomplete
- No Active Hazard

## Risk Matrix

The Risk Matrix evaluates:

- Heat stress
- Lightning
- Air quality
- Weather
- Overall condition

Possible Overall states:

- Red
- Amber
- Green
- Unknown
- Refreshing

Missing critical data is not treated as safe.

## Refresh Readiness

### Refresh Now

The **Refresh Now** button uses the same protected refresh sequence as the automatic schedule.

### Seven-Feed Progress

```text
Refreshing 1/7: WBGT
Refreshing 2/7: Lightning
Refreshing 3/7: PSI
Refreshing 4/7: Temperature
Refreshing 5/7: Humidity
Refreshing 6/7: Rainfall
Refreshing 7/7: Forecast
```

### Refresh Protection

The dashboard includes:

- Five-minute automatic refresh
- Fifteen-second timeout for each API request
- Request cancellation using `AbortController`
- Delay between API requests
- Refresh lock
- One pending refresh queue
- `try` and `finally` cleanup
- Selected-location snapshot for each refresh
- Follow-up refresh when the location changes during an active sequence

### Last Successful Refresh

A successful timestamp is recorded only when:

- All seven feeds are `OK`
- The location has not changed during the refresh
- Developer Test Mode is not active

Successful-refresh history is stored separately for each location key.

### Data Freshness

```text
Fresh: less than 10 minutes
Aging: 10 to less than 20 minutes
Stale: 20 minutes or more
No Successful Refresh: no valid successful timestamp
```

Freshness is based on the last complete seven-feed browser success. It is not the official source publication time.

## System Health

Possible feed states:

- OK
- Loading
- Rate Limited
- Timeout
- Fallback
- Mapping Unavailable
- Unavailable

Possible Dashboard Health states:

```text
HEALTHY
DEGRADED
REFRESHING
INITIALISING
```

## Payload Validation

Validation is enabled for:

- WBGT readings
- Lightning observations
- Temperature stations and readings
- Humidity stations and readings
- Rainfall stations and readings
- Forecast entries

The console reports:

- Record count
- Usable record count
- Schema signature
- Sample records
- PASS
- FALLBACK REQUIRED

Validation is diagnostic. It does not stop a usable approved fallback from being applied.

## Coordinate Parsing

The location parser supports common structures including:

```text
location.latitude / location.longitude
location.lat / location.lon
location.lat / location.lng
coordinates.latitude / coordinates.longitude
coordinates.lat / coordinates.lon
coordinates.lat / coordinates.lng
point.latitude / point.longitude
label_location
labelLocation
latitude / longitude
lat / lon
lat / lng
x / y
[longitude, latitude]
```

Coordinates must pass the configured Singapore-range check.

## Safe Developer Test Mode

### Opening the Panel

Press:

```text
Ctrl + Shift + D
```

or append:

```text
?debug=true
```

The query parameter opens the panel but does not activate a simulation.

### Test Scenarios

- Lightning Red ≤8 km
- Lightning Amber 8-15 km
- High WBGT
- Heavy Rain
- Combined Hazard
- Lightning Distance Unavailable
- Clear Conditions
- Custom

### Scenario Expectations

#### Lightning Red

```text
Distance: 5.0 km
Overall: RED
Primary Hazard: Lightning Within 8 km
Control Priority: IMMEDIATE ACTION REQUIRED
```

#### Lightning Amber

```text
Distance: 12.0 km
Overall: AMBER
Primary Hazard: Lightning Within 15 km
Control Priority: HIGH ATTENTION REQUIRED
```

#### High WBGT

```text
WBGT: 32.5°C
Heat Risk: HIGH
Overall: RED
Primary Hazard: High Heat Stress
Control Priority: HIGH ATTENTION REQUIRED
```

#### Heavy Rain

```text
Rainfall: 15.0 mm
Forecast: Heavy Rain
Weather Risk: MONITOR
Overall: AMBER
Primary Hazard: Rain Detected
Control Priority: CONTROLS REQUIRED
```

#### Combined Hazard

```text
Lightning: 6.0 km
WBGT: 32.3°C
PSI: 120
Rainfall: 12.0 mm
Forecast: Thundery Showers

Primary Hazard: Lightning Within 8 km
Control Priority: IMMEDIATE ACTION REQUIRED
```

#### Lightning Distance Unavailable

```text
Lightning observations: 8
Distance: unavailable
Overall: AMBER
Primary Hazard: Lightning Distance Unavailable
Control Priority: DATA VERIFICATION REQUIRED
```

#### Clear Conditions

```text
Lightning: Clear
WBGT: 28.0°C
PSI: 50
Rainfall: 0.0 mm
Forecast: Fair
Overall: GREEN
Control Priority: ROUTINE MONITORING
```

### Test-Mode Safety Controls

- Hidden by default
- Session only
- Persistent red simulation banner
- Amber outlines around simulated fields
- Live refresh paused during simulation
- Test values excluded from successful-refresh tracking
- Test values not saved to browser storage
- Active test panel cannot be hidden
- Exit requires live-data restoration
- Page reload returns to live mode

### Exiting Test Mode

Select:

```text
Exit Test Mode and Restore Live Data
```

The dashboard will:

1. Clear simulation indicators.
2. Reset location-dependent values.
3. Restore the loading state.
4. Run a full seven-feed live refresh.
5. Recalculate the Risk Matrix and Advisory.

## Data Classification

- WBGT: Nearest valid reading to selected location, with feed fallback
- Lightning: Nearest MSS observation to selected location, with Singapore-wide fallback
- PSI: Inferred region
- Forecast: Nearest area or regional fallback
- Temperature: Nearest active station
- Humidity: Nearest active station
- Rainfall: Nearest active station

## Data Sources

The dashboard uses Data.gov.sg endpoints for:

- WBGT
- Lightning
- PSI
- 2-hour forecast
- Air Temperature
- Relative Humidity
- Rainfall

## Repository Structure

```text
sg-weather-dashboard/
├── index.html
├── README.md
├── CHANGELOG.md
└── Singapore_Weather_Dashboard_User_Manual.docx
```

## GitHub Pages Deployment

1. Back up the current production `index.html`.
2. Upload the corrected v2.6.2 file as `index.html`.
3. Upload `README.md` and `CHANGELOG.md` to the repository root.
4. Commit to the deployment branch.
5. Open the published dashboard.
6. Perform a hard browser refresh.
7. Run the checks below.

## Production Checks

1. Confirm the title and visible version show v2.6.2.
2. Confirm the dashboard loads without a console syntax error.
3. Confirm all seven feeds complete or reach a controlled error state.
4. Test each location preset.
5. Test ITE College West.
6. Test one valid custom location.
7. Test Current Device Location if permission is available.
8. Confirm nearest station names and distances are reasonable.
9. Confirm the inferred PSI region is displayed.
10. Confirm lightning shows distance, no observation, fallback, timeout, or unavailable.
11. Confirm Last Successful Refresh and Freshness update only after a complete live success.
12. Open Developer Test Mode.
13. Run all seven predefined scenarios.
14. Confirm Lightning Distance Unavailable produces Data Verification Required.
15. Exit test mode and confirm a fresh live refresh starts.
16. Confirm no uncaught browser-console errors appear.

## Known Limitations

- Region reference points are representative points, not official boundaries.
- PSI is regional rather than site specific.
- Nearest-station readings may not represent all conditions at the selected location.
- Temperature, Humidity, Rainfall, and WBGT may use different sources.
- Station availability can change between refreshes.
- Forecast information is area based.
- Browser GPS accuracy depends on the device, browser, permission, and environment.
- Corporate browser policy can block geolocation.
- A lightning observation without usable coordinates cannot provide distance.
- Browser storage is local to the browser and device.
- Clearing site data removes stored locations and successful-refresh history.
- External API downtime, timeout, rate limits, or schema changes can affect a feed.
- Developer Test Mode validates rule behaviour but does not validate live API accuracy.
- The cause-based advice is rule based and does not replace a site risk assessment.
- The dashboard does not store permanent historical trends or event logs.
- The dashboard is not an official warning system or system of record.
- Official alerts, approved procedures, risk assessments, and supervisor instructions take precedence.

## Version History

### v2.6.2

- Added Safe Developer Test Mode.
- Added seven predefined scenarios and custom inputs.
- Added simulation warnings and field outlines.
- Paused live refresh during simulation.
- Excluded simulations from successful-refresh tracking.
- Added live-data restoration after testing.
- Corrected Lightning Distance Unavailable to use Data Verification Required.

### v2.6.1

- Fixed the global `location` identifier conflict.
- Renamed the selected location state to `selectedLocation`.
- Added API timeouts.
- Expanded coordinate parsing.
- Added lightning distance fallback.
- Changed validation to non-blocking diagnostics.

### v2.6.0

- Added location-based selection.
- Added ITE College West preset.
- Added Custom Location and Current Device Location.
- Added nearest lightning distance.
- Added nearest station and forecast-area selection.
- Added PSI-region inference.
- Added location-specific freshness.

### v2.5.0

- Added the Cause-Based Advisory.
- Added Primary Hazard, Supporting Conditions, Affected Activities, Operational Actions, Control Priority, and Decision Basis.

### v2.4.2

- Added Refresh Now, seven-feed progress, Last Successful Refresh, and Data Freshness.

### v2.4.1

- Added Active Region, Dashboard Health badges, System Health status detail, Forecast Area, and Data Classification.

### v2.4.0

- Added regional Temperature, Humidity, and Rainfall station selection.

### v2.3.0

- Added preferred-region persistence.

### v2.2.x

- Added lightning priority, refresh protection, forecast mapping protection, and incomplete-data handling.

### v2.1.0

- Added regional PSI and forecast mapping.

### v2.0.0

- Created the Singapore multi-zone dashboard.

## Author

Created by **Kelvin Siow**.

## Disclaimer

This dashboard provides decision support only. Verify conditions using official information and apply approved workplace safety, emergency, and operational procedures before work starts or continues.
