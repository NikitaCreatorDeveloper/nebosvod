# Небосвод · PUBLIC 1.0.0-rc.3

Предрелиз Android-приложения на Kotlin / Compose. Подписанный APK: [GitHub Releases](https://github.com/NikitaCreatorDeveloper/nebosvod/releases/tag/v1.0.0-rc.3).
Версия `1.0.0-rc.3`, versionCode `102`. Репозиторий содержит документацию и публичные release artifacts, без Kotlin source.

| Данные | PUBLIC provider |
|---|---|
| Прогноз, текущая погода, часы/дни | Open-Meteo Forecast API, automatic Best Match |
| ИКВ | Open-Meteo Air Quality / CAMS ENSEMBLE, прежняя шкала 1–5 |
| Географическая подложка | OpenStreetMap |
| Осадки на карте | RainViewer Weather Maps API, последний доступный прошедший радарный кадр |

API-ключи и регистрация не нужны. Google Weather и OpenWeather не используются PUBLIC runtime.
Явное согласованное отличие от личной версии: удалены картографические слои облачности,
температуры и ветра; слой осадков заменён радаром RainViewer. Показатели в прогнозе,
карточки, hourly/daily, offline, виджет, ИКВ, города, настройки и темы сохранены.
Дневная/ночная вероятность осадков = максимум почасовых вероятностей периода (APPROVED).

RainViewer имеет ограниченное покрытие: отсутствие радарных данных не означает отсутствие
осадков. Внизу карты показано время формирования кадра. Затемнение отмечает отсутствие
покрытия. Тайлы доступны до zoom 7; управление масштабом карты до 12 сохранено за счёт
увеличения настоящего родительского тайла, без новой детализации. Nowcast отсутствует.
Публичный RainViewer API предназначен для личного, образовательного и небольшого
общественного использования; доступность не гарантирована.

PUBLIC application ID: `app.nebo.weather.public`; debug: `app.nebo.weather.public.preview`.
Личные `app.nebo.weather` / `app.nebo.weather.preview` не обновляются этим кандидатом.
Личный исходный baseline сохранён в теге `nebo-before-open-meteo-20260925`;
дополнительный снимок до RainViewer находится локально в `out/before-rainviewer-source.zip`.
Песочницы приложений раздельны: автоматического копирования личных данных в PUBLIC нет.
Схемы локальных данных и сроки погодного кэша не изменены.

Сборка: `testDebugUnitTest`, `lintDebug`, `assembleDebug`, `compileReleaseKotlin`.
Статические проверки: `tools/verify-source.py`, `tools/verify-provider-migration.py`,
`tools/audit-public-providers.py`. Не используйте build-and-install для проверки на телефоне.
`PUBLISH-GITHUB.cmd` — отдельная ручная публикация документации и подписанных артефактов;
публикация использует отдельный docs-only репозиторий. Это prerelease, не стабильный выпуск 1.0.

Документы: [Privacy Policy](PRIVACY.md), [источники](SOURCES.md),
[notices](THIRD_PARTY_NOTICES.md), [аудит карты](PUBLIC-MAP-PROVIDER-AUDIT.md),
[миграция](OPEN-METEO-MIGRATION.md).

Weather data by [Open-Meteo](https://open-meteo.com/) · Air quality: CAMS ENSEMBLE ·
Weather data by [RainViewer](https://www.rainviewer.com/) ·
© [OpenStreetMap contributors](https://www.openstreetmap.org/copyright).
