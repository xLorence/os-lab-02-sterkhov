# Задание 2. Настройка загрузчика GRUB
### Что я изменил?
Я изменил 2 параметра в файле `/etc/default/grub` :
```
GRUB_TIMEOUT=10 (было 5)
GRUB_TIMEOUT_STYLE=menu (его изначально не было, поэтому прописал вручную)
```
### Обновление GRUB
После изменения файла я выполнил:

`sudo update-grub`

Команда была выполнена успешно и система перезагрузилась
### Проверка
После перезагрузки меню GRUB отображалось 10 секунд<img width="2132" height="1594" alt="7" src="https://github.com/user-attachments/assets/fb99ec4d-7be2-4430-9523-6579948f5cea" /><img width="1187" height="256" alt="8" src="https://github.com/user-attachments/assets/8a2c7098-f055-4700-85cb-a19c59469703" />

