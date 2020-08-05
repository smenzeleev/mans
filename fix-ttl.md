# Обход блокировки раздачи wi-fi с мобильной ap
1. Создать/открыть на редактирование `/etc/init.d/local.autostart`
2. Добавить:
```
 #!bin/bash

sudo iptables -t mangle -A POSTROUTING -j TTL --ttl-set 65
```
> Где 65 - значение ttl для iphone/android (дефолтное 64). Для win.phone 129(дефолтное 128)
3. Дать скрипту право на исполнение `sudo chmod +x /etc/init.d/local.autostart`
4. Добавить скрипт в автозапуск `sudo update-rc.d local.autostart defaults 80`
> В случае ошибки LSB добавить `net.ipv4.ip_default_ttl = 65` в `/etc/sysctl.conf` 
5. Reboot