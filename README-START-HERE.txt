BUDS CORE CONTROL — GITHUB ACTIONS + ESIGN

Внутри:
- BudsCoreControl-ESign-starter.zip — исходный проект приложения
- ios-build.yml — инструкция сборки для GitHub Actions

КАК ЗАПУСТИТЬ С IPHONE:

1. Распакуй этот архив в приложении «Файлы».
2. Открой github.com в Safari и войди в аккаунт.
3. Создай новый PRIVATE repository с именем buds-core-control. Не добавляй README, license или .gitignore.
4. В репозитории нажми Add file -> Upload files и загрузи BudsCoreControl-ESign-starter.zip. Сделай Commit changes в ветку main.
5. Нажми Add file -> Create new file.
6. В поле имени файла введи строго: .github/workflows/ios-build.yml
7. Открой ios-build.yml из этого комплекта, скопируй весь текст и вставь в редактор GitHub. Нажми Commit changes.
8. Открой вкладку Actions. Если GitHub попросит разрешить запуск workflows, разреши.
9. Выбери Build Buds Core Control IPA -> Run workflow -> Run workflow.
10. Дождись зелёной галочки. Открой завершённый запуск и скачай artifact BudsCoreControl-unsigned-IPA.
11. Распакуй скачанный artifact в «Файлах». Внутри будет BudsCoreControl-unsigned.ipa.
12. Импортируй IPA в ESign и подпиши своим p12 и mobileprovision. Не загружай сертификат, профиль или пароль в GitHub.

ВАЖНО:
- Этот процесс создаёт unsigned IPA. ESign выполняет подпись на iPhone.
- Для Development/Ad Hoc профиля UDID iPhone должен быть включён в профиль.
- Bundle ID проекта: com.vlad.BudsCoreControl. Профиль должен разрешать этот App ID.
- Не отправляй никому p12, provisioning profile или пароль.
- ANC/Transparency пока не реализованы: сначала необходимо изучить реальный BLE-интерфейс Buds Core. Приложение сейчас предназначено для диагностики BLE.
