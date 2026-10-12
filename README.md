# Abeam for iPhone

Native iPhone navigation with a clean map interface, GPS position and speed, destination search, driving routes, spoken directions, automatic rerouting, saved destinations, day/night appearance and road notices.

**[Download Abeam.ipa](https://github.com/SpecB1111/abeam-navigation/raw/refs/heads/main/Abeam.ipa)** — the link is populated only after a successful cloud build. Check `BUILD-INFO.json` for the exact source commit and build result.

Save the IPA to Files on your iPhone. Keep AltServer running on your computer, then open **AltStore → My Apps → +** and choose Abeam.ipa. Abeam has a different app identifier from Apex Speed/VØRIX Ride. It requires iOS 17 or later. This is an unsigned device build; AltStore supplies signing. Installation and real-world driving need device testing.

## Coverage, without fake alerts

- Apple MapKit supplies the real map, traffic rendering, place search and routes. Internet and location access are needed.
- Live Traffic NSW supplies six official feeds: incidents, roadworks, floods, fires, alpine conditions and major events. Notices refresh while active; failed feeds are marked unavailable and cached notices expire. Published roadworks/closures can have time-of-day conditions; follow signs. Other states' incident feeds are not connected.
- A camera snapshot is bundled if the OpenStreetMap extract succeeds. These are mapped camera locations, not live detections. Coverage, direction, current operation and road speed limits are not guaranteed. The app shows its snapshot date and source.
- Police and mobile-camera observations can be saved at the current GPS position and expire automatically. They remain on this phone. There is no shared crowd-reporting backend or Waze feed.
- A tested average-speed calculation component is included in the source, but automatic average-speed enforcement zones are **not enabled**: verified direction-specific entry/exit geometry and applicability data have not been obtained. No simulated average is shown.
- There is no offline map download, verified road speed-limit database, lane guidance or guaranteed nationwide camera/incident coverage.

## Navigation and CarPlay

The navigation engine projects GPS fixes onto the planned route, tracks distance to instructions, speaks upcoming maneuvers, detects repeated off-route fixes and requests a new route. Guidance pauses when GPS is stale or inaccurate. The remaining ETA is an estimate scaled from the current route's travel time; it is not a continuously updated traffic forecast. This early build has not been validated on-road and does not claim Apple Maps or Waze reliability.

The optional CarPlay target implements native route previews, a navigation session, maneuver updates, estimates, saved destinations and local-report controls. It must be signed with Apple's approved CarPlay navigation entitlement. **The downloadable AltStore phone app does not have CarPlay enabled.** The entitlement cannot be granted by changing source code or by AltStore. CarPlay hardware remains untested.

## Privacy and sources

Saved destinations, settings and personal reports stay on the device. Apple receives searches and route coordinates to provide those services. NSW feed requests do not include GPS coordinates. No analytics or app account is required. Background GPS runs only during an active drive.

- [Apple MapKit](https://developer.apple.com/documentation/mapkit)
- [Apple CarPlay approval](https://developer.apple.com/carplay/)
- [Transport for NSW Live Traffic](https://opendata.transport.nsw.gov.au/dataset/live-traffic-hazards)
- [© OpenStreetMap contributors — ODbL](https://www.openstreetmap.org/copyright)
- [Waze data access is partner-restricted](https://support.google.com/waze/partners/answer/13458165?hl=en)

## Source and build

`Abeam.zip` contains the source. The GitHub workflow runs Swift core tests, generates the Xcode project, compiles a device app, packages the IPA and compiles the separate CarPlay target. Only a successful build updates the permanent IPA. Source changes trigger another build; an installed copy still needs updating through AltStore.

On a Mac with Xcode and XcodeGen: `swift test`, then `bash Scripts/build-ipa.sh`. The browser files included alongside the native source are the original design preview; the iPhone binary is built from `ios` and `Core`.
