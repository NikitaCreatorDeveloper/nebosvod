# THIRD_PARTY_NOTICES · PUBLIC

This document records the main third-party software, data and network services used by the public Nebosvod portfolio release. It supplements, and does not replace, the original licenses and service terms.

## Data providers

### Open-Meteo

Weather data by [Open-Meteo](https://open-meteo.com/).

Air quality: CAMS ENSEMBLE via [Open-Meteo Air Quality API](https://open-meteo.com/en/docs/air-quality-api).

Open-Meteo data attribution is provided under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Service usage is also subject to the [Open-Meteo terms](https://open-meteo.com/en/terms). The free public API is used by this non-commercial portfolio release.

### RainViewer

Weather data by [RainViewer](https://www.rainviewer.com/).

Radar imagery is requested through the public Weather Maps API with visible linked attribution. The [RainViewer API terms](https://www.rainviewer.com/api.html) describe the free public API as intended for personal, educational and small-scale community use and do not provide an uptime or global coverage guarantee.

This release uses observed past radar frames only; no radar nowcast is claimed.

### OpenStreetMap

© [OpenStreetMap contributors](https://www.openstreetmap.org/copyright).

OpenStreetMap geographic data is available under ODbL. Access to the public tile service follows the [OSMF tile usage policy](https://operations.osmfoundation.org/policies/tiles/), including attribution, caching and avoiding bulk/prefetch use.

Google Weather and OpenWeather are not network providers of the PUBLIC runtime. Historical names in compatibility/cache code or audit documentation do not imply an active dependency or endorsement.

## Software libraries

The Android build uses Kotlin, kotlinx.coroutines, AndroidX components (including Compose/Material, WorkManager and DataStore) and OkHttp. These projects/components are distributed under Apache License 2.0 or contain Apache-2.0 licensed components as applicable.

JUnit 4 is used for tests only and is licensed under Eclipse Public License 1.0.

The Android packaging process retains applicable bundled `META-INF` license/notice resources from resolved dependencies. The source project’s dependency inventory was reviewed during release preparation; the public docs-only repository does not contain the resolved Gradle tree.

References:

- Apache License 2.0: https://www.apache.org/licenses/LICENSE-2.0
- Eclipse Public License 1.0: https://www.eclipse.org/legal/epl-v10.html
- Kotlin: https://github.com/JetBrains/kotlin
- AndroidX: https://source.android.com/docs/setup/about/licenses
- OkHttp: https://github.com/square/okhttp

## Nebosvod's own material

Nebosvod application binaries, original documentation, name and original artwork are governed by the repository’s [Nebosvod Portfolio License](LICENSE). Public availability does **not** mean the unpublished Kotlin source code is open source.

The official `1.0.0-rc.3` build is distributed as a non-commercial portfolio/showcase release. Third-party licenses and service terms always take precedence for their respective material and services.
