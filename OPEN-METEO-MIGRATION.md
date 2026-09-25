# Миграция PUBLIC: Open-Meteo / OSM / RainViewer

> Исторический отчёт этапа миграции до подписи и публикации. Указания «не опубликован»,
> «signing не выполнялся» и результаты debug APK ниже относятся к тому этапу.
> Последующий подписанный PUBLIC предрелиз: [1.0.0-rc.3](https://github.com/NikitaCreatorDeveloper/nebosvod/releases/tag/v1.0.0-rc.3);
> applicationId `app.nebo.weather.public`, versionCode `102`. См. [release notes](RELEASE-NOTES.md).

## Границы и сохранность

PRIVATE/PERSONAL ORIGINAL: unchanged, baseline commit
`56e2062164452f1338fc71ba7abccec0ffee2946`, tag `nebo-before-open-meteo-20260925`.
Рабочее дерево содержит PUBLIC-кандидат. Перед RainViewer дополнительно сохранён
`out/before-rainviewer-source.zip` и SHA-256 manifest.
Телефон, личная установленная версия, её ключи и данные не затрагивались.
PUBLIC имеет отдельный application ID `app.nebo.weather.public` (debug `.preview`).
Это новая песочница, не автоматический перенос личной установки. Совместимые схемы
DataStore/cache сохранены. Push, подписанный release и публикация не выполнялись.

## Окончательное решение владельца

Forecast / AQI — Open-Meteo. Basemap — OpenStreetMap. Precipitation radar — RainViewer.
Google Weather и OpenWeather полностью исключены из PUBLIC сетевого runtime.
Удалены их key stores, callbacks подключения, экран ввода и ссылки на кабинеты ключей.
Исторический AQI parser и Google error fixtures находятся только в test source.
GoogleWeatherService/Parser и старые имена файлов/полей кэша остались как локальный
совместимый секционный протокол: endpoint выбирает исключительно Open-Meteo provider.

Единственное согласованное функциональное отличие карты: cloud/temperature/wind
overlays удалены, precipitation overlay заменён наблюдательным радаром RainViewer.
Никакого grid sampling Open-Meteo, имитации слоёв или radar nowcast нет.
Удалены только недействующие элементы выбора слоёв; навигация и controls карты сохранены.
Настройки поставщиков теперь открывают информацию вместо ввода несуществующих ключей.

## Прогноз и APPROVED probability

OpenMeteo DTO → adapter → существующий секционный протокол → Forecast/Conditions/Hour/Day.
Официальный Forecast API, без ключа, automatic Best Match, timezone auto, epoch seconds,
°C, m/s, mm; pressure_msl сохранён согласно прежней семантике. 48 прогнозных часов,
24 часа истории по запросу, 10 дневных периодов 07:00–07:00 (11 календарных дней API).
Восход/закат и DST учитываются в часовом поясе локации; polar null не выдумывается.
WMO codes сопоставляются существующим состояниям, описаниям, иконкам и фону.

**APPROVED владельцем:** дневная/ночная вероятность осадков = максимум почасовых
`precipitation_probability` за соответствующий период 07–19 / 19–07. Это не вероятность
объединения событий. Показатель, UI и формат отображения не меняются.
Тест `approvedDayNightProbabilityIsMaximumOfHourlyProbabilities` проверяет максимум,
границы периода и прежний формат чисел. API time для осадков, probability и gust max
относится к завершающемуся часу; адаптер относит значение к правильному интервалу Nebo.

## AQI / offline / widget

Open-Meteo Air Quality: шесть концентраций → максимум категорий прежней шкалы 1–5.
Сохранены пороги SO2 [20,80,250,350], NO2 [40,70,150,200], PM10 [20,50,100,200],
PM2.5 [10,25,50,75], O3 [60,100,140,180], CO [4400,9400,12400,15400] μg/m³.
[Историческое происхождение порогов](https://openweathermap.org/api/air-pollution)
не является активным провайдером. US/European AQI не подставляются, null не становится нулём.
UI ИКВ и его поля сохранены. Ранее в миграции добавлен атомарный AQI disk snapshot с прежним TTL 1 час.

Текущая погода/часы сохраняются до 55 минут, дни/история до 23 часов. Форматы и TTL
этой RainViewer-итерацией не менялись. Виджет и его ресурсы не редактировались:
проверяются побайтно относительно снимка до карты. Старый неиспользуемый текст ветки
widget `configured=false` не означает запрос ключа: PUBLIC всегда передаёт `configured=true`.

## Regression matrix

| Возможность | PUBLIC | Результат |
|---|---|---|
| Current/feels like/метрики | Open-Meteo → прежние domain fields | Сохранено |
| Hourly/daily/history, units, timezones | Прежние интервалы и UI | Сохранено |
| Day/night probability | Hourly max | APPROVED, отдельный тест |
| AQI | Прежний набор и шкала | Сохранено |
| Offline/process death | Прежние файлы/TTL + ранее добавленный AQI snapshot | Сохранено |
| Widget/configuration/background | Общий repository, прежние ресурсы | Сохранено |
| Locations/settings/themes | Прежние DataStore keys/schema | Сохранено |
| Map basemap | OSM | Сохранено |
| Map precipitation | RainViewer observed radar | Согласованная замена; покрытие/zoom ограничены источником |
| Map clouds/temperature/wind | Удалены | Согласованное PUBLIC-отличие |
| Map navigation/pan/zoom/recenter/refresh | Прежний native renderer | Сохранено; above z7 увеличиваются реальные родительские тайлы |
| Source setup | Информация вместо ввода ключей | Необходимая часть keyless миграции |
| Personal phone app | Не запускалась/не устанавливалась | UNTOUCHED |

## Проверки и доказательства

31 JUnit methods: 16 Open-Meteo, 8 RainViewer, 7 существующих (включают наборы
offline/cache/widget/domain contracts). `testDebugUnitTest`, `lintDebug`, `assembleDebug`,
`compileReleaseKotlin` проверяются локально; signed release не производится.
304 source/static checks; 68 regression checks; отдельный source/config/APK provider scan.
RainViewer HTTP smoke: metadata + реальные raster/coverage tiles, без ключей, HTTP 200.
Android runtime проверяется только в одноразовом `emulator-5580`.
Логи: `out/rainviewer-build.log`, `out/rainviewer-static.log`, `out/rainviewer-regression.log`,
`out/public-candidate-scan.json`, `out/public-provider-audit.json`, `out/rainviewer-live-http.json`.
Итоговые runtime результаты: `out/public-runtime-checks.json`.
Подробности карты и ограничений: [PUBLIC-MAP-PROVIDER-AUDIT.md](PUBLIC-MAP-PROVIDER-AUDIT.md).


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
