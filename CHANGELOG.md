# Changelog

All notable changes to the **Singapore Weather & Outdoor Work Dashboard** are documented in this file.

---

## [2.4.2] - 2026-09-10

### Operational Readiness Enhancement

#### Added

- Added a **Refresh Now** button.
- Added seven-feed refresh-progress reporting:
  - WBGT
  - Lightning
  - PSI
  - Temperature
  - Humidity
  - Rainfall
  - Forecast
- Added a Last Successful Refresh field.
- Added a separate Last Dashboard Refresh Completed field.
- Added Data Freshness status:
  - Fresh
  - Aging
  - Stale
  - No Successful Refresh
- Added browser storage for the last successful refresh timestamp.
- Added the last successful refresh and freshness status to the footer.
- Added refresh completion messages for fully successful and degraded refreshes.

#### Changed

- Updated the browser title to v2.4.2.
- Updated the visible version label to:

```text
Version 2.4.2 | Operational Readiness Enhancement
```

- Changed freshness calculation to use the last fully successful seven-feed refresh.
- Changed the Refresh Now button to a disabled state while a refresh is active.
- Kept the five-minute automatic refresh schedule.

#### Refresh behaviour

- The Refresh Now button uses the same protected refresh path as the automatic refresh.
- Parallel refresh sequences are prevented.
- An additional refresh request during an active refresh records one pending refresh.
- The selected region is captured once for each seven-feed sequence.
- The refresh button is restored in the `finally` block.
- A degraded refresh updates the completion time but does not overwrite the last successful timestamp.

#### Freshness thresholds

```text
Fresh: less than 10 minutes
Aging: 10 to less than 20 minutes
Stale: 20 minutes or more
No Successful Refresh: no valid successful timestamp
```

#### Retained

The following functions remain in place:

- Preferred-region persistence
- Active Region indicator
- Regional PSI
- Regional forecast-area selection
- Regional Temperature station selection
- Regional Humidity station selection
- Regional Rainfall station selection
- Lightning Priority Banner
- Advisory priority
- Risk Matrix thresholds
- Forecast fallback order
- API request spacing
- Refresh lock protection
- Pending-refresh queue
- API failure handling
- Live Data Incomplete protection
- Dashboard Health and System Health
- Five-minute automatic refresh

#### Operational impact

- Users can request an immediate refresh without starting a parallel API sequence.
- Users can see the current feed being refreshed.
- Users can distinguish a refresh attempt from a complete seven-feed success.
- Users can identify when the last complete dataset is Fresh, Aging, or Stale.
- The successful timestamp remains available after page reload in the same browser.

---

## [2.4.1] - 2026-09-09

### Operational Visibility Enhancement

#### Added

- Added an Active Region indicator below the Singapore clock.
- Added the Active Region to the footer Dashboard Status summary.
- Added coloured Dashboard Health badges.
- Added detailed status text for each System Health data feed.
- Added a Forecast Area label.
- Added a footer Dashboard Status section.
- Added a Data Classification legend.
- Added a version tooltip.

#### Changed

- Reformatted regional station information to show station name, station ID, distance, and selected region.
- Changed the System Health display from a basic list to a feed-and-status grid.
- Improved mobile presentation for Active Region, System Health, and Data Classification.

#### Retained

No weather-engine or safety-decision logic was intentionally changed in this release.

---

## [2.4.0] - 2026-09-09

### Regional Weather Station Selection

#### Added

- Added nearest-active-station selection for Air Temperature.
- Added nearest-active-station selection for Relative Humidity.
- Added nearest-active-station selection for Rainfall.
- Added representative reference points for West, East, North, South, and Central.
- Added station name, station ID, distance, and selected-region information.
- Added regional loading states for Temperature, Humidity, and Rainfall.

#### Changed

- Changed Temperature from the first returned reading to a selected regional station reading.
- Changed Humidity from the first returned reading to a selected regional station reading.
- Changed Rainfall from the first returned reading to a selected regional station reading.
- Expanded selected-region refresh handling to PSI, Forecast, Temperature, Humidity, and Rainfall.

#### Reliability

- Regional selection uses only stations with coordinates and a current numeric reading.
- Each refresh captures the selected region before requesting regional feeds.
- Failed regional station selection produces an unavailable state rather than an unrelated reading.

---

## [2.3.0] - 2026-09-09

### Preferred Region Persistence

- Added preferred-region storage using `localStorage`.
- Added region validation.
- Added automatic restoration of the region selector during page load.
- Added safe fallback to West.
- Added browser storage error handling.
- Removed an invalid `refreshZone` reference from the clock update function.

---

## [2.2.5 Patch 2.1] - 2026-09-09

### Region Refresh State Completion Fix

- Added selected-region refresh state.
- Cleared previous PSI and forecast values during region changes.
- Added regional feed readiness handling.
- Expanded regional forecast fallback areas.

---

## [2.2.4] - 2026-09-09

### Refresh Lock Hardening

- Added `try/finally` protection to the controlled refresh function.
- Ensured the refresh lock is released after unexpected errors.
- Preserved pending refresh handling.

---

## [2.2.3] - 2026-09-09

### API Failure and Stale-State Protection

- Added failed metric cleanup.
- Added summary-value cleanup.
- Added Live Data Incomplete handling.
- Added Unknown Risk state.
- Added grey data-integrity indicators.
- Prevented missing critical data from being treated as safe.

---

## [2.2.2] - 2026-09-09

### Forecast Mapping Protection

- Added ordered forecast-area fallbacks.
- Removed silent fallback to an unrelated forecast area.
- Added Mapping Unavailable handling.

---

## [2.2.1] - 2026-09-09

### Refresh Queue Enhancement

- Added one pending refresh request when a refresh is already active.
- Reduced missed selected-region updates.

---

## [2.2.0] - 2026-09-09

### Lightning Operations Enhancement

- Added Lightning Priority Banner.
- Added blinking lightning icon and value.
- Added Hazardous Air Quality banner.
- Aligned PSI card, Risk Matrix, and overall risk thresholds.
- Added lightning-specific outdoor work controls.

---

## [2.1.0] - 2026-09-09

### Regional PSI and Forecast Foundation

- Added regional PSI retrieval.
- Added dynamic PSI labels.
- Added regional forecast mapping.
- Added lightning animation.
- Added test-mode framework.

---

## [2.0.0] - 2026-09-09

### Singapore Multi-Zone Foundation

- Created the Singapore Weather & Outdoor Work Dashboard.
- Added West, East, North, South, and Central selection.

---

## Current Known Limitations

- Region reference points are representative points and not official boundaries.
- Nearest-station readings may not represent the entire selected region.
- Temperature, Humidity, and Rainfall can use different stations.
- Lightning observations are Singapore-wide and not selected by region.
- WBGT is a national feed and is not selected by region.
- Forecasts are area-based and not exact-site forecasts.
- Preferred-region and successful-refresh storage are local to each browser and device.
- Clearing browser site data removes stored values.
- External API downtime or rate limits remain possible.
- Freshness is based on the last seven-feed browser success, not official source publication time.
- No historical trend storage is included.
- No persistent operational event log is included.
- The dashboard is not an official system of record.

---

## Release Control Checklist

Before publishing:

1. Confirm the browser title and visible version label show v2.4.2.
2. Confirm all test-mode switches are `false`.
3. Run a JavaScript syntax check.
4. Check for duplicate and missing HTML IDs.
5. Test all five regions.
6. Confirm Active Region follows the selector.
7. Confirm preferred region persists after reload.
8. Confirm Refresh Now is disabled during an active refresh.
9. Confirm progress moves through all seven feeds.
10. Confirm the completion timestamp updates after each completed refresh.
11. Confirm the successful timestamp updates only when all seven feeds are `OK`.
12. Confirm freshness changes from Fresh to Aging and Stale at the defined thresholds.
13. Confirm System Health shows all seven feeds.
14. Confirm no browser console errors appear.

---

## Author

Created by **Kelvin Siow**.
