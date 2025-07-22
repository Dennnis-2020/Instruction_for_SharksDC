# Запись событий в rsyslog-сервер
## Команда 
Чтобы записать событие на rsyslog-сервер, необходимо создать конфигурацию конечной точки логирования следующей командой, указав требуемые аргументы:

```
aaaevents syslog add  [-h] --help
                      [--protocol PROTOCOL]
                      [--address ADDRESS]
                      [--port PORT]
                      [--use_tls USE_TLS]
                      [--ca_data CA_DATA]
                      [--ca_file CA_FILE]
                      [--log_enabled LOG_ENABLED]
```
>[!CAUTION]
>1. Обязательные аргументы: `[--protocol PROTOCOL]` и `[--address ADDRESS]`.
>2. Во ВЦОД может существовать только одна конфигурация конечной точки syslog-сервера.

## Аргументы команды
`[-h] --help` — отобразить это сообщение и выйти из процесса.\
`[--protocol PROTOCOL]` — протокол передачи данных, можно указать `UDP` или `TCP`.\
`[--address ADDRESS]` — IP-адрес rsyslog-сервера или его доменное имя.\
`[--port PORT]` — порт соединения с rsyslog-сервера. Если порт не указан, по умолчанию присваивается **514** для UDP-протокола и **1468** для TCP-протокола.\
`[--use_tls USE_TLS]` — использовать защищенное соединение с rsyslog-сервером (протокол шифрования TLS).

>*Пример*\
>*`... --use_tls yes ...`*

>[!WARNING]
>1. Подключение по TLS доступно только в конфигурациях, где выбран протокол TCP.
>2. Для подключение с TLS необходимо предоставить сертификат сервера (`CA_DATA` или `CA_FILE`).
>3. Для корректной работы rsyslog-сервера с TLS потребуется установить системный пакет **rsyslog-gnutls**.

`[--ca_data CA_DATA]` — указать содержимое сертификата в командной строке.

>[!NOTE]
>Когда сертификат предоставляется в командной строке, необходимо разделять строки с помощью символа `\n`.

>*Пример*\
>*`... --ca_data "-----BEGIN CERTIFICATE-----\nMIIDnjCCAoagA...G05Ub0k\n-----END CERTIFICATE-----\n"`*

`[--ca_file CA_FILE]` — указать полный путь к файлу сертификату.

>*Пример*\
>*`... --ca_file <full-path-to-certificate>`*

>[!WARNING]
>Аргументы текст предостережения `CA_DATA` и `CA_FILE` не могут быть применены одновременно.

`[--log_enabled LOG_ENABLED]` — включение логирования на сервере rsyslog.

>*Пример*\
>*`... --log_enabled yes ...`*

## Пример использования команды aaaevents syslog add
>*Пример*\
>*`aaaevents syslog add --protocol tcp --address <rsyslog-server-addr> --use_tls yes --ca_file <full-path-to-certificate> --log_enabled yes`*

## Дополнительные материалы
### Использование самоподписанного сертификата
Для TLS-шифрования можно использовать самоподписанный сертификат. Как сгенерировать приватный ключ `key.pem` и самоподписанный сертификат `cert.pem` на 10 лет показано в примере.

>*Пример*\
>*`openssl req -x509 -nodes -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650 -addext 'subjectAltName=IP:<rsyslog-server-addr>'`*

>[!NOTE]
>В сертификате должен присутствовать `subjectAltName`, соответствующий адресу/домену сервера: `subjectAltName=IP:<server-ip-address>` / `subjectAltName=DNS:<server-domain-name>`

### Конфигурация сервера с TLS
Подробно о настройке TLS для сервера можно прочитать в [официальной документации проекта Rsyslog](https://www.rsyslog.com/doc/tutorials/tls.html#server-setup).

