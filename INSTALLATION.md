# Установка и доверие к APK

## Официальный файл

Скачивайте Небосвод только из официального GitHub Release:

- версия: `1.0.0-rc.3`;
- APK: `Nebosvod-1.0.0-rc.3.apk`;
- package: `app.nebo.weather.public`;
- versionCode: `102`;
- SHA-256: `02d685092ea0dddce113281c6f9951ca2a313c839d11008644820a7adf90a160`.

Релиз также содержит `SHA256SUMS.txt` и публичный `publisher-certificate.pem`.

## Проверка SHA-256

Windows PowerShell:

```powershell
Get-FileHash .\Nebosvod-1.0.0-rc.3.apk -Algorithm SHA256
```

macOS / Linux:

```bash
shasum -a 256 Nebosvod-1.0.0-rc.3.apk
```

Результат должен точно совпасть с SHA-256 выше.

## Подпись Android

APK уже подписан отдельным publisher signing key. Приватный ключ и пароли не находятся в GitHub. Публичный сертификат приложен к релизу, чтобы можно было сопоставлять будущие официальные сборки с тем же издателем.

Важно: **подписанный APK** и **проверенный Google разработчик / репутация Play Protect** — не одно и то же.

## Почему Play Protect может показать предупреждение

Прямая установка APK с GitHub является установкой вне Google Play. Google Play Protect может сканировать ранее неизвестные приложения и показывать предупреждение, даже если APK корректно подписан и его SHA-256 совпадает.

Не следует решать это отключением Play Protect у пользователей.

## Правильный путь для владельца проекта

Если Небосвод остаётся GitHub-only приложением для широкой аудитории, официальный путь Android — зарегистрировать владельца и package name `app.nebo.weather.public` в Android Developer Console с тем же signing certificate. Для широкой аудитории нужен Full distribution account; Limited distribution рассчитан только на закрытую группу устройств.

Android Developer Verification подтверждает личность разработчика и регистрацию package name. Это отдельный слой от проверки содержимого Play Protect и не является обещанием, что Google никогда не покажет дополнительный security scan для sideloaded APK.

Если цель — максимально обычная установка для массового пользователя без сценария sideload, наиболее предсказуемый канал — публикация через Google Play. У новых personal Play Console accounts могут действовать обязательные closed-testing requirements перед production.

Официальная документация:

- Android developer verification: https://developer.android.com/developer-verification
- Android Developer Console registration: https://developer.android.com/developer-verification/guides/android-developer-console
- Google Play Protect: https://support.google.com/googleplay/answer/2812853
- Google Play testing requirements: https://support.google.com/googleplay/android-developer/answer/14151465

## Текущий статус

- APK signing — **готово**.
- SHA-256 — **опубликован и совпадает с release artifact**.
- Public signing certificate — **опубликован**.
- Android/Google developer identity + package registration — **требует действий владельца аккаунта**.
- Google Play production listing — **не создавался**.

Это единственная часть доверия/распространения, которую нельзя завершить из репозитория: она требует входа владельца в Google, принятия условий, проверки личности и, для соответствующего типа аккаунта, оплаты/процедуры Google.
