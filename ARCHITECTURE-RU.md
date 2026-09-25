# Архитектура PUBLIC

Kotlin / Compose / StateFlow; OkHttp + coroutines; RemoteViews widget; WorkManager.
Open-Meteo DTO → adapter → существующие Forecast/Conditions/Hour/Day и секционный кэш.
Open-Meteo Air Quality → прежняя шкала ИКВ 1–5 → RAM/дисковый снимок.
Карта нативная, без WebView: OSM basemap + RainViewer observed radar/coverage tiles.
Текущий viewport определяет запросы; zoom выше 7 использует crop/scale исходных тайлов.
Схемы DataStore и forecast/widget cache сохраняются. Старые Google имена локального
секционного протокола обеспечивают совместимость и не делают HTTP-запросов.
Ключевые хранилища и экраны ввода Google/OpenWeather удалены из PUBLIC source.
PUBLIC application ID app.nebo.weather.public отделён от личной версии.
