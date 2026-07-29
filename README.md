
# Инструкция по настройке VPN сервера с cdn turboflare (аналог зарубежного cloudflare)

## Содержание

- [Инструкция по настройке VPN сервера с cdn turboflare (аналог зарубежного cloudflare)](#инструкция-по-настройке-vpn-сервера-с-cdn-turboflare-аналог-зарубежного-cloudflare)
  - [Содержание](#содержание)
  - [Первоначчальная настройка](#первоначчальная-настройка)
    - [Начало](#начало)
    - [Безопасность](#безопасность)
      - [Начало](#начало-1)
      - [Для обычных людей](#для-обычных-людей)
      - [Для параноиков](#для-параноиков)
      - [Для удобства](#для-удобства)
  - [Настройка vless xhttp tls реверс прокси + cdn turboflare](#настройка-vless-xhttp-tls-реверс-прокси--cdn-turboflare)
    - [Turboflare](#turboflare)
      - [Регистрация](#регистрация)
      - [Подключение cdn](#подключение-cdn)
      - [Проверка](#проверка)
      - [Настройки cdn](#настройки-cdn)
    - [Настройки сервера (xray-core)](#настройки-сервера-xray-core)
      - [Этап 1. xray на сервер](#этап-1-xray-на-сервер)
      - [Этап 2. Серверный конфиг](#этап-2-серверный-конфиг)
      - [Этап 3. Клиентский конфиг/ссылка](#этап-3-клиентский-конфигссылка)
    - [Выбор заглушки и настройка веб-сервера](#выбор-заглушки-и-настройка-веб-сервера)
      - [Этап 1. Выбор заглушки](#этап-1-выбор-заглушки)
      - [Этап 2. Настройка веб-сервера](#этап-2-настройка-веб-сервера)

## Первоначчальная настройка

### Начало

Сперва подключаемся к серверу:

```
ssh root@ip
```

Дальше поменяем пароль для суперпользователя, если вы при входе на сервер будете использовать Ctrl+C и Ctrl+V, лучшим паролем для вас будет сгенерированый следующей командой:

```
openssl rand -hex 128
```

Меняем пароль:

```
passwd
```

Полностью обновляем операционную систему и автоматически перезагружаем сервер по завершению:

```
apt update && apt upgrade -y && reboot
```

Дальше снова подключаемся к серверу:

```
ssh root@ip
```

### Безопасность

#### Начало

Для начала изменим ssh порт:

```
nano /etc/ssh/sshd_config
```

Ищем там строчку "# Port 22" и меняем её на любой порт:

[Порт](/image/Изменить%20порт.PNG)

Выполняем перезагрузку ssh:

```
systemctl restart sshd
```

или:

```
systemctl restart ssh
```

Проверяем что всё хорошо, открываем ещё терминал и пишем:

```
ssh root@ваш id -p ваш порт
```

Дальше есть 2 пути, один если вы параноик и очень боитесь за сервер (от этого способа как по мне нету смысла), второй для обычных пользователей

#### Для обычных людей

(ПОКА ЧТО МНЕ ЛЕНЬ)

Тут мы настроим подключение к серверу по ssh ключам к сеперпользователю (root)

#### Для параноиков

(ПОКА ЧТО МНЕ ЛЕНЬ)

Тут мы настроим подключение к серверу по ssh ключам к созданному нами пользователю (дальше подключение по паролю к root пользователю)

#### Для удобства

Тут тоже скоро кое что будет но:

(ПОКА ЧТО МНЕ ЛЕНЬ)

## Настройка vless xhttp tls реверс прокси + cdn turboflare

### Turboflare

#### Регистрация

Для начала вам нужно зарегистрироваться в turboflare. Делается всё это очень легко вам потребуется только почта и номер телефон

[turboflare-регистрация](/image/turboflare-регистрация.PNG)

Почту и телефон нужно подтвердить

[turboflare-регистрация(1)](/image/turboflare-регистрация(1).PNG)

Дальше задаём пароль

[turboflare-регистрация(2)](/image/turboflare-регистрация(2).PNG)

И подтверждаем телефон

[turboflare-регистрация(3)](/image/turboflare-регистрация(3).PNG)

#### Подключение cdn

После регистрации нас встречает следующий интерфейс

[turboflare-подключение_cdn](/image/turboflare-подключение_cdn.PNG)

(у вас там ничего не будет)

Сразу скажу что для того чтобы использовать turboflare нужно делегировать ваш домен (поддомен не подойдёт)

Жмём подключить сайт

Дальше нужно будет написать ваш домен и ip адрес сервера с портом :443

[turboflare-подключение_cdn(1)](/image/turboflare-подключение_cdn(1).PNG)

Дальше вы идёте к своему регистратору домена и меняете ns записи на те что указаны в turboflare

[](/image/turboflare-подключение_cdn(2).PNG)

После этого можно спокойно идти гулять на 24 часа, хоть и пишут они от 15 минут, у меня оба домена делегировались 23-24 часа.

После того как вы подождали 24 часа вы заходите в turboflare и нажимаете на кнопочку "перевод трафика", всё ваш домен делегирован и трафик уже идёт через cdn turoflare.

#### Проверка

Если вы хотите вы можете проверить точно ли вам дали ip в бс, для этого нажмите в панеле turboflare на свой домен, дальше перейдите в настройки dns-зоны и пролистав ниже вы увидете свой домен и привязанный к нему ip. Дальше идёте в тг бота @Latency_Lab_bot и проверяете.

[turboflare-проверка_cdn](/image/turboflare-проверка_cdn.PNG)

#### Настройки cdn

Из настроек в turboflare нету почти ничего, по этому тут и нечего говорить, просто сделайте вот так:

[turboflare-настройки_cdn](/image/turboflare-настройки_cdn.PNG)

(Обязательно укажите порт 443)

### Настройки сервера (xray-core)

Я настраивал голый xray поэтому буду расказывать про него, 3x-ui там вроде токже можно такое сделать, remnawave я не изучал так что хз что там, но там как там ноды и она впринцепи не лёгкая значит вы можете взять мой конфиг xray и сами допилить под remnawave.

#### Этап 1. xray на сервер

Для начала нам нужно скачать сам xray с этим нам поможет:

```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install --version v26.7.28
```

И запускаем

```
systemctl enable xray
```

#### Этап 2. Серверный конфиг

Генерируем рандомный UUID клиента:

```
cat /proc/sys/kernel/random/uuid
```

И редактируем конфиг xray

```
nano /usr/local/etc/xray/config.json
```

```
{
  "log": { "loglevel": "warning" },
  "inbounds": [
    {
      "tag": "vless-xhttp-cdn",
      "listen": "127.0.0.1",
      "port": 8081,
      "protocol": "vless",
      "settings": {
        "clients": [{ "id": "СГЕНЕРИРОВАННЫЙ UUID" }],
        "decryption": "none"
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls", "quic"]
      },
      "streamSettings": {
        "network": "xhttp",
        "security": "none",
        "xhttpSettings": {
          "mode": "packet-up",
          "path": "/static/getFile/video/segment.ts",
          "extra": {
            "xmux": {
              "maxConcurrency": "1"
            },
            "seqKey": "chunk_id",
            "sessionKey": "auth",
            "noSSEHeader": true,
            "noGRPCHeader": true,
            "seqPlacement": "query",
            "xPaddingBytes": "50-150",
            "xPaddingMethod": "tokenish",
            "sessionIDLength": "16-32",
            "sessionPlacement": "query",
            "xPaddingObfsMode": true,
            "xPaddingPlacement": "header",
            "scMaxBufferedPosts": 100,
            "scMaxEachPostBytes": 3000000,
            "scMinPostsIntervalMs": "5-10",
            "serverMaxHeaderBytes": 32768
          }
        }
      }
    }
  ],
  "outbounds": [
    { "tag": "direct", "protocol": "freedom" },
    { "tag": "block", "protocol": "blackhole" }
  ]
}
```

Здесь вам нужно поменять только клиентский UUID

Дальше вводим 2 команды:

```
xray run -test -config /usr/local/etc/xray/config.json
```

```
systemctl restart xray
```

#### Этап 3. Клиентский конфиг/ссылка

Клиентский конфиг будет выглядеть следующим образом:

```
{
  "log": { "loglevel": "warning" },
  "inbounds": [
    {
      "tag": "socks-in",
      "listen": "127.0.0.1",
      "port": 1080,
      "protocol": "socks",
      "settings": { "udp": true }
    }
  ],
  "outbounds": [
    {
      "tag": "proxy",
      "protocol": "vless",
      "settings": {
        "vnext": [{
          "address": "ВАШ_ДОМЕН",
          "port": 443,
          "users": [{ "id": "СГЕНЕРИРОВАННЫЙ UUID", "encryption": "none" }]
        }]
      },
      "streamSettings": {
        "network": "xhttp",
        "security": "tls",
        "tlsSettings": {
          "serverName": "ВАШ_ДОМЕН",
          "alpn": ["h2", "http/1.1"],
          "fingerprint": "firefox"
        },
        "xhttpSettings": {
          "mode": "packet-up",
          "path": "/static/getFile/video/segment.ts",
          "extra": {
            "xmux": {
              "maxConcurrency": "1"
            },
            "seqKey": "chunk_id",
            "sessionKey": "auth",
            "noSSEHeader": true,
            "noGRPCHeader": true,
            "seqPlacement": "query",
            "xPaddingBytes": "50-150",
            "xPaddingMethod": "tokenish",
            "sessionIDLength": "16-32",
            "sessionPlacement": "query",
            "xPaddingObfsMode": true,
            "xPaddingPlacement": "header",
            "scMaxBufferedPosts": 100,
            "scMaxEachPostBytes": 3000000,
            "scMinPostsIntervalMs": "5-10",
            "serverMaxHeaderBytes": 32768
          }
        }
      }
    },
    { "tag": "direct", "protocol": "freedom" }
  ]
}
```

Клиентская ссылка будет выглядеть так:

```
vless://СГЕНЕРИРОВАННЫЙ UUID@ВАШ_ДОМЕН:443?type=xhttp&path=%2Fstatic%2FgetFile%2Fvideo%2Fsegment.ts&mode=packet-up&security=tls&sni=ВАШ_ДОМЕН&alpn=h2,http%2F1.1&fp=firefox&extra=%7B%22xmux%22%3A%7B%22maxConcurrency%22%3A%221%22%2C%22hMaxRequestTimes%22%3A%2250%22%2C%22hKeepAlivePeriod%22%3A8%7D%2C%22seqKey%22%3A%22chunk_id%22%2C%22sessionKey%22%3A%22auth%22%2C%22noSSEHeader%22%3Atrue%2C%22noGRPCHeader%22%3Atrue%2C%22seqPlacement%22%3A%22query%22%2C%22xPaddingBytes%22%3A%2250-150%22%2C%22xPaddingMethod%22%3A%22tokenish%22%2C%22sessionIDLength%22%3A%2216-32%22%2C%22sessionPlacement%22%3A%22query%22%2C%22xPaddingObfsMode%22%3Atrue%2C%22xPaddingPlacement%22%3A%22header%22%2C%22scMaxBufferedPosts%22%3A100%2C%22scMaxEachPostBytes%22%3A3000000%2C%22scMinPostsIntervalMs%22%3A%225-10%22%2C%22serverMaxHeaderBytes%22%3A32768%7D#КОММЕНТАРИЙ
```

Но работать оно ещё не будет так как веб-сервер и реверс прокси мы ещё не настроили.

### Выбор заглушки и настройка веб-сервера

#### Этап 1. Выбор заглушки

Вы можете выбрать для заглушки любой сайт, хоть написанный дип сиком, хоть како-нибудь open source проект, я же выберу filestash, просто потому что мне нужно облачное хранилище, а filestash ест мала ресурсов (по сравнению с тем же nextcloud) и относительно удобный

Я его буду ставить на сервер с помощью docker compose, поэтому нам потребуется docker и плагин docker-compose:

```
sudo apt update && sudo apt install ca-certificates curl
```

```
sudo install -m 0755 -d /etc/apt/keyrings && curl -fsSL https://docker.com | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
```

```
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Проверка:

```
docker compose version
```

Создаём структуру проекта:

```
mkdir -p ~/filestash/{data,config}
```

```
cd ~/filestash
```

Дальше редактируем docker-compose:

```
nano docker-compose.yml
```

```
services:
  filestash:
    image: machines/filestash
    container_name: filestash
    restart: unless-stopped
    ports:
      - "127.0.0.1:8334:8334"
    environment:
      - APPLICATION_URL=https://ваш_домен
    volumes:
      - ./data:/state
```

Запускаем:

```
docker compose up -d
```

#### Этап 2. Настройка веб-сервера

Я буду показывать на примере веб-сервера caddy, вы же можете выбрать и nginx

Для начала нам нужно получить сертификат для нашего домена:

```
sudo apt install -y certbot
```

```
sudo certbot certonly --standalone -d ваш_домен --email ваша@почта.com
```

Дальше ставим caddy

```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install -y caddy
```

Caddy сам встанет как systemd-сервис и попытается запуститься на 80/443 с дефолтным конфигом — сразу останавливаем, пока не пропишем свой Caddyfile:

```
sudo systemctl stop caddy
```

Дальше мы должны дать доступ caddy к certbot, так как caddy по умолчанию не имеет root прав и не сможет прочитать сертификат:

```
apt install acl -y
setfacl -R -m u:caddy:rX /etc/letsencrypt/live/
setfacl -R -m u:caddy:rX /etc/letsencrypt/archive/
setfacl -m u:caddy:x /etc/letsencrypt/
```

Дальше редактируем Caddy file:

```
sudo nano /etc/caddy/Caddyfile
```

```
:443, ваш_домен {
    tls /etc/letsencrypt/live/ваш_домен/fullchain.pem /etc/letsencrypt/live/ваш_домен/privkey.pem
    header Strict-Transport-Security "max-age=15552000; includeSubDomains"

    reverse_proxy 127.0.0.1:8334
}
```

Проверяем, заходим на свой домен там должен быть filestash:

[filestash](/image/filestash.PNG)

Задайте пароль от админки.

Дальше вам нужно будет выбрать способ использования filestash:

[filestash(1)](/image/filestash(1).PNG)

Я выберу 2, так как буду просто хранить файлы и раздавать доступ друзьям, вы можете использовать 1 если доступ никаму не будете раздавать

Дальше сразу идёте в настройки и меняете параметр хост убрав https:// перед своим доменом:

[filestash(2)](/image/filestash(2).PNG)

-->

[filestash(3)](/image/filestash(3).PNG)

