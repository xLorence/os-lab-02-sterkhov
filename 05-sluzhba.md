# Задание 5. Собственная служба systemd

### 1. Скрипт

Я создал скрипт `"/usr/local/bin/zagruzka-log.sh"`:

```
#!/bin/bash
echo "Система загружена: $(date)" >> /var/log/moya-zagruzka.log

После создания сделал его исполняемым:

sudo chmod +x /usr/local/bin/zagruzka-log.sh
```

### 2. Файл службы

Файл службы находится по адресу:

`/etc/systemd/system/zagruzka-log.service`

Его содержимое:

```
[Unit]
Description=Запись отметки о загрузке системы
After=multi-user.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/zagruzka-log.sh

[Install]
WantedBy=multi-user.target
```
После создания файла я выполнил:

- sudo systemctl daemon-reload
- sudo systemctl enable zagruzka-log.service
- sudo systemctl start zagruzka-log.service

### 3. Проверка службы

Проверил состояние службы командой:

`sudo systemctl status zagruzka-log.service`

Служба была успешно загружена и включена в автозапуск. В выводе указано:

`Loaded: loaded (...); enabled`

Также выполнение завершилось с:

`status=0/SUCCESS`

Это означает, что скрипт выполнился без ошибки.

После выполнения службы её состояние стало "inactive (dead)". В данном случае это нормально, потому что используется "Type=oneshot". Служба выполняет команду один раз и после этого завершается.

### 4. Проверка файла журнала

Я проверил файл:

`cat /var/log/moya-zagruzka.log`

В нём появились две записи:

```
Система загружена: Sat Oct 3 01:02:54 AM +07 2026
Система загружена: Sat Oct 3 01:03:56 AM +07 2026
```

Таким образом, служба работает и записывает дату и время загрузки системы.

### 5. Объяснение параметров

`"Type=oneshot"` означает, что служба выполняет указанную команду один раз и после завершения останавливается.

`"WantedBy=multi-user.target"` означает, что служба подключается к цели `"multi-user.target"` и поэтому может автоматически запускаться при загрузке системы.

# Что пошло не так

Сначала при создании службы я допустил ошибку в строке `"ExecStart"` и перенёс путь к скрипту на следующую строку. Из-за этого systemd не смог правильно прочитать файл службы и выдал ошибку `"bad unit file setting"`.

После исправления файла и повторного выполнения `"daemon-reload"` служба запустилась успешно.
