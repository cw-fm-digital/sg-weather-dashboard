# Changelog

All notable changes to the **Singapore Weather & Outdoor Work Dashboard** are documented in this file.

---

## [2.6.2] - 2026-09-11

### Safe Developer Test Mode

#### Added

- Added a hidden Developer Test Mode panel.
- Added keyboard access using `Ctrl + Shift + D`.
- Added optional panel opening using `?debug=true`.
- Added a persistent simulation warning banner.
- Added amber outlines around simulated dashboard fields.
- Added predefined scenarios:
  - Lightning Red ≤8 km
  - Lightning Amber 8-15 km
  - High WBGT
  - Heavy Rain
  - Combined Hazard
  - Lightning Distance Unavailable
  - Clear Conditions
- Added Custom scenario inputs for:
  - Lightning state
  - Lightning distance
  - Lightning observation count
  - WBGT
  - PSI
  - Rainfall
  - Forecast
- Added numeric input limits.
- Added lightning threshold validation.
- Added an Exit Test Mode and Restore Live Data control.

#### Safety Controls

- Developer Test Mode is hidden by default.
- Opening the panel does not activate a simulation.
- Test values are session only.
- Test values are not saved to `localStorage`.
- Live refresh is paused while a simulation is active.
- Simulated results do not update Last Successful Refresh.
- Simulated results do not update location-specific freshness history.
- The test panel cannot be hidden while a simulation is active.
- Page reload returns the dashboard to live mode.
- Exiting test mode starts a full live refresh.

#### Corrected

- Corrected the Lightning Distance Unavailable Control Priority.
- Added an explicit `lightningFallback` branch before general Red, critical-data, Amber, controls, and routine branches.

Correct logic:

```javascript
if(primary.key === "lightningR") {
    pr = "IMMEDIATE ACTION REQUIRED";
    pc = "p-immediate";
} else if(primary.key === "lightningFallback") {
    pr = "DATA VERIFICATION REQUIRED";
    pc = "p-data";
}
```

The Lightning Distance Unavailable scenario now produces:

```text
Risk Matrix:
Lightning = DISTANCE UNAVAILABLE
Overall = AMBER

Primary Hazard:
Lightning Distance Unavailable

Control Priority:
DATA VERIFICATION REQUIRED
```

#### Scenario Results

- Lightning Red: Immediate Action Required
- Lightning Amber: High Attention Required
- High WBGT: High Attention Required
- Heavy Rain: Controls Required
- Combined Hazard: Immediate Action Required
- Lightning Distance Unavailable: Data Verification Required
- Clear Conditions: Routine Monitoring

#### Retained

- Location presets
- ITE College West preset
- Custom Location
- Current Device Location
- Nearest lightning-distance calculation
- Nearest weather-station selection
- WBGT location selection and fallback
- Forecast area selection and fallback
- PSI-region inference
- Cause-Based Advisory
- Refresh Now
- Seven-feed progress
- Request timeout
- Refresh lock
- Pending refresh queue
- Last Successful Refresh by location
- Data Freshness by location
- System Health
- Dashboard Health
- Payload-validation diagnostics

#### Known Minor Risks

- System Health can show all feeds as OK during simulation because the test mode injects controlled healthy feed states.
- Live freshness remains visible during simulation and represents the last live successful refresh.
- `testSnapshot` is captured but live restoration uses a new refresh rather than snapshot restoration.
- Decision Basis can show `None` for lightning distance when the distance is unavailable, while the Primary Hazard correctly states Lightning Distance Unavailable.

These items do not change the corrected hazard priority and do not prevent live data restoration.

---

## [2.6.1] - 2026-09-11

### Location Engine Hotfix

#### Fixed

- Fixed the browser startup error caused by declaring a global variable named `location`.
- Renamed the application state variable to `selectedLocation`.
- Retained payload object properties named `location`.

#### Added

- Added a 15-second timeout for each API request.
- Added request cancellation using `AbortController`.
- Added `TIMEOUT` feed state.
- Expanded coordinate parsing.
- Added support for coordinate arrays where appropriate.
- Added Lightning Distance Unavailable fallback.
- Added `FALLBACK` feed state.
- Added non-blocking schema diagnostics.

#### Changed

- Changed payload validation from blocking validation to diagnostic validation.
- Changed lightning handling to retain the observation count when distance cannot be resolved.
- Changed unresolved lightning distance from clear status to an Amber verification state.
- Allowed later feeds to continue after a controlled feed failure or timeout.

---

## [2.6.0] - 2026-09-11

### Location-Based Selection and Lightning Distance

#### Added

- Added location-based selection.
- Added West, East, North, South, and Central reference locations.
- Added ITE College West preset.
- Added Custom Location.
- Added Current Device Location using browser geolocation.
- Added selected-location persistence.
- Added nearest MSS lightning-distance calculation.
- Added 8 km Red and 15 km Amber thresholds.
- Added nearest Temperature station selection.
- Added nearest Humidity station selection.
- Added nearest Rainfall station selection.
- Added nearest valid WBGT reading selection.
- Added nearest forecast-area selection.
- Added configured regional forecast fallback.
- Added automatic PSI-region inference.
- Added location-specific successful-refresh history.
- Added selected-location snapshot protection.
- Added payload-validation framework.

#### Known Issue

- The release contained a browser startup conflict because the app declared `location` as a global lexical variable.
- The release was superseded by v2.6.1.

---

## [2.5.0] - 2026-09-11

### Cause-Based Advisory Enhancement

#### Added

- Added Primary Hazard identification.
- Added Measurement, Trigger, and Control fields.
- Added Supporting Conditions.
- Added Affected Activities.
- Added hazard-specific Operational Actions.
- Added Control Priority levels.
- Added Decision Basis.
- Added rule-based handling for lightning, heat, air quality, rain, forecast, and missing data.

---

## [2.4.2] - 2026-09-10

### Operational Readiness Enhancement

- Added Refresh Now.
- Added seven-feed refresh progress.
- Added Last Dashboard Refresh Completed.
- Added Last Successful Refresh.
- Added Data Freshness.
- Added successful-refresh browser storage.

---

## [2.4.1] - 2026-09-09

### Operational Visibility Enhancement

- Added Active Region.
- Added Dashboard Health badges.
- Added detailed System Health status.
- Added Forecast Area label.
- Added Dashboard Status footer.
- Added Data Classification.

---

## [2.4.0] - 2026-09-09

### Regional Weather Station Selection

- Added regional Temperature selection.
- Added regional Humidity selection.
- Added regional Rainfall selection.
- Added station name, ID, distance, and region information.

---

## [2.3.0] - 2026-09-09

### Preferred Region Persistence

- Added preferred region browser storage.
- Added validation and West fallback.

---

## [2.2.5 Patch 2.1] - 2026-09-09

- Added selected-region refresh state.
- Cleared prior regional values during location changes.
- Expanded forecast fallback areas.

---

## [2.2.4] - 2026-09-09

- Added `try` and `finally` refresh-lock protection.
- Preserved pending refresh handling.

---

## [2.2.3] - 2026-09-09

- Added failed-metric cleanup.
- Added Live Data Incomplete handling.
- Added Unknown Risk state.
- Prevented missing critical data from being treated as safe.

---

## [2.2.2] - 2026-09-09

- Added ordered forecast fallback.
- Removed silent unrelated-area selection.
- Added Mapping Unavailable.

---

## [2.2.1] - 2026-09-09

- Added one pending refresh request.

---

## [2.2.0] - 2026-09-09

- Added lightning priority banner.
- Added blinking lightning display.
- Added Hazardous Air Quality banner.
- Aligned PSI and overall thresholds.

---

## [2.1.0] - 2026-09-09

- Added regional PSI.
- Added regional forecast mapping.
- Added test switches.

---

## [2.0.0] - 2026-09-09

- Created the Singapore multi-zone dashboard.

---

## Release Control Checklist

Before publishing v2.6.2:

1. Confirm the title and visible version show v2.6.2.
2. Confirm production test switches are `false`.
3. Run a JavaScript syntax check.
4. Check for duplicate and missing HTML IDs.
5. Confirm the dashboard loads without a global `location` declaration error.
6. Confirm all API requests have timeout protection.
7. Test all location presets.
8. Test ITE College West.
9. Test one custom location.
10. Test Current Device Location if permission is available.
11. Confirm station names and distances.
12. Confirm PSI-region inference.
13. Confirm forecast-area selection or approved fallback.
14. Confirm lightning distance, no observation, fallback, timeout, and unavailable states.
15. Run all seven Developer Test Mode scenarios.
16. Confirm Lightning Distance Unavailable shows Data Verification Required.
17. Exit test mode and confirm a full live refresh starts.
18. Confirm simulated data do not update successful-refresh history.
19. Confirm no uncaught browser-console errors appear.
20. Keep v2.6.1 as the immediate rollback version during initial monitoring.

---

## Current Known Limitations

- Reference locations are not official boundaries.
- PSI is regional rather than site specific.
- Nearest-station data may not represent exact site conditions.
- Different feeds can use different stations.
- Forecast information is area based.
- GPS availability and accuracy depend on the browser and device.
- Lightning distance requires usable observation coordinates.
- Browser storage does not transfer between devices.
- External API outages, rate limits, timeouts, and schema changes remain possible.
- Test mode validates decision logic, not live-source correctness.
- The dashboard is not an official warning system or system of record.

---

## Author

Created by **Kelvin Siow**.
