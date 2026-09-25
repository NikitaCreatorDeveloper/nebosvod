# PUBLIC data sources

- Forecast: [Open-Meteo Forecast API](https://open-meteo.com/en/docs), automatic model selection, без ключа.
- AQI: [Open-Meteo Air Quality](https://open-meteo.com/en/docs/air-quality-api), CAMS ENSEMBLE.
  Сохраняется существующая шкала 1–5 по концентрациям; US/European AQI не подставляется.
- Basemap: [OpenStreetMap](https://www.openstreetmap.org/copyright),
  [правила тайлов](https://operations.osmfoundation.org/policies/tiles/).
- Precipitation: [RainViewer Weather Maps API](https://www.rainviewer.com/api/weather-maps-api.html).
  Метаданные: `https://api.rainviewer.com/public/weather-maps.json`.
  Путь кадра берётся из `radar.past[].path`, host проверяется; URL не строится из предполагаемого timestamp.
  Используется последний прошедший кадр, palette 2, тайлы 512px и маска покрытия.
  [Текущие ограничения](https://www.rainviewer.com/api/transition-faq.html): zoom ≤7,
  100 запросов/IP/минуту, без nowcast/IR и иных палитр. Локальный бюджет 80/мин включает
  метаданные и обе разновидности тайлов; общий IP может получить 429, после него backoff.
  Метаданные обновляются не чаще 5 минут при успехе / минуты при ошибке; одинаковые тайлы
  объединяются и кэшируются. Только видимые тайлы, без фонового prefetch и массовой загрузки.

Attribution RainViewer и OSM видна на карте и содержит кликабельные ссылки;
Open-Meteo/CAMS attribution в прогнозе и виджете сохранена.
PUBLIC не использует Google Weather/OpenWeather. Legacy Google section/cache identifiers
нужны только для чтения прежнего формата, без сетевого доступа.
PRIVATE/PERSONAL ORIGINAL — unchanged (локальный baseline); приложение на телефоне не затронуто.
