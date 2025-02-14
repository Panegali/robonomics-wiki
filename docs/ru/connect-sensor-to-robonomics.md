---
title: Подключение датчика к Робономике
contributors: [LoSk-p, Vourhey]
translated: true
---

## Устройство датчика

Универсальная плата для датчика качества воздуха, основана на ESP8266 позволяет использовать следующие модули:  NODEMCU v3, NODEMCU v2, WEMOS D1 MINI. Устройство расчитано на питание 6 - 24 вольта, при  использовании DC-DC преобразователя DC MINI560.

![plata](../images/sensors-connectivity/plata.png)

Данная плата позволяет подключить датчики PM:

- [SDS011](https://cdn-reichelt.de/documents/datenblatt/X200/SDS011-DATASHEET.pdf)
- PMS1003-6003
- [PMS7003/G7](http://www.plantower.com/en/content/?110.html)
- [SPS 30 PM Sensor](https://sensirion.com/products/catalog/SPS30/)

Возможность подключения по интерфейсу I2C :

- [BMP180](https://cdn-shop.adafruit.com/datasheets/BST-BMP180-DS000-09.pdf) - температура и влажность
- [BME/P280](https://www.mouser.com/datasheet/2/783/BST-BME280-DS002-1509607.pdf) - температура, влажность, атмосферное давление
- [HTU21D](https://eu.mouser.com/ProductDetail/Measurement-Specialties/HTU21D?qs=tx5doIiTu8oixw1WN5Uy8A%3D%3D) - температура и влажность
- SHT3x (I2C) - температура и влажность
- [CCS811 VOC SENSOR](https://www.sciosense.com/wp-content/uploads/documents/Application-Note-Baseline-Save-and-Restore-on-CCS811.pdf) - Летучие органические вещества, еквивалент СО2
- LCD1602/ 2004 / OLED SSD1306 /  SH1106 - поддерживаемые дисплеи

Возможность подключения по интерфейсу 1 Wire :

- DTH22(AM2302) - температура и влажность
- DS18B20 - температура.

Также существует модель MINI уменьшенного размера и с урезанным списком подключаемых устройств. Исходные схемы для обеих моделей можно найти по ссылкам для [полной модели](https://oshwlab.com/ludovich88/aira_sensor_rev0-1) и [модели MINI](https://oshwlab.com/ludovich88/aira_sensor_d1_mini).

> Чтобы получить готовую плату, свяжитесь с разработчиками по адресу vm@multi-agent.io.

После получения/сборки датчика остается только прошить и настроить его.

## Прошивка

Наша прошивка основана на прошивке от [Sensor.Community](https://github.com/opendata-stuttgart/sensors-software), в которую были добавлены несколько датчиков и изменена схема отправки данных. Исходный код можно найти [по ссылке](https://github.com/LoSk-p/sensors-software/tree/master/airrohr-firmware). 

Чтобы прошить датчик вы можете использовать `airrohr-flasher`. Скачайте исполняемый файл для вашей операционной системы из [последнего релиза](https://github.com/airalab/sensors-connectivity/releases).

### Для Linux

Сначала вам нужно добавить пользователя в группу `dialout` (для Ubuntu) для получения доступа к USB порту:

```bash
sudo usermod -a -G dialout $USER
```

После этого перезагрузите компьютер. Далее поменяйте разрешения файла и запустите его:

```bash
chmod +x airrohr-flasher-linux
./airrohr-flasher-linux
```

### Для Windows:
Распакуйте флешер и запустите его двойным кликом. Также вам нужно установить драйвера для USB2serial (Windows 10 должен начать загрузку автоматически):

* Драйвера для NodeMCU v3 (CH340): [Windows](http://www.wch.cn/downloads/file/5.html) ([2018/09/04 v3.4 зеркало](https://d.inf.re/luftdaten/CH341SER.ZIP))

### Для MacOS
Скачайте флешер и запустите его. Также вам нужно установить драйвера для USB2serial: 
* Драйвера for NodeMCU v3 (CH340): [MacOS](http://www.wch.cn/downloads/file/178.html) ([2018/09/04 v1.4 зеркало](https://d.inf.re/luftdaten/CH341SER_MAC.ZIP))

---

Выберите прошивку (на английском или на русском) и нажмите `Upload`. Загрузка прошивки займет некоторое время.

![flasher](../images/sensors-connectivity/7_flasher.jpg)

## Настройка

После загрузки прошивки перезагрузите ESP (просто отключите и подключите заново USB).

Через некоторое время после перезагрузки ESP создаст Wi-Fi сеть с названием RobonomicsSensor-xxxxxxx. Подключитесь к ней с телефона или с компьютера, далее откроется окно авторизации (если оно не открылось в любом браузере перейдите по адресу 192.168.4.1). Выберете в списке вашу Wi-Fi сеть (или напишите сами, если ее нет в списке) и заполните поле с паролем. Также напишите координаты места, где будет установлен датчик, в поле ниже:

![guest](../images/sensors-connectivity/guest_ru.jpg)

Нажмите `Сохранить и перезапустить`.

Плата подключится к указанной Wi-Fi сети и через пару минут вы сможете увидеть данные на [карте](https://sensors.robonomics.network/#/):

![map](../images/sensors-connectivity/14_map.jpg)

## Дополнительная настройка

Для более детальной настройки (она может понадобиться для подключения дополнительных датчиков или отправки данных на собственный сервер) вам нужно найти адрес датчика в вашей Wi-Fi сети. Для этого можно использовать `airrohr-flasher` (ваш компьютер должен находиться в той же сети, к которой подключен датчик). Запустите его и перейдите во вкладку `Discovery`, далее нажмите `Refresh`, подождите немного и появится адрес вашего датчика.

![addr](../images/sensors-connectivity/11_flaser2.jpg)

Перейдите по этому адресу двойным кликом (или введите его в браузере), вы попадете в меню датчика:

![home](../images/sensors-connectivity/home_ru.png)

Во вкладке `Конфигурация` можно настроить используемые датчики:

![sensors](../images/sensors-connectivity/sensors_ru.png)

А также настроить отправку на собственный сервер. Для этого во вкладке `APIs` нужно убрать отметку с `Robonomics` и отметить `Отправить в свой API` и указать адрес сервера и порт (65 для sensors connectivity):

![apis](../images/sensors-connectivity/apis_ru.png)

Для сохранения настроек нажмите `Сохранить и перезапустить`.