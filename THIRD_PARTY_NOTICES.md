# THIRD_PARTY_NOTICES · PUBLIC

## Data providers

Weather data by [Open-Meteo](https://open-meteo.com/).
Air quality: CAMS ENSEMBLE via [Open-Meteo Air Quality API](https://open-meteo.com/en/docs/air-quality-api).
Open-Meteo data is provided under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
The application aggregates/transforms data to the existing Nebo periods and AQI scale.
See [Open-Meteo terms](https://open-meteo.com/en/terms) for service usage conditions.

Weather data by [RainViewer](https://www.rainviewer.com/).
Radar imagery is used through the official public Weather Maps API, with visible linked credit.
[API terms](https://www.rainviewer.com/api.html) limit free usage to personal, educational
and small community scenarios; no uptime/coverage guarantee. No radar nowcast is offered.

© [OpenStreetMap contributors](https://www.openstreetmap.org/copyright).
Geographic data is available under ODbL; tile service access follows the
[OSMF tile usage policy](https://operations.osmfoundation.org/policies/tiles/).

Google Weather and OpenWeather are not PUBLIC data providers. Historical AQI threshold
provenance and cache names do not imply a network dependency or endorsement.

## Libraries

Kotlin, kotlinx.coroutines, AndroidX (Compose/Material, WorkManager, DataStore and others)
and OkHttp use Apache-2.0. JUnit4 (tests only) uses EPL-1.0.
Existing bundled META-INF LICENSE/NOTICE resources are retained by the Android build.
This notice does not replace the exact resolved dependency inventory and license texts
required for a signed distribution; no new external library was added for RainViewer.
The owner's application source license has not been selected; PUBLIC does not imply an
open-source grant. No release/publication or rights-review confirmation was performed.
