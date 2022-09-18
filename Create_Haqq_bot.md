
# Alerting system trough the Telegram in Haqq test network
A short guide on how to set up a notification in telegram (ukranian language)

Отже у нас вже є створенний та працюючий моніторинг тепер ми прикрутимо туди телеграм сповіщання

#### Для цього нам необхідно створити свого бота через Botfather https://t.me/BotFather

Переходимо за посилання та натискаємо кнопку або вводимо /newbot

<img width="394" alt="image" src="https://user-images.githubusercontent.com/59205554/187072414-3bd83456-3399-4edb-850b-6c091312d939.png">

####  Далі він спитає назву бота таким чином:
Гаразд, новий бот. 
Як ми будемо це називати? Виберіть назву для свого бота.
Далі назвіть унікальну силку на ваш бот, вона не повинна співпадати з іншими зареєстрованими силками:

Наприклад HaqqMonitoringAlerting_bot (В кінці знаходится `bot`)

<img width="499" alt="Знімок екрана 2022-09-18 о 09 22 05" src="https://user-images.githubusercontent.com/102728347/190889019-5e502fbd-9dcd-4564-a74b-8e673dda445d.png">


І він видасть вам неоюхідний нам апі ключ таким чином:

Готово! Вітаємо з новим ботом. Ви знайдете його на t.me/HaqqMonitoringAlerting_bot. Тепер ви можете додати опис, про розділ і зображення профілю для вашого бота, перегляньте /help для списку команд. До речі, коли ви закінчите створювати свого крутого бота, надішліть запит у службу підтримки ботів, якщо вам потрібно краще ім’я користувача для нього. Просто переконайтеся, що бот повністю працездатний, перш ніж це робити.

також вам знадобиться чат айди який ви можете отримати за допомогою цього бота https://t.me/RawDataBot

<img width="378" alt="image" src="https://user-images.githubusercontent.com/59205554/187073448-9bde2c35-25b4-48d8-bdc6-2f4f17dbedd1.png">


### Використовуй цей токен щоб отримати доступ до HTTP API:
```shell
5470625631:AAE5pRC5sX8KjGddJHUPfMfsd2zG-_Yq3gE
```
Тримайте свій токен у безпеці та зберігайте його безпечно, будь-хто може використовувати його для керування вашим ботом.

Опис API бота див. на цій сторінці: https://core.telegram.org/bots/api

### Отже заходимо в конфіг та міняємо у строці # Telegram settings та   # Chain specific setting також у # Telegram settings
enabled: yes
та api_key: '5555555555:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA'
```
#exemple '5470625631:AAE5pRC5sX8KjGddJHUPfMfsd2zG-_Yq3gE'
token=''
```
```
sed -i s/'5555555555:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA'/$token/ ~/tenderduty/config.yml
sed -i "s/'enabled: no'/'enabled: yes'/" ~/tenderduty/config.yml
```
таким чином


### Після цього перезапускаємо докер командой
```shell
docker restart tenderduty
```
<img width="491" alt="image" src="https://user-images.githubusercontent.com/59205554/187072869-c2ac14e8-9c6a-4416-9d7c-b1d3ea53755f.png">


тепер ви будете отримувати сповіщання в телеграм!)
