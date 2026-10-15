# Changelog

All notable changes to the Singapore Weather & Outdoor Work Dashboard are recorded in this file.

## [2.7.0 RC1] - 2026-10-15

### Added

- NEA 1-hour PM2.5 data feed.
- PM2.5 summary value and dedicated dashboard card.
- PM2.5 health status and unavailable-data handling.
- Regional PM2.5 selection based on the selected location's inferred Singapore region.
- PM2.5 bands for Normal, Elevated, High, and Very High conditions.
- PM2.5 triggers in the air-quality risk, overall risk, banner, cause-based advisory, supporting conditions, affected activities, operational actions, and decision basis.
- 30-minute lightning clearance countdown.
- Lightning suspension state after a red or amber observation.
- Clearance start after a successful check confirms no lightning within 15 km.
- Clearance reset when a new lightning observation occurs within 15 km.
- Clearance lockout when the lightning feed fails or distance cannot be resolved.
- Full clearance-period restart after lightning data service recovery.
- Eligible-for-resumption review state after countdown completion.
- Supervisor Confirm Resumption control.
- Local-device lightning audit timestamps for red detection, amber detection, clearance start, latest successful check, and supervisor confirmation.
- Location-specific lightning control records in browser local storage.
- PM2.5 support in developer test state and data structures.
- Updated data-source and data-classification information.

### Changed

- Increased monitored live feeds from seven to eight.
- Updated the summary section from four to five indicators.
- Updated the air-quality calculation to consider both 24-hour PSI and 1-hour PM2.5.
- Updated cause-based hazard prioritisation to include PM2.5 and active lightning clearance.
- Updated lightning card status wording for suspension, clearance, review, and unavailable-data states.
- Updated the main banner to keep activities suspended during an active lightning clearance.
- Updated the risk matrix to show PM2.5-driven air-quality conditions.
- Updated refresh progress and completion messages for eight feeds.
- Updated location-change loading logic for PM2.5 and lightning clearance state.
- Updated the dashboard title and displayed version to 2.7.0 RC1.

### Safety Corrections

- A clear lightning reading no longer removes an existing suspension immediately.
- Lightning clearance does not start until a successful check confirms no observation within 15 km.
- Any observation within 15 km resets the clearance start.
- Lightning API failure blocks clearance and prevents resumption based on stale data.
- Countdown completion does not automatically resume activities.
- Supervisor confirmation is required before the local suspension state is closed.
- Lightning distance-unavailable results are not treated as clear conditions.

### Retained from 2.6.2

- Singapore regional presets.
- ITE College West preset.
- Custom location entry.
- Current device location option.
- Location-based nearest-station selection.
- Regional PSI inference.
- Nearest forecast-area selection with regional fallback.
- WBGT, temperature, humidity, rainfall, lightning, PSI, and forecast feeds.
- Data freshness and last-successful-refresh tracking by location.
- API timeout, rate-limit, and unavailable-feed handling.
- Payload and schema validation logging.
- System Health panel.
- Cause-based outdoor work advisory.
- Risk matrix.
- Safe Developer Test Mode.
- Five-minute scheduled refresh and manual refresh.

### Validation

- JavaScript syntax check completed successfully.
- Version reference check completed.
- PM2.5 endpoint and interface element checks completed.
- Eight-feed refresh sequence check completed.
- Lightning clearance constant and supervisor confirmation checks completed.

## [2.6.2]

### Baseline

- Multi-location Singapore dashboard with regional presets, ITE College West, custom coordinates, and device location.
- Seven live weather and environmental feeds.
- Safe Developer Test Mode.
- Cause-based advisory, risk matrix, system health, data freshness, and validation logic.

## Release File

- `SG_Weather_Dashboard_v2.7.0_RC1.html`
