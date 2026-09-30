# Тестирование Небосвода

Этот репозиторий — публичная витрина и канал распространения. Kotlin source не публикуется, поэтому приватный unit/static/runtime test suite нельзя честно перезапустить в GitHub Actions из этого репозитория.

## Финальная валидация публичного кандидата

Проверки выполнялись на исходном проекте перед подписанием и публикацией `1.0.0-rc.3`.

| Область | Результат |
|---|---:|
| JUnit | **31 PASS / 0 FAIL** |
| Android Lint | **0 errors / 41 warnings** |
| Debug build | **PASS** |
| PUBLIC release compile | **PASS** |
| Source/static checks | **304 PASS** |
| Regression checks | **68 PASS** |
| PUBLIC source/config/APK checks | **359 PASS** |
| Emulator runtime checks | **11 PASS** |

### Что входило в runtime-проверки

- онлайн-прогноз;
- карта и получение погодных/радарных данных;
- ИКВ;
- offline/process death;
- конфигурация и закрепление виджета;
- сетевое обновление;
- reboot + offline;
- сохранение выбранного города.

Для RainViewer отдельно проверялись выбор последнего прошедшего кадра, stale/future/empty metadata, отсутствие nowcast fallback, host/path validation, границы тайлов, crop родительских тайлов для UI zoom выше 7 и coverage endpoint.

Для прогноза, кэша, widget и AQI были повторены существующие unit/regression проверки.

## Артефакт

Проверенный публичный APK соответствует опубликованному release artifact:

- файл: `Nebosvod-1.0.0-rc.3.apk`;
- package: `app.nebo.weather.public`;
- versionCode: `102`;
- SHA-256: `02d685092ea0dddce113281c6f9951ca2a313c839d11008644820a7adf90a160`.

Релиз содержит публичный сертификат издателя и `SHA256SUMS.txt`.

## Известные ограничения

- Offline-прогноз сохраняется, но полный offline-радар не заявляется: не все тайлы могут уже находиться в кэше.
- Пустая область RainViewer не интерпретируется как гарантированное отсутствие осадков.
- Во время одного cold boot/reboot эмулятора наблюдался System UI ANR; crash PUBLIC-приложения в журнале не обнаружен.
- Lint warnings не были ошибками сборки; финальный lint завершился без errors.

Подробный исторический отчёт: [PUBLIC-MAP-PROVIDER-AUDIT.md](PUBLIC-MAP-PROVIDER-AUDIT.md).

## Что делает публичный CI

Workflow `Showcase integrity` проверяет только то, что реально находится в этом репозитории: обязательные документы, лицензию, ссылки на галерею, идентификатор пакета и опубликованный SHA-256. Он намеренно не называется Android build/test CI.
