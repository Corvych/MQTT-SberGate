﻿# MQTT-SberGate
MQTT SberGate SberDevice IoT Agent for Home Assistant

Агент представляет собой прослойку между облаком Sber и HomeAssistant (HA).
Его задача взять из HA нужные устройства, отобразить их в облаке Sber и отслеживать
изменения в облаке с последующей передачей в HA.
На данный момент агент выбирает сущности switch и script и отображает их как relay в облаке Sber.

Первоначальные настройки.
Для работы агента необходима регистрация в Studio (https://developers.sber.ru/studio/workspaces/).
Требуется создать интеграцию и получить регистрационные данные для агента (sber-mqtt_login, sber-mqtt_password).
Также необходим токен для управления HA. Очень рекомендую завести для этого отдельного пользователя.
Для получения заходим в профиль пользователя и создаём долгосрочный токен доступа (ha-api_token)

Немного истории.
Проект начался незадолго до нового года, когда подключенную к HomeAsistant ёлочку захотелось включать голосом.
Благо к тому времени у Sberа уже был MQTT-to-Cloud для DIY (https://developers.sber.ru/docs/ru/smarthome/mqtt-diy/mqtt-to-diy).
Правда список поддерживаемых контроллеров ограничивается всеголишь двумя: LogicMachine и Wiren Board.
Суть их работы: установка в них специального агента, который пробросит имеющиеся устройства в облако Sber и будет следить за их изменениями.
Экспериментов было масса. Изначально даже проект начинался на php, но со временем пришло понимание, что быстрее и проще всё реализовать
на родном для HomeAssistant python и завернуть в виде аддона. Возможно лучше было бы в виде интеграции, но проблема со свободным временем
не даёт возможности разбираться и двигаться в этом направлении, да и поставленные задачи решины...
Изначально было реализовано два MQTT подключения. Первое смотрело в облако Sber, а второе в локальный MQTT брокер HomeAssistant.
Было очень неудобно, так как приходило городить огород чтобы HomeAssistant реагировал на данные внутреннего MQTT.
Но, мне подсказали, что у HomeAssistant есть замечательный REST API с помощью которого можно очень просто им управлять.
Поэтому подключение к внутреннему MQTT HomeAssistant было отключено (код до сих пор ещё болтается в недрах агента),
а управление переведено на REST API.
Также подсмотрел замечательную идею управления AndroidTV через adb. Поэтому кроме switch дабавились script.
Теперь стало возможно сказать ассистенту: "Включи камеру на улице". После чего отрабатывает скрипт HA который отправляет в VLC нужный поток.
Попытки прокинуть script как камеру не увенчались успехом, даже писал в поддержку. Получил ответ, что-то вроде "пока не реализовано".

Ссылки.
Регистрация пространства в Studio: https://developers.sber.ru/docs/ru/smarthome/space/registration
Создание проекта интеграции в Studio: https://developers.sber.ru/docs/ru/smarthome/mqtt-diy/create-mqtt-diy-integration-project
Авторизация контроллера в облаке Sber: https://developers.sber.ru/docs/ru/smarthome/mqtt-diy/controller-authorization
Как работает интеграция: https://developers.sber.ru/docs/ru/smarthome/mqtt-diy/integration-scheme
Создание интеграции: https://developers.sber.ru/docs/ru/smarthome/mqtt-diy/create-mqtt-diy-integration
HA REST API: https://developers.home-assistant.io/docs/api/rest