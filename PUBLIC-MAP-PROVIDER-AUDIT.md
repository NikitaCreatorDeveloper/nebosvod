# PUBLIC map/provider audit

> Исторический отчёт этапа миграции до подписи и публикации. Указания «не опубликован»,
> «signing не выполнялся» и результаты debug APK ниже относятся к тому этапу.
> Последующий подписанный PUBLIC предрелиз: [1.0.0-rc.3](https://github.com/NikitaCreatorDeveloper/nebosvod/releases/tag/v1.0.0-rc.3);
> applicationId `app.nebo.weather.public`, versionCode `102`. См. [release notes](RELEASE-NOTES.md).

## Окончательная архитектура

| Функция | Личный baseline | PUBLIC |
|---|---|---|
| Forecast | Google Weather | Open-Meteo Forecast |
| AQI | OpenWeather | Open-Meteo Air Quality, прежняя шкала |
| Basemap | OSM | OSM |
| Precipitation overlay | OpenWeather precipitation_new | RainViewer observed radar |
| Clouds overlay | OpenWeather clouds_new | Удалён по решению владельца |
| Temperature overlay | OpenWeather temp_new | Удалён по решению владельца |
| Wind overlay | OpenWeather wind_new | Удалён по решению владельца |

Прежний отчёт FAIL описывал промежуточную сборку с четырьмя OpenWeather overlays.
Владелец затем разрешил указанное отличие PUBLIC и явно выбрал RainViewer.
Поиск эквивалентной замены четырёх слоёв прекращён. RainViewer в baseline отсутствовал;
он добавлен этой итерацией по прямому указанию владельца, а не объявлен старой функцией.

## Реальные сетевые пути

- `OpenMeteoWeatherProvider` → `https://api.open-meteo.com/v1/forecast`.
- `OpenMeteoAirQualityProvider` → `https://air-quality-api.open-meteo.com/v1/air-quality`.
- `RainViewerRepository` → `https://api.rainviewer.com/public/weather-maps.json`.
- `RainViewer.latest` принимает только HTTPS tilecache.rainviewer.com, валидированный
  returned path `/v2/radar/{id}` и свежий прошедший кадр из `radar.past`.
- `MapTileRepository` → RainViewer `{host}{path}/512/{z}/{x}/{y}/2/1_1.png` и
  `{host}/v2/coverage/0/512/{z}/{x}/{y}/0/0_0.png`.
- Basemap → `https://tile.openstreetmap.org/{z}/{x}/{y}.png`.

Google Weather / OpenWeather endpoints и key stores удалены из PUBLIC production code.
Карты доступны без key gate; provider settings содержат информацию, а не форму ключа.
Static scan проверяет production source/resources/config и DEX/resources debug APK.
Это не packet capture и не универсальное доказательство отсутствия произвольного
обфусцированного endpoint; в данном коде явные URL и их call paths проверены.
Parser/service и cache names Google используются только в локальном compatibility protocol.

## Ограничения, которые соблюдает реализация

[Weather Maps API](https://www.rainviewer.com/api/weather-maps-api.html),
[Transition Summary](https://www.rainviewer.com/api/transition-faq.html),
[Color Schemes](https://www.rainviewer.com/api/color-schemes.html).

На момент проверки доступны past radar, Universal Blue (2), zoom максимум 7;
nowcast/IR не используются. В официальной общей FAQ есть устаревшие/противоречивые
фразы о nowcast и лимитах; применены более строгие конкретные ограничения перехода 2026:
100 requests/IP/minute. Локальный общий budget 80/мин, backoff после 429, ограниченная
параллельность, объединение одинаковых tile requests, HTTP/bitmap cache и metadata cache.
Общий IP может обслуживать другие приложения, поэтому абсолютная гарантия квоты невозможна.
Никакого массового prefetch. Metadata refresh не чаще 5 минут / retry после ошибки от минуты.

Сохранён zoom UI 3–12. На уровнях 8–12 crop/scale настоящего тайла z7 не добавляет
детализацию и не делает запрещённых z8+ запросов RainViewer. Отсутствие покрытия
отмечается оригинальной coverage mask; ошибка маски явно видна. Пустой радар не означает
гарантированное отсутствие осадков. Нет fake слоёв или sampling Open-Meteo.
Показано время формирования кадра UTC; это не точное время каждого измерения.
Отсутствующий/старый кадр не выдумывается; допустимый cached frame помечается сохранённым.
Ошибка радара не скрывает OSM basemap и не превращается в бесконечный spinner.

Видимая linked attribution: Weather data by RainViewer и © OpenStreetMap contributors.
[RainViewer API terms](https://www.rainviewer.com/api.html) допускают личное,
образовательное и небольшое общественное использование без SLA.
[OSM policy](https://operations.osmfoundation.org/policies/tiles/) соблюдается:
HTTPS, identifying User-Agent, HTTP cache, visible attribution, только видимые тайлы.

## Проверки

- Unit tests: выбор latest past (включая opaque hashed paths), отсутствие nowcast fallback,
  empty/stale/future metadata, host/path injection, tile bounds, crop quadrants до z12,
  поддерживаемая палитра, настоящий coverage endpoint.
- Существующие forecast/offline/widget/AQI tests сохранены и повторены.
- Static/regression: `out/rainviewer-static.log`, `out/rainviewer-regression.log`.
- Production/APK/key scan: `out/public-candidate-scan.json`, `out/public-provider-audit.json`.
- HTTP 200 metadata/radar/coverage: `out/rainviewer-live-http.json`.
- Build/lint/debug/release compile: `out/rainviewer-build.log`.
- Emulator map, offline, widget: `out/public-runtime-checks.json` и `out/public-*.xml/png`.

PRIVATE/PERSONAL ORIGINAL сохранён локальным baseline; телефон UNTOUCHED.
PUBLIC sandbox отдельный. PUBLISH-GITHUB.cmd не запускался; RIGHTS-REVIEWED не вводился.
Технический кандидат не является опубликованным или подписанным release.


## Итог локальной валидации

PUBLIC PROVIDERS — PASS. Google Weather / OpenWeather runtime — REMOVED. API keys — NO.
31 JUnit methods PASS / 0 FAIL; lint 0 errors / 41 warnings; debug build и PUBLIC release compile — PASS.
304 source checks, 68 regression checks, 359 PUBLIC source/config/APK checks — PASS.
11 emulator runtime checks — PASS: online forecast/map/AQI, offline/process death, widget
configuration/pin, network update 20:21 → 20:32, reboot offline и сохранение выбранного города.
Протестированный APK совпадает по SHA-256 с финальной сборкой; package проверен в APK manifest.

Ограничение offline-карты: датированные metadata восстанавливаются, но не все radar tiles
доступны без сети; показывается явная ошибка, данные не имитируются. Полный offline-радар
не заявляется. Во время cold boot/reboot эмулятора наблюдался System UI ANR; после Wait
проверка продолжена. Падений PUBLIC приложения в crash log не обнаружено.

Личный baseline commit/tag не изменены, телефон не использовался. Одноразовый эмулятор закрыт.
PUBLISH-GITHUB.cmd не запускался; RIGHTS-REVIEWED не вводился; push, signing и публикации не было.
PUBLIC CANDIDATE READY — технический локальный кандидат, не опубликованный release.
