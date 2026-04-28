# Minecraft 1.21.1 NeoForge Server by Ebenburg Team

## 📦 Технические детали

| Параметр | Значение |
|----------|----------|
| **Версия Minecraft** | 1.21.1 |
| **Модлоадер** | NeoForge 21.1.228 |
| **Java** | OpenJDK 21 |
| **ОС** | Ubuntu 24.04 |
| **RAM** | 16GB (Xmx16G, Xms8G) |
| **Порт** | 25565 |

Количество выделенной RAM может варьироваться от ваших потребностей.

## 🚀 Запуск и управление

```bash
# Запуск сервера
cd ~/mc && ./run.sh

# Остановка
/stop # в консоли сервера ИЛИ Ctrl+C
```

## 📂 Структура папок

```
~/mc/
├── run.sh                    # скрипт запуска
├── user_jvm_args.txt         # настройки памяти
├── eula.txt                  # лицензионное соглашение
├── libraries/                # библиотеки (включая server-1.21.1.jar)
└── mods/                     # сюда складывать моды
```
## ⚙️ Настройка памяти

Измените user_jvm_args.txt:
```
-Xmx8G
-Xms4G
```

## 🔄 Подключение через nginx (VPS)

Если используете reverse-proxy, то для подключения можете использовать следующую настройку:

```nginx
stream {
    server {
        listen 25565;
        proxy_pass <ваш_IP>:25565;
        proxy_timeout 600s;
        tcp_nodelay on;
    }
}
```

## 🛠️ Systemd (автозапуск)

```bash
sudo nano /etc/systemd/system/minecraft.service
```

```ini
[Unit]
Description=Minecraft Server 1.21.1 NeoForge
After=network.target

[Service]
Type=simple
User=nom4d
WorkingDirectory=<папка с сервером>
ExecStart=<sh скрипт с запуском>
Restart=on-failure
RestartSec=30

[Install]
WantedBy=multi-user.target
```

## 🔥 Firewall (iptables/ufw)

Если порт 25565 не открыт, добавьте правило:
```iptables
sudo iptables -I INPUT 1 -p tcp --dport 25565 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo netfilter-persistent save
```
```ufw
sudo ufw allow 25565/tcp
sudo ufw enable
```

## ✅ Проверка работоспособности

```bash
# Локально
nc -zv localhost 25565

# C reverse-proxy
telnet <IP сервера (в NAT)> 25565

# Снаружи (через домен)
telnet play.ваш-домен.com 25565
```