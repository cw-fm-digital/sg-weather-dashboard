# Singapore Weather & Outdoor Work Dashboard

**Version 2.4.2 | Operational Readiness Enhancement**

A browser-based weather and outdoor-work decision-support dashboard covering five Singapore regions.

The dashboard combines regional PSI and forecast information, nearest-active-station weather readings, Singapore-wide lightning observations, operational advisories, risk status, system health, saved region preference, refresh progress, last successful refresh tracking, and data freshness.

## Current Release

```text
Version: 2.4.2
Release: Operational Readiness Enhancement
```

v2.4.2 extends v2.4.1 with manual refresh control and refresh-readiness information. The release retains the five-minute automatic refresh, API request spacing, region selection, regional station selection, forecast mapping, safety thresholds, advisory priority, refresh lock, and pending-refresh queue.

## Main Features

### Region Selection

Users can select:

- West
- East
- North
- South
- Central

The selected region controls:

- 24-hour PSI
- PSI labels and status
- 2-hour forecast area
- Air Temperature station selection
- Relative Humidity station selection
- Rainfall station selection

### Preferred Region

The selected region is stored in the browser using `localStorage`.

- The selected region remains after a page refresh.
- The selected region is restored when the dashboard is reopened in the same browser.
- Invalid stored values default to West.
- Browser storage errors do not stop the dashboard.

The preference is specific to the browser, device, and dashboard address.

### Active Region Indicator

The current region appears below the Singapore clock and in the footer status summary.

```text
ACTIVE REGION
NORTH
```

### Refresh Now

The **Refresh Now** button starts the existing protected refresh sequence.

- The button is disabled during an active refresh.
- Parallel refresh sequences are prevented.
- If another refresh is requested during an active refresh, one pending refresh is recorded.
- The five-minute automatic refresh remains active.

### Refresh Progress

The dashboard shows each stage of the seven-feed sequence:

```text
Refreshing 1/7: WBGT
Refreshing 2/7: Lightning
Refreshing 3/7: PSI
Refreshing 4/7: Temperature
Refreshing 5/7: Humidity
Refreshing 6/7: Rainfall
Refreshing 7/7: Forecast
```

After completion, the dashboard shows:

```text
Completed: 7/7 feeds OK
```

or:

```text
Completed with unavailable feeds
```

### Last Successful Refresh

The dashboard separates:

- Last dashboard refresh completed
- Last successful refresh

The last successful timestamp updates only when all seven feed states are `OK`.

A degraded refresh updates the completion time but does not overwrite the last successful refresh time.

### Data Freshness

Data freshness is calculated from the last successful refresh.

```text
Fresh: less than 10 minutes
Aging: 10 to less than 20 minutes
Stale: 20 minutes or more
No successful refresh: no valid successful timestamp
```

The last successful refresh timestamp is stored in the same browser using `localStorage`.

## Regional Weather Station Selection

Air Temperature, Relative Humidity, and Rainfall use the nearest available active station to a representative point for the selected region.

Each card displays:

```text
Station: [station name]
ID: [station ID]
Distance: [distance]
Region: [selected region]
```

Different metrics may use different stations because station availability can differ between datasets.

## Regional Forecast Fallbacks

The dashboard checks forecast areas in a defined order.

```javascript
const FORECASTS = {
    west: [
        "Choa Chu Kang",
        "Tengah",
        "Bukit Batok",
        "Bukit Panjang",
        "Jurong East",
        "Jurong West",
        "Clementi"
    ],
    east: [
        "Changi",
        "Tampines",
        "Pasir Ris",
        "Bedok",
        "Paya Lebar",
        "Marine Parade"
    ],
    north: [
        "Woodlands",
        "Yishun",
        "Sembawang",
        "Mandai",
        "Sungei Kadut",
        "Seletar"
    ],
    south: [
        "Sentosa",
        "Bukit Merah",
        "Queenstown",
        "Southern Islands",
        "Kallang"
    ],
    central: [
        "City",
        "Novena",
        "Toa Payoh",
        "Bishan",
        "Bukit Timah",
        "Tanglin",
        "Central Water Catchment"
    ]
};
```

If no configured area is returned, the dashboard shows a mapping unavailable state instead of an unrelated forecast.

## Dashboard Information

The dashboard displays:

- Wet Bulb Globe Temperature, or WBGT
- Lightning observations
- Regional 24-hour PSI
- Regional Air Temperature station reading
- Regional Relative Humidity station reading
- Regional Rainfall station reading
- Regional 2-hour forecast
- Outdoor Work Advisory
- Risk Matrix
- System Health
- Dashboard Health
- Refresh progress
- Last completed refresh
- Last successful refresh
- Data freshness
- Next scheduled refresh

## Dashboard Health

Dashboard Health uses these states:

```text
🟢 HEALTHY
🟠 DEGRADED
🔄 REFRESHING
🔵 INITIALISING
```

### System Health Feed States

Each data feed shows an icon and status label.

Possible states include:

- OK
- Loading
- Rate Limited
- Mapping Unavailable
- Unavailable

## Risk Matrix

The Risk Matrix evaluates:

- Heat stress
- Lightning
- Air quality
- Weather
- Overall condition

Overall states include:

- Red
- Amber
- Green
- Unknown
- Refreshing

Missing critical data is not treated as safe.

## Advisory Priority

The banner and advisory use this priority:

1. Lightning detected
2. Hazardous air quality
3. Live data incomplete
4. Red risk condition
5. Amber risk condition
6. Green condition

### PSI Categories

- Above 300: Hazardous
- Above 200: Very Unhealthy
- Above 100: Unhealthy
- 100 or below: Good / Moderate

### Lightning Alert

When lightning observations are returned:

- The lightning icon blinks.
- The lightning value blinks.
- The banner flashes red.
- The advisory instructs users to suspend exposed activities and move to shelter.

## Reliability Controls

The dashboard includes:

- Five-minute automatic refresh
- Manual Refresh Now control
- Seven-stage refresh progress
- Delay between API requests
- Refresh lock protection using `try` and `finally`
- One pending refresh queue
- Consistent selected-region snapshot during a refresh
- Regional refresh loading state
- Forecast fallback protection
- Failed API state cleanup
- Stale-value prevention
- Live Data Incomplete protection
- Preferred-region validation
- Last-successful-refresh browser storage
- Browser storage error handling
- Freshness calculation based on the last fully successful refresh

## Data Classification

- WBGT: National feed
- Lightning: Singapore-wide observations
- PSI: Regional
- Forecast: Regional area
- Temperature: Regional station
- Humidity: Regional station
- Rainfall: Regional station

## Data Sources

The dashboard retrieves live information through Data.gov.sg endpoints for:

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
└── Singapore_Weather_Dashboard_User_Manual_v2.4.1.docx
```

The dashboard uses one HTML file containing its HTML, CSS, and JavaScript.

## GitHub Pages Deployment

1. Keep a backup of the current production `index.html`.
2. Rename `index_v2.4.2.html` to `index.html`.
3. Upload `index.html`, `README.md`, and `CHANGELOG.md` to the repository root.
4. Commit the files to the `main` branch.
5. Confirm GitHub Pages deploys from `main` and `/ (root)`.
6. Open the published dashboard.
7. Run the production checks below.

## Production Checks

Before deployment, confirm:

```javascript
const TEST_MODE = {
    LIGHTNING: false,
    PSI: false,
    WBGT: false,
    RAINFALL: false
};
```

After deployment:

1. Confirm the browser title and dashboard label show v2.4.2.
2. Confirm the Active Region matches the selector.
3. Test West, East, North, South, and Central.
4. Confirm the preferred region remains after page reload.
5. Select **Refresh Now**.
6. Confirm progress moves from feed 1 to feed 7.
7. Confirm the Refresh Now button is disabled during refresh.
8. Confirm Last Dashboard Refresh Completed updates after every completed sequence.
9. Confirm Last Successful Refresh updates only when all seven feeds show `OK`.
10. Confirm Data Freshness shows Fresh after a fully successful refresh.
11. Confirm PSI and forecast match the selected region.
12. Confirm Temperature, Humidity, and Rainfall show station details.
13. Confirm System Health shows all seven feeds.
14. Confirm there are no browser console errors.

## Known Limitations

- Region reference points are internal representative points, not official administrative or meteorological boundaries.
- The nearest active station may not represent all conditions across a region.
- Temperature, Humidity, and Rainfall may use different stations.
- Station selection can change when a nearer station is unavailable.
- Lightning observations are Singapore-wide and are not filtered by selected region.
- WBGT is a national feed and is not selected by region.
- Forecast information is area-based rather than site-specific.
- Preferred-region and last-successful-refresh storage do not transfer between browsers or devices.
- Clearing browser site data can remove stored preferences and the successful-refresh timestamp.
- External API rate limiting or downtime can produce API Busy or Unavailable states.
- Data Freshness reflects the last complete seven-feed success in that browser, not official API publication time.
- The dashboard does not store historical trends or persistent event logs.
- The dashboard is not an official system of record.
- The dashboard does not replace official alerts, risk assessments, site procedures, or supervisor decisions.

## Version History

### v2.4.2

Operational Readiness Enhancement:

- Added protected Refresh Now control.
- Added seven-feed refresh progress.
- Added last successful refresh tracking.
- Added Fresh, Aging, Stale, and No Successful Refresh states.
- Added browser persistence for the last successful refresh timestamp.
- Added completion text for full and degraded refreshes.
- Retained the five-minute automatic refresh and existing safety logic.

### v2.4.1

Operational Visibility Enhancement:

- Added Active Region indicator.
- Added coloured Dashboard Health badges.
- Added detailed System Health statuses.
- Improved station source formatting.
- Added Forecast Area label.
- Added footer Dashboard Status.
- Added Data Classification legend.
- Added version tooltip.

### v2.4.0

Regional Weather Station Selection:

- Added regional Air Temperature station selection.
- Added regional Relative Humidity station selection.
- Added regional Rainfall station selection.
- Added station name, ID, distance, and region display.
- Expanded the regional refresh state.

### v2.3.0

- Added preferred-region persistence.
- Added saved-region validation.
- Restored the saved region on page load.

### v2.2.x

- Added Lightning Priority Banner.
- Added forecast mapping protection.
- Added refresh queue and refresh-lock protection.
- Added API failure and stale-state protection.
- Added Live Data Incomplete handling.
- Aligned PSI and Risk Matrix thresholds.

### v2.1.0

- Added regional PSI.
- Added regional forecast mapping.
- Added lightning alert animation and test-mode framework.

### v2.0.0

- Created the Singapore multi-zone dashboard.

## Author

Created by **Kelvin Siow**.

## Disclaimer

This dashboard is for decision support only. Verify conditions using official information and apply approved workplace safety, emergency, and operational procedures before work starts or continues.
