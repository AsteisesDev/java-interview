[Вопросы для собеседования](README.md)

# Настройка и безопасность сети
+ [Как настроить статический IP-адрес в Linux?](#как-настроить-статический-ip-адрес-в-linux)
+ [Как работает DHCP и как его настроить?](#как-работает-dhcp-и-как-его-настроить)
+ [Как настроить DNS в Linux?](#как-настроить-dns-в-linux)
+ [Как настроить маршрутизацию в Linux?](#как-настроить-маршрутизацию-в-linux)
+ [Какие инструменты диагностики сети существуют и как ими пользоваться?](#какие-инструменты-диагностики-сети-существуют-и-как-ими-пользоваться)
+ [Как проверить прослушиваемые порты и доступность сервисов?](#как-проверить-прослушиваемые-порты-и-доступность-сервисов)
+ [Что такое iptables и как работает фильтрация трафика?](#что-такое-iptables-и-как-работает-фильтрация-трафика)
+ [Что такое ufw и как им пользоваться?](#что-такое-ufw-и-как-им-пользоваться)
+ [Что такое DMZ и зачем она нужна?](#что-такое-dmz-и-зачем-она-нужна)
+ [Как работает TLS/SSL и что такое цепочка доверия сертификатов?](#как-работает-tlsssl-и-что-такое-цепочка-доверия-сертификатов)
+ [Что такое mTLS и зачем нужна двусторонняя аутентификация?](#что-такое-mtls-и-зачем-нужна-двусторонняя-аутентификация)
+ [Как настроить HTTPS в Nginx с редиректом с HTTP?](#как-настроить-https-в-nginx-с-редиректом-с-http)
+ [Как усилить безопасность SSH-сервера?](#как-усилить-безопасность-ssh-сервера)
+ [Какие существуют методы защиты от DDoS-атак?](#какие-существуют-методы-защиты-от-ddos-атак)
+ [Какие основные атаки на сеть существуют и как от них защищаться?](#какие-основные-атаки-на-сеть-существуют-и-как-от-них-защищаться)
+ [Что такое OWASP Top 10 и какие уязвимости наиболее актуальны для Java-разработчика?](#что-такое-owasp-top-10-и-какие-уязвимости-наиболее-актуальны-для-java-разработчика)
+ [Как работает CORS и как его настроить?](#как-работает-cors-и-как-его-настроить)
+ [Что такое JWT, какова его структура и лучшие практики использования?](#что-такое-jwt-какова-его-структура-и-лучшие-практики-использования)
+ [Как работает OAuth 2.0 и OpenID Connect?](#как-работает-oauth-20-и-openid-connect)
+ [Что такое NetworkPolicy в Kubernetes?](#что-такое-networkpolicy-в-kubernetes)
+ [Что такое Service Mesh и как он обеспечивает безопасность?](#что-такое-service-mesh-и-как-он-обеспечивает-безопасность)
+ [Как безопасно хранить секреты в приложении?](#как-безопасно-хранить-секреты-в-приложении)
+ [Чем отличается шифрование данных at rest и in transit?](#чем-отличается-шифрование-данных-at-rest-и-in-transit)
+ [Что и как логировать с точки зрения безопасности?](#что-и-как-логировать-с-точки-зрения-безопасности)
+ [Что такое Zero Trust Architecture?](#что-такое-zero-trust-architecture)
+ [Что такое VPN и как настроить базовое VPN-соединение?](#что-такое-vpn-и-как-настроить-базовое-vpn-соединение)
+ [Что такое сетевая сегментация и зачем она нужна?](#что-такое-сетевая-сегментация-и-зачем-она-нужна)

## Как настроить статический IP-адрес в Linux?

Статический IP-адрес — это фиксированный адрес, который не меняется при перезагрузке системы. В серверной среде (особенно в банковской инфраструктуре) статические адреса обязательны для обеспечения стабильной работы сервисов.

### Настройка через Netplan (Ubuntu 18.04+)

Netplan — современный инструмент конфигурации сети в Ubuntu. Конфигурация хранится в YAML-файлах в каталоге `/etc/netplan/`.

```yaml
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
        search:
          - mybank.local
```

Применение конфигурации:

```bash
# Проверить конфигурацию на ошибки
sudo netplan try

# Применить конфигурацию
sudo netplan apply
```

### Настройка через /etc/network/interfaces (Debian, старые Ubuntu)

```bash
# /etc/network/interfaces
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
    dns-search mybank.local
```

Применение:

```bash
sudo systemctl restart networking
# или
sudo ifdown eth0 && sudo ifup eth0
```

### Проверка настроек

```bash
# Посмотреть текущие адреса
ip addr show eth0

# Посмотреть маршруты
ip route show

# Проверить DNS
resolvectl status
```

Для Java-разработчика в банке важно понимать сетевую конфигурацию серверов, так как приложения часто привязываются к конкретным интерфейсам и адресам (например, Spring Boot `server.address=192.168.1.100`).

[к оглавлению](#Настройка-и-безопасность-сети)

## Как работает DHCP и как его настроить?

**DHCP (Dynamic Host Configuration Protocol)** — протокол, позволяющий автоматически назначать IP-адреса и другие сетевые параметры устройствам в сети.

### Процесс получения адреса (DORA)

1. **Discover** — клиент отправляет широковещательный запрос в сеть: «Есть ли тут DHCP-сервер?»
2. **Offer** — DHCP-сервер отвечает предложением IP-адреса и параметров.
3. **Request** — клиент запрашивает предложенный адрес.
4. **Acknowledge** — сервер подтверждает и выдаёт адрес на определённый срок (lease time).

### Настройка DHCP-клиента (Netplan)

```yaml
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
```

### Настройка DHCP-сервера (isc-dhcp-server)

```bash
sudo apt install isc-dhcp-server
```

```bash
# /etc/dhcp/dhcpd.conf
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option domain-name "mybank.local";
    default-lease-time 600;
    max-lease-time 7200;

    # Резервирование адреса для сервера приложений
    host app-server {
        hardware ethernet 00:11:22:33:44:55;
        fixed-address 192.168.1.10;
    }
}
```

### Полезные команды

```bash
# Посмотреть текущий lease
cat /var/lib/dhcp/dhclient.leases

# Обновить адрес
sudo dhclient -r eth0   # освободить
sudo dhclient eth0       # получить новый

# Посмотреть выданные адреса на сервере
cat /var/lib/dhcp/dhcpd.leases
```

В банковской среде DHCP обычно используется для рабочих станций, а серверы приложений всегда получают статические адреса или DHCP-резервацию по MAC-адресу.

[к оглавлению](#Настройка-и-безопасность-сети)

## Как настроить DNS в Linux?

**DNS (Domain Name System)** — система, преобразующая доменные имена в IP-адреса. Корректная настройка DNS критически важна для работы микросервисов, которые обращаются друг к другу по именам.

### /etc/resolv.conf

Классический способ указания DNS-серверов:

```bash
# /etc/resolv.conf
nameserver 10.0.0.1          # корпоративный DNS банка
nameserver 10.0.0.2          # резервный DNS
search mybank.local dev.mybank.local
options timeout:2 attempts:3
```

- `nameserver` — адрес DNS-сервера (максимум 3).
- `search` — домены для автодополнения (запрос `app-server` будет преобразован в `app-server.mybank.local`).
- `options` — параметры: таймаут и число попыток.

### systemd-resolved

В современных дистрибутивах `/etc/resolv.conf` часто является симлинком на systemd-resolved:

```bash
# Проверить статус
resolvectl status

# Настройка через /etc/systemd/resolved.conf
[Resolve]
DNS=10.0.0.1 10.0.0.2
FallbackDNS=8.8.8.8
Domains=mybank.local
DNSSEC=allow-downgrade
DNSOverTLS=opportunistic

# Применить
sudo systemctl restart systemd-resolved
```

### Локальный DNS через /etc/hosts

Файл `/etc/hosts` позволяет задать соответствие имён и адресов локально, без обращения к DNS:

```bash
# /etc/hosts
127.0.0.1       localhost
192.168.1.10    app-server.mybank.local app-server
192.168.1.11    db-master.mybank.local db-master
192.168.1.12    db-replica.mybank.local db-replica
192.168.1.20    kafka-01.mybank.local kafka-01
```

Порядок разрешения имён определяется в `/etc/nsswitch.conf`:

```bash
# /etc/nsswitch.conf
hosts: files dns
# Сначала /etc/hosts (files), потом DNS-сервер (dns)
```

### Диагностика DNS

```bash
# Запрос A-записи
dig app-server.mybank.local

# Краткий вывод
dig +short app-server.mybank.local

# Запрос конкретного типа записи
dig MX mybank.local
dig NS mybank.local

# Использование конкретного DNS-сервера
dig @10.0.0.1 app-server.mybank.local

# Альтернативный инструмент
nslookup app-server.mybank.local
```

Для Java-приложений DNS-кеширование управляется через JVM-параметры `networkaddress.cache.ttl` и `networkaddress.cache.negative.ttl` в файле `java.security`. По умолчанию JVM может кешировать DNS-записи навечно, что создаёт проблемы при изменении адресов.

[к оглавлению](#Настройка-и-безопасность-сети)

## Как настроить маршрутизацию в Linux?

Маршрутизация определяет, через какой шлюз и интерфейс отправлять пакеты к определённым адресам.

### Просмотр таблицы маршрутизации

```bash
# Текущие маршруты
ip route show

# Пример вывода:
# default via 192.168.1.1 dev eth0 proto static
# 192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100
# 10.10.0.0/16 via 192.168.1.254 dev eth0
```

### Управление маршрутами

```bash
# Добавить маршрут к подсети через шлюз
sudo ip route add 10.10.0.0/16 via 192.168.1.254

# Добавить маршрут через конкретный интерфейс
sudo ip route add 172.16.0.0/12 dev eth1

# Изменить шлюз по умолчанию (default gateway)
sudo ip route del default
sudo ip route add default via 192.168.1.1

# Удалить маршрут
sudo ip route del 10.10.0.0/16
```

### Постоянные маршруты (Netplan)

```yaml
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
        - to: 10.10.0.0/16
          via: 192.168.1.254
        - to: 172.16.0.0/12
          via: 192.168.1.253
          metric: 100
```

### Включение IP-forwarding (для маршрутизатора)

```bash
# Временно
sudo sysctl -w net.ipv4.ip_forward=1

# Постоянно
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

### Диагностика маршрутизации

```bash
# Какой маршрут будет использован для адреса
ip route get 10.10.5.20

# Трассировка маршрута
traceroute 10.10.5.20

# Более информативная трассировка с MTR
mtr 10.10.5.20
```

В банковской инфраструктуре маршрутизация часто используется для разделения трафика: production-трафик идёт через один шлюз, трафик к системам мониторинга — через другой, а административный доступ — через третий (VPN-шлюз).

[к оглавлению](#Настройка-и-безопасность-сети)

## Какие инструменты диагностики сети существуют и как ими пользоваться?

Умение диагностировать сетевые проблемы — ключевой навык для Java-разработчика, особенно при работе с микросервисами в банковской среде.

### ping — проверка доступности узла

```bash
# Базовая проверка
ping 192.168.1.1

# Ограничить количество пакетов
ping -c 4 app-server.mybank.local

# С указанием размера пакета (проверка MTU)
ping -s 1472 -M do 192.168.1.1
```

### traceroute / mtr — трассировка маршрута

```bash
# Стандартная трассировка
traceroute app-server.mybank.local

# TCP-трассировка (если ICMP заблокирован)
traceroute -T -p 8080 app-server.mybank.local

# MTR — комбинация ping и traceroute в реальном времени
mtr app-server.mybank.local

# MTR с отчётом (10 пакетов)
mtr -r -c 10 app-server.mybank.local
```

### dig / nslookup — диагностика DNS

```bash
# Полный DNS-запрос
dig app-server.mybank.local

# Обратный DNS-запрос (IP -> имя)
dig -x 192.168.1.10

# Трассировка DNS-разрешения
dig +trace app-server.mybank.local
```

### ss / netstat — анализ сетевых соединений

```bash
# Все TCP-соединения
ss -ta

# Прослушиваемые TCP-порты с именами процессов
ss -tlnp

# Соединения к конкретному порту
ss -tn dport = :8080

# Статистика соединений
ss -s

# Аналог через netstat (устаревший, но часто встречается)
netstat -tlnp
netstat -an | grep ESTABLISHED
```

### tcpdump — захват сетевого трафика

```bash
# Захват трафика на интерфейсе eth0
sudo tcpdump -i eth0

# Только трафик на порт 8080
sudo tcpdump -i eth0 port 8080

# HTTP-трафик с содержимым пакетов
sudo tcpdump -i eth0 -A port 80

# Запись в файл для анализа в Wireshark
sudo tcpdump -i eth0 -w capture.pcap port 443

# Фильтр по хосту
sudo tcpdump -i eth0 host 192.168.1.10

# Только SYN-пакеты (начало TCP-соединений)
sudo tcpdump -i eth0 'tcp[tcpflags] & tcp-syn != 0'
```

### Wireshark — графический анализатор трафика

Wireshark позволяет детально анализировать захваченный трафик. Основные фильтры:

```
# Фильтр по протоколу
http
tls
tcp

# Фильтр по IP
ip.addr == 192.168.1.10

# Фильтр по порту
tcp.port == 8080

# HTTP-запросы
http.request.method == "POST"

# TLS handshake
tls.handshake.type == 1
```

В банковской практике `tcpdump` и Wireshark незаменимы при отладке взаимодействия между сервисами — например, для диагностики проблем с TLS-соединениями или анализа задержек между микросервисами.

[к оглавлению](#Настройка-и-безопасность-сети)

## Как проверить прослушиваемые порты и доступность сервисов?

### Проверка прослушиваемых портов

```bash
# Все TCP-порты в состоянии LISTEN с именами процессов
sudo ss -tlnp

# Пример вывода:
# State   Recv-Q  Send-Q  Local Address:Port  Peer Address:Port  Process
# LISTEN  0       128     0.0.0.0:22           0.0.0.0:*         users:(("sshd",pid=1234,fd=3))
# LISTEN  0       100     0.0.0.0:8080         0.0.0.0:*         users:(("java",pid=5678,fd=45))
# LISTEN  0       128     127.0.0.1:5432       0.0.0.0:*         users:(("postgres",pid=910,fd=6))

# UDP-порты
sudo ss -ulnp

# Все порты (TCP + UDP)
sudo ss -tulnp

# Какой процесс занимает порт 8080
sudo lsof -i :8080
```

### Проверка доступности удалённых сервисов

```bash
# Проверка TCP-порта с помощью nc (netcat)
nc -zv app-server.mybank.local 8080
# Connection to app-server.mybank.local 8080 port [tcp/*] succeeded!

# Таймаут 3 секунды
nc -zv -w 3 db-master.mybank.local 5432

# Проверка через telnet
telnet app-server.mybank.local 8080

# Проверка доступности с помощью /dev/tcp (bash built-in)
echo > /dev/tcp/app-server.mybank.local/8080 && echo "Порт открыт" || echo "Порт закрыт"
```

### Сканирование портов с помощью nmap

```bash
# Сканирование конкретных портов
nmap -p 22,80,443,8080,5432 app-server.mybank.local

# Сканирование диапазона портов
nmap -p 1-1024 app-server.mybank.local

# Определение сервисов и версий
nmap -sV -p 22,8080,5432 app-server.mybank.local

# Быстрое сканирование подсети
nmap -sn 192.168.1.0/24

# Сканирование с определением ОС
sudo nmap -O app-server.mybank.local
```

### Важно для Java-разработчика

При запуске Spring Boot приложения полезно проверять:

```bash
# Убедиться, что приложение слушает нужный порт
ss -tlnp | grep 8080

# Проверить, что приложение привязано к правильному адресу
# 0.0.0.0:8080 — слушает на всех интерфейсах
# 127.0.0.1:8080 — только локально (не доступно извне!)
```

В Spring Boot это настраивается через:

```properties
server.address=0.0.0.0
server.port=8080
```

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое iptables и как работает фильтрация трафика?

**iptables** — утилита для управления встроенным в ядро Linux межсетевым экраном (Netfilter). Она позволяет задавать правила фильтрации, NAT и модификации сетевых пакетов.

### Таблицы (tables)

| Таблица | Назначение |
|---------|-----------|
| **filter** | Фильтрация пакетов (разрешить/запретить). Используется по умолчанию |
| **nat** | Трансляция сетевых адресов (SNAT, DNAT, MASQUERADE) |
| **mangle** | Модификация заголовков пакетов (TTL, TOS, маркировка) |
| **raw** | Обработка пакетов до отслеживания соединений (conntrack) |

### Цепочки (chains) таблицы filter

| Цепочка | Когда применяется |
|---------|-------------------|
| **INPUT** | Входящие пакеты, адресованные этому серверу |
| **OUTPUT** | Исходящие пакеты от этого сервера |
| **FORWARD** | Пакеты, проходящие через сервер транзитом (маршрутизация) |

### Основные команды

```bash
# Просмотр всех правил
sudo iptables -L -n -v

# Просмотр правил с номерами строк
sudo iptables -L -n --line-numbers

# Просмотр правил таблицы nat
sudo iptables -t nat -L -n -v
```

### Примеры правил фильтрации

```bash
# Разрешить входящие SSH-подключения
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Разрешить входящий трафик на порт 8080 (Java-приложение)
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT

# Разрешить входящие подключения только из определённой подсети
sudo iptables -A INPUT -s 10.10.0.0/16 -p tcp --dport 5432 -j ACCEPT

# Запретить все остальные входящие подключения к PostgreSQL
sudo iptables -A INPUT -p tcp --dport 5432 -j DROP

# Разрешить loopback-трафик
sudo iptables -A INPUT -i lo -j ACCEPT

# Разрешить уже установленные и связанные соединения
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Запретить весь остальной входящий трафик (должно быть последним правилом)
sudo iptables -P INPUT DROP
```

### Пример базовой конфигурации для сервера приложений банка

```bash
# Сброс всех правил
sudo iptables -F
sudo iptables -X

# Политики по умолчанию: всё запретить
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Разрешить loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Разрешить установленные соединения
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# SSH только из управляющей подсети
sudo iptables -A INPUT -s 10.0.1.0/24 -p tcp --dport 22 -j ACCEPT

# HTTP/HTTPS от балансировщика нагрузки
sudo iptables -A INPUT -s 10.0.10.0/24 -p tcp --dport 8080 -j ACCEPT

# Мониторинг (Prometheus metrics)
sudo iptables -A INPUT -s 10.0.20.0/24 -p tcp --dport 9090 -j ACCEPT

# Логирование заблокированных пакетов
sudo iptables -A INPUT -j LOG --log-prefix "IPTABLES-DROP: " --log-level 4
sudo iptables -A INPUT -j DROP
```

### NAT — трансляция адресов

```bash
# Перенаправление порта 80 на внутренний сервер 192.168.1.10:8080 (DNAT)
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.10:8080

# Маскарадинг для выхода в интернет через NAT
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

### Сохранение правил

```bash
# Установить пакет для сохранения
sudo apt install iptables-persistent

# Сохранить текущие правила
sudo netfilter-persistent save

# Файлы правил
# /etc/iptables/rules.v4
# /etc/iptables/rules.v6
```

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое ufw и как им пользоваться?

**UFW (Uncomplicated Firewall)** — надстройка над iptables, предоставляющая упрощённый интерфейс для управления межсетевым экраном.

### Основные команды

```bash
# Включить/выключить
sudo ufw enable
sudo ufw disable

# Статус и все правила
sudo ufw status verbose

# Политики по умолчанию
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Управление правилами

```bash
# Разрешить порт
sudo ufw allow 22/tcp
sudo ufw allow 8080/tcp

# Разрешить из конкретной подсети
sudo ufw allow from 10.0.1.0/24 to any port 22

# Разрешить диапазон портов
sudo ufw allow 8000:9000/tcp

# Запретить порт
sudo ufw deny 3306/tcp

# Удалить правило
sudo ufw delete allow 8080/tcp

# Использование профилей приложений
sudo ufw app list
sudo ufw allow 'Nginx Full'
sudo ufw allow 'OpenSSH'
```

### Пример настройки для Java-сервера

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from 10.0.1.0/24 to any port 22 proto tcp    # SSH из управляющей сети
sudo ufw allow from 10.0.10.0/24 to any port 8080 proto tcp  # Приложение от LB
sudo ufw allow from 10.0.20.0/24 to any port 9090 proto tcp  # Метрики Prometheus
sudo ufw enable
```

UFW проще iptables и подходит для типовых конфигураций. В банковской среде для сложных правил (NAT, mangle) всё равно используется iptables или nftables.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое DMZ и зачем она нужна?

**DMZ (Demilitarized Zone, демилитаризованная зона)** — изолированный сегмент сети, расположенный между внешней (интернет) и внутренней (корпоративной) сетями. DMZ содержит сервисы, которые должны быть доступны извне, но при этом не имеют прямого доступа к внутренней сети.

### Типичная архитектура DMZ в банке

```
Интернет
    │
[Внешний Firewall]
    │
┌───────────────────────────── DMZ ──────────────────────────────┐
│  Web-сервер (Nginx)    API Gateway    WAF (Web App Firewall)   │
│  Reverse Proxy         Load Balancer  Публичное API            │
└────────────────────────────────────────────────────────────────┘
    │
[Внутренний Firewall]
    │
┌──────────────────── Внутренняя сеть ───────────────────────────┐
│  Application Servers   Базы данных   Kafka   Внутренние API    │
│  Spring Boot apps      PostgreSQL    Redis   Системы ДБО       │
└────────────────────────────────────────────────────────────────┘
```

### Правила для DMZ

1. **Интернет -> DMZ** — разрешён только определённый трафик (80, 443).
2. **DMZ -> Внутренняя сеть** — разрешён только необходимый трафик (конкретные порты к конкретным серверам).
3. **Внутренняя сеть -> DMZ** — допускается для управления и мониторинга.
4. **Интернет -> Внутренняя сеть** — полностью запрещён.

### Что размещается в DMZ

- **Reverse proxy / Load Balancer** (Nginx, HAProxy) — принимает запросы из интернета и проксирует их во внутреннюю сеть.
- **WAF (Web Application Firewall)** — фильтрует вредоносные HTTP-запросы.
- **API Gateway** — точка входа для публичных API банка.
- **DNS-серверы** — для обслуживания внешних запросов.
- **Почтовые серверы** — для приёма внешней почты.

### Пример правил iptables для DMZ

```bash
# Разрешить трафик из интернета в DMZ только на 80 и 443
iptables -A FORWARD -i eth0 -o eth1 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -i eth0 -o eth1 -p tcp --dport 443 -j ACCEPT

# Разрешить DMZ -> внутренняя сеть только на порт приложения
iptables -A FORWARD -i eth1 -o eth2 -d 10.0.100.10 -p tcp --dport 8080 -j ACCEPT

# Запретить прямой доступ из интернета во внутреннюю сеть
iptables -A FORWARD -i eth0 -o eth2 -j DROP
```

В банках DMZ — обязательное требование регуляторов (PCI DSS). Java-разработчик должен понимать, в каком сегменте работает его приложение, чтобы правильно настраивать подключения к другим сервисам.

[к оглавлению](#Настройка-и-безопасность-сети)

## Как работает TLS/SSL и что такое цепочка доверия сертификатов?

**TLS (Transport Layer Security)** — криптографический протокол, обеспечивающий конфиденциальность и целостность данных при передаче по сети. SSL — устаревший предшественник TLS (SSL 3.0 -> TLS 1.0). В банковской среде используется TLS 1.2 и TLS 1.3.

### Процесс TLS Handshake (TLS 1.2)

```
Клиент                                          Сервер
   │                                               │
   │──── ClientHello (версии TLS, cipher suites) ──>│
   │                                               │
   │<─── ServerHello (выбранный cipher suite) ──────│
   │<─── Certificate (сертификат сервера) ──────────│
   │<─── ServerKeyExchange (параметры DH) ──────────│
   │<─── ServerHelloDone ──────────────────────────│
   │                                               │
   │──── ClientKeyExchange (предмастер-секрет) ───>│
   │──── ChangeCipherSpec ─────────────────────────>│
   │──── Finished (зашифровано) ───────────────────>│
   │                                               │
   │<─── ChangeCipherSpec ─────────────────────────│
   │<─── Finished (зашифровано) ───────────────────│
   │                                               │
   │<═══ Зашифрованный обмен данными ══════════════>│
```

В TLS 1.3 handshake сокращён до одного round-trip (1-RTT), что значительно ускоряет установку соединения.

### Цепочка доверия сертификатов

```
Root CA (корневой центр сертификации)
  │ подписывает
  ▼
Intermediate CA (промежуточный центр сертификации)
  │ подписывает
  ▼
Leaf Certificate (сертификат сервера / конечного субъекта)
```

- **Root CA** — самоподписанный сертификат, хранится в хранилище доверенных сертификатов ОС/браузера.
- **Intermediate CA** — промежуточный сертификат, подписанный Root CA.
- **Leaf Certificate** — сертификат конкретного сервера, подписанный Intermediate CA.

Клиент проверяет всю цепочку: leaf -> intermediate -> root. Если root CA присутствует в хранилище доверенных сертификатов — соединение доверенное.

### Типы сертификатов

| Тип | Описание | Применение |
|-----|----------|-----------|
| **Самоподписанный** | Подписан собственным ключом | Разработка, тестирование |
| **Let's Encrypt** | Бесплатный, автоматически обновляемый | Публичные сервисы |
| **Коммерческий CA** | Выдаётся DigiCert, GlobalSign и др. | Банковские продуктивные среды |
| **Внутренний CA** | Собственный CA банка | Внутренние сервисы, mTLS |

### Создание самоподписанного сертификата

```bash
# Генерация ключа и сертификата
openssl req -x509 -newkey rsa:4096 -keyout server.key -out server.crt \
  -days 365 -nodes -subj "/CN=app-server.mybank.local"

# Просмотр содержимого сертификата
openssl x509 -in server.crt -text -noout

# Проверка цепочки сертификатов удалённого сервера
openssl s_client -connect app-server.mybank.local:443 -showcerts
```

### Получение сертификата Let's Encrypt

```bash
# Установка certbot
sudo apt install certbot python3-certbot-nginx

# Получение сертификата
sudo certbot --nginx -d api.mybank.com

# Автоматическое продление (добавляется в cron автоматически)
sudo certbot renew --dry-run
```

### Работа с сертификатами в Java

Java использует хранилища ключей (KeyStore) и доверенных сертификатов (TrustStore):

```bash
# Импорт сертификата в TrustStore
keytool -import -alias mybank-ca -file ca.crt \
  -keystore truststore.jks -storepass changeit

# Просмотр содержимого KeyStore
keytool -list -v -keystore keystore.jks -storepass changeit

# Конвертация PEM -> PKCS12 для Java
openssl pkcs12 -export -in server.crt -inkey server.key \
  -out keystore.p12 -name server -CAfile ca.crt -caname root
```

Настройка в Spring Boot:

```yaml
server:
  ssl:
    key-store: classpath:keystore.p12
    key-store-password: ${SSL_KEYSTORE_PASSWORD}
    key-store-type: PKCS12
    enabled: true
    protocol: TLS
    enabled-protocols: TLSv1.3,TLSv1.2
```

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое mTLS и зачем нужна двусторонняя аутентификация?

**mTLS (Mutual TLS)** — расширение TLS, при котором не только клиент проверяет сертификат сервера, но и сервер проверяет сертификат клиента. Обе стороны аутентифицируют друг друга.

### Отличие от обычного TLS

| Параметр | TLS | mTLS |
|----------|-----|------|
| Сервер предъявляет сертификат | Да | Да |
| Клиент предъявляет сертификат | Нет | Да |
| Аутентификация сервера | Да | Да |
| Аутентификация клиента | Нет (другие механизмы) | Да (на уровне TLS) |

### Процесс mTLS Handshake

```
Клиент                                          Сервер
   │──── ClientHello ──────────────────────────────>│
   │<─── ServerHello + Certificate ─────────────────│
   │<─── CertificateRequest ───────────────────────│  ← сервер запрашивает
   │<─── ServerHelloDone ──────────────────────────│     сертификат клиента
   │──── Certificate (сертификат клиента) ─────────>│  ← клиент предъявляет
   │──── ClientKeyExchange ────────────────────────>│     свой сертификат
   │──── CertificateVerify ────────────────────────>│
   │──── Finished ─────────────────────────────────>│
   │<─── Finished ─────────────────────────────────│
```

### Когда используется mTLS

- **Межсервисное взаимодействие** — микросервисы аутентифицируют друг друга.
- **Банковские интеграции** — взаимодействие с платёжными системами (SWIFT, НСПК).
- **API для партнёров** — контрагенты банка подключаются с использованием клиентских сертификатов.
- **Service Mesh** — Istio/Linkerd автоматически обеспечивают mTLS между подами.

### Настройка mTLS в Spring Boot

```yaml
server:
  ssl:
    key-store: classpath:server-keystore.p12
    key-store-password: ${SSL_KEYSTORE_PASSWORD}
    key-store-type: PKCS12
    trust-store: classpath:truststore.jks
    trust-store-password: ${SSL_TRUSTSTORE_PASSWORD}
    client-auth: need  # need = обязательно, want = опционально
```

### Настройка RestTemplate / WebClient для mTLS

```java
@Bean
public RestTemplate restTemplate() throws Exception {
    KeyStore keyStore = KeyStore.getInstance("PKCS12");
    keyStore.load(new FileInputStream("client-keystore.p12"),
                  "password".toCharArray());

    KeyStore trustStore = KeyStore.getInstance("JKS");
    trustStore.load(new FileInputStream("truststore.jks"),
                    "password".toCharArray());

    SSLContext sslContext = SSLContextBuilder.create()
        .loadKeyMaterial(keyStore, "password".toCharArray())
        .loadTrustMaterial(trustStore, null)
        .build();

    HttpClient httpClient = HttpClients.custom()
        .setSSLContext(sslContext)
        .build();

    return new RestTemplateBuilder()
        .requestFactory(() -> new HttpComponentsClientHttpRequestFactory(httpClient))
        .build();
}
```

В банковской среде mTLS является стандартом для критичных интеграций, так как обеспечивает взаимную аутентификацию сторон на уровне транспорта ещё до обработки бизнес-логики.

[к оглавлению](#Настройка-и-безопасность-сети)

## Как настроить HTTPS в Nginx с редиректом с HTTP?

### Базовая конфигурация HTTPS

```nginx
# /etc/nginx/sites-available/mybank-api
server {
    listen 443 ssl http2;
    server_name api.mybank.com;

    # Сертификаты
    ssl_certificate     /etc/nginx/ssl/api.mybank.com.crt;
    ssl_certificate_key /etc/nginx/ssl/api.mybank.com.key;

    # Цепочка промежуточных сертификатов
    ssl_trusted_certificate /etc/nginx/ssl/ca-chain.crt;

    # Протоколы и шифры
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers on;

    # Оптимизация SSL
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    ssl_session_tickets off;

    # OCSP Stapling
    ssl_stapling on;
    ssl_stapling_verify on;

    # Заголовки безопасности
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options nosniff;
    add_header X-Frame-Options DENY;
    add_header X-XSS-Protection "1; mode=block";

    # Проксирование на Java-приложение
    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Редирект с HTTP на HTTPS

```nginx
server {
    listen 80;
    server_name api.mybank.com;

    # Редирект всех HTTP-запросов на HTTPS
    return 301 https://$host$request_uri;
}
```

### Настройка с Let's Encrypt

```bash
# Получить сертификат и автоматически настроить Nginx
sudo certbot --nginx -d api.mybank.com

# Проверка конфигурации
sudo nginx -t

# Перезагрузка
sudo systemctl reload nginx
```

### Проверка настроек SSL

```bash
# Проверка соединения
openssl s_client -connect api.mybank.com:443

# Проверка поддерживаемых протоколов
nmap --script ssl-enum-ciphers -p 443 api.mybank.com

# Онлайн-проверка (если сервер публичный)
# https://www.ssllabs.com/ssltest/
```

В Spring Boot приложении за Nginx необходимо настроить доверие к заголовкам `X-Forwarded-*`:

```yaml
server:
  forward-headers-strategy: framework  # или native
```

[к оглавлению](#Настройка-и-безопасность-сети)

## Как усилить безопасность SSH-сервера?

SSH — основной инструмент удалённого доступа к серверам. Для банковской среды базовых настроек недостаточно — требуется hardening.

### 1. Отключение входа под root

```bash
# /etc/ssh/sshd_config
PermitRootLogin no
```

### 2. Только ключевая аутентификация (отключение паролей)

```bash
# Генерация ключей на клиенте
ssh-keygen -t ed25519 -C "developer@mybank.local"

# Копирование публичного ключа на сервер
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@app-server.mybank.local

# /etc/ssh/sshd_config
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
UsePAM no
```

### 3. Смена порта SSH

```bash
# /etc/ssh/sshd_config
Port 2222
```

### 4. Ограничение пользователей и источников

```bash
# /etc/ssh/sshd_config
AllowUsers deploy@10.0.1.0/24 admin@10.0.1.0/24
AllowGroups sshusers

# Максимальное количество попыток аутентификации
MaxAuthTries 3

# Время на аутентификацию
LoginGraceTime 30

# Запрет пустых паролей
PermitEmptyPasswords no
```

### 5. Дополнительные настройки hardening

```bash
# /etc/ssh/sshd_config

# Только протокол версии 2
Protocol 2

# Отключить X11 forwarding
X11Forwarding no

# Отключить агент-forwarding (если не нужен)
AllowAgentForwarding no

# Таймаут неактивности (300 секунд)
ClientAliveInterval 300
ClientAliveCountMax 2

# Лог-уровень
LogLevel VERBOSE

# Баннер перед аутентификацией
Banner /etc/ssh/banner
```

### 6. Установка и настройка fail2ban

fail2ban блокирует IP-адреса после нескольких неудачных попыток входа:

```bash
sudo apt install fail2ban
```

```ini
# /etc/fail2ban/jail.local
[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600        # 1 час блокировки
findtime = 600        # Окно обнаружения: 10 минут
ignoreip = 10.0.1.0/24  # Не блокировать управляющую сеть
```

```bash
# Управление fail2ban
sudo systemctl enable fail2ban
sudo fail2ban-client status sshd
sudo fail2ban-client set sshd unbanip 192.168.1.50
```

### 7. Применение изменений

```bash
# Проверка конфигурации
sudo sshd -t

# Перезапуск sshd
sudo systemctl restart sshd
```

Важно: при изменении настроек SSH всегда держите открытой текущую сессию и проверяйте возможность нового подключения в отдельном терминале, чтобы не потерять доступ к серверу.

[к оглавлению](#Настройка-и-безопасность-сети)

## Какие существуют методы защиты от DDoS-атак?

**DDoS (Distributed Denial of Service)** — распределённая атака на отказ в обслуживании, когда множество источников одновременно генерируют трафик к целевому серверу, делая его недоступным для легитимных пользователей. Для банков DDoS-атаки — одна из самых распространённых угроз.

### Уровни защиты

### 1. Защита на уровне ядра Linux (SYN Cookies)

SYN flood — атака, при которой злоумышленник отправляет огромное количество SYN-пакетов, не завершая TCP handshake, исчерпывая ресурсы сервера.

```bash
# Включить SYN cookies
sudo sysctl -w net.ipv4.tcp_syncookies=1

# Увеличить очередь полуоткрытых соединений
sudo sysctl -w net.ipv4.tcp_max_syn_backlog=65536

# Уменьшить количество повторных отправок SYN-ACK
sudo sysctl -w net.ipv4.tcp_synack_retries=2

# Сохранить постоянно
cat >> /etc/sysctl.conf << EOF
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 65536
net.ipv4.tcp_synack_retries = 2
net.core.somaxconn = 65535
EOF
sudo sysctl -p
```

### 2. Rate Limiting на уровне iptables

```bash
# Ограничение новых соединений: не более 25 в секунду
sudo iptables -A INPUT -p tcp --dport 443 -m connlimit --connlimit-above 100 -j DROP
sudo iptables -A INPUT -p tcp --dport 443 -m limit --limit 25/sec --limit-burst 50 -j ACCEPT

# Блокировка IP с превышением лимита
sudo iptables -A INPUT -p tcp --dport 443 -m recent --name DDOS --rcheck --seconds 60 --hitcount 100 -j DROP
sudo iptables -A INPUT -p tcp --dport 443 -m recent --name DDOS --set -j ACCEPT
```

### 3. Rate Limiting в Nginx

```nginx
# Определение зоны ограничения (10 запросов в секунду на IP)
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;
limit_conn_zone $binary_remote_addr zone=conn_limit:10m;

server {
    location /api/ {
        # Ограничение запросов с буфером 20
        limit_req zone=api_limit burst=20 nodelay;

        # Ограничение одновременных соединений
        limit_conn conn_limit 10;

        # Код ответа при превышении лимита
        limit_req_status 429;

        proxy_pass http://backend;
    }
}
```

### 4. Rate Limiting в Spring Boot

```java
// Использование bucket4j для rate limiting
@Bean
public FilterRegistrationBean<RateLimitFilter> rateLimitFilter() {
    Bandwidth limit = Bandwidth.classic(100,
        Refill.greedy(100, Duration.ofMinutes(1)));
    Bucket bucket = Bucket.builder().addLimit(limit).build();
    // ... настройка фильтра
}
```

### 5. Внешние сервисы защиты от DDoS

- **Cloudflare / AWS Shield / Akamai** — фильтрация трафика на уровне CDN.
- **Анти-DDoS от провайдера** — фильтрация на уровне канала связи.
- **WAF (Web Application Firewall)** — фильтрация на уровне приложения.

В банковской инфраструктуре обычно используется многоуровневая защита: внешний Anti-DDoS-сервис + WAF + rate limiting на Nginx/API Gateway + защита на уровне приложения.

[к оглавлению](#Настройка-и-безопасность-сети)

## Какие основные атаки на сеть существуют и как от них защищаться?

### 1. MITM (Man-in-the-Middle) — атака «человек посередине»

**Суть:** злоумышленник встраивается между двумя сторонами коммуникации, перехватывая и/или модифицируя трафик.

```
Клиент ──── [Злоумышленник] ──── Сервер
         перехватывает трафик
```

**Защита:**
- Использование TLS/HTTPS для всех соединений.
- Certificate Pinning (привязка к конкретному сертификату).
- HSTS (HTTP Strict Transport Security) — браузер запоминает, что сайт должен открываться только по HTTPS.
- Проверка сертификатов (не игнорировать ошибки сертификатов в коде!).

```java
// НЕПРАВИЛЬНО — отключение проверки сертификатов (часто в тестах)
TrustManager[] trustAllCerts = new TrustManager[]{
    new X509TrustManager() {
        public void checkClientTrusted(...) {} // ОПАСНО!
        public void checkServerTrusted(...) {} // ОПАСНО!
    }
};

// ПРАВИЛЬНО — использование корректного TrustStore
SSLContext sslContext = SSLContextBuilder.create()
    .loadTrustMaterial(trustStore, null) // проверяем CA
    .build();
```

### 2. ARP Spoofing — подмена ARP-таблицы

**Суть:** злоумышленник отправляет ложные ARP-ответы, связывая свой MAC-адрес с IP-адресом шлюза. Весь трафик начинает проходить через него.

**Защита:**
- Статические ARP-записи для критичных устройств.
- Dynamic ARP Inspection (DAI) на коммутаторах.
- Сегментация сети (VLAN).
- Мониторинг ARP-таблиц: `arpwatch`.

```bash
# Статическая ARP-запись
sudo arp -s 192.168.1.1 00:11:22:33:44:55

# Мониторинг ARP
sudo apt install arpwatch
sudo arpwatch -i eth0
```

### 3. DNS Spoofing — подмена DNS-ответов

**Суть:** злоумышленник подменяет DNS-ответы, направляя пользователя на поддельный сервер.

**Защита:**
- DNSSEC — криптографическая подпись DNS-записей.
- DNS over HTTPS (DoH) / DNS over TLS (DoT).
- Использование проверенных DNS-серверов.
- Мониторинг DNS-запросов.

### 4. SYN Flood

**Суть:** злоумышленник отправляет огромное количество TCP SYN-пакетов с поддельными IP-адресами. Сервер выделяет ресурсы на каждое полуоткрытое соединение и исчерпывает их.

**Защита:**
```bash
# SYN cookies
sysctl -w net.ipv4.tcp_syncookies=1

# Лимит SYN-пакетов через iptables
iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
iptables -A INPUT -p tcp --syn -j DROP
```

### 5. TCP Session Hijacking — захват сессии

**Суть:** злоумышленник перехватывает существующую TCP-сессию, зная sequence numbers.

**Защита:**
- Шифрование трафика (TLS).
- Рандомизация начальных sequence numbers (современные ОС делают это по умолчанию).

### Общие рекомендации для банковской среды

1. **Шифруйте всё** — TLS для всех соединений, включая внутренние.
2. **Сегментируйте сеть** — VLAN, подсети, файрволы между сегментами.
3. **Мониторьте** — IDS/IPS (Snort, Suricata), SIEM-системы.
4. **Обновляйте** — патчи безопасности, актуальные версии протоколов.
5. **Принцип минимальных привилегий** — открывайте только необходимые порты и протоколы.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое OWASP Top 10 и какие уязвимости наиболее актуальны для Java-разработчика?

**OWASP (Open Web Application Security Project)** — международная организация, занимающаяся безопасностью веб-приложений. **OWASP Top 10** — регулярно обновляемый список наиболее критичных уязвимостей.

### OWASP Top 10 (2021)

### A01: Broken Access Control (Нарушение контроля доступа)

Пользователь получает доступ к ресурсам, которые ему не положены.

```java
// УЯЗВИМЫЙ КОД: нет проверки владельца
@GetMapping("/api/accounts/{id}")
public Account getAccount(@PathVariable Long id) {
    return accountRepository.findById(id).orElseThrow();
}

// БЕЗОПАСНЫЙ КОД: проверка принадлежности
@GetMapping("/api/accounts/{id}")
public Account getAccount(@PathVariable Long id, Authentication auth) {
    Account account = accountRepository.findById(id).orElseThrow();
    if (!account.getOwner().equals(auth.getName())) {
        throw new AccessDeniedException("Нет доступа к чужому счёту");
    }
    return account;
}
```

### A02: Cryptographic Failures (Криптографические ошибки)

Слабое шифрование, хранение паролей в открытом виде.

```java
// НЕПРАВИЛЬНО
String hashedPassword = DigestUtils.md5Hex(password); // MD5 небезопасен!

// ПРАВИЛЬНО
String hashedPassword = new BCryptPasswordEncoder(12).encode(password);
```

### A03: Injection (Внедрение)

**SQL Injection:**

```java
// УЯЗВИМЫЙ КОД
String query = "SELECT * FROM users WHERE login = '" + userInput + "'";
// Ввод: ' OR '1'='1' -- → возвращает всех пользователей

// БЕЗОПАСНЫЙ КОД: параметризованные запросы
@Query("SELECT u FROM User u WHERE u.login = :login")
User findByLogin(@Param("login") String login);

// Или через PreparedStatement
PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE login = ?");
ps.setString(1, userInput);
```

### A04: Insecure Design (Небезопасный дизайн)

Отсутствие лимитов на операции, недостаточная валидация бизнес-логики.

```java
// НЕБЕЗОПАСНО: нет лимита на перебор кодов подтверждения
@PostMapping("/api/confirm-transfer")
public void confirm(@RequestBody ConfirmRequest request) {
    if (request.getCode().equals(expectedCode)) { ... }
}

// БЕЗОПАСНО: ограничение попыток
@PostMapping("/api/confirm-transfer")
public void confirm(@RequestBody ConfirmRequest request) {
    if (attemptCounter.incrementAndGet(request.getTransferId()) > 3) {
        throw new TooManyAttemptsException("Превышен лимит попыток");
    }
    if (!request.getCode().equals(expectedCode)) {
        throw new InvalidCodeException("Неверный код");
    }
    // ...
}
```

### A05: Security Misconfiguration (Неправильная настройка безопасности)

```yaml
# НЕПРАВИЛЬНО: стандартные настройки Spring Boot в production
spring:
  h2:
    console:
      enabled: true     # Консоль H2 доступна!
management:
  endpoints:
    web:
      exposure:
        include: "*"    # Все actuator-эндпоинты открыты!

# ПРАВИЛЬНО
management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  endpoint:
    health:
      show-details: when-authorized
```

### A07: Cross-Site Scripting (XSS)

```java
// УЯЗВИМО: вставка пользовательского ввода без экранирования
model.addAttribute("name", userInput);
// В шаблоне: <p th:utext="${name}"></p> — utext не экранирует!

// БЕЗОПАСНО: использовать th:text, которое экранирует HTML
// <p th:text="${name}"></p>
```

### A08: Cross-Site Request Forgery (CSRF)

```java
// Spring Security: CSRF включён по умолчанию для stateful-приложений
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf(csrf -> csrf
            .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
        );
        // Для REST API с JWT — CSRF можно отключить
        // http.csrf(csrf -> csrf.disable());
        return http.build();
    }
}
```

### A10: Server-Side Request Forgery (SSRF)

```java
// УЯЗВИМО: пользователь указывает URL для запроса
@GetMapping("/api/fetch")
public String fetch(@RequestParam String url) {
    return restTemplate.getForObject(url, String.class);
    // Злоумышленник: /api/fetch?url=http://169.254.169.254/latest/meta-data/
}

// БЕЗОПАСНО: белый список доменов
private static final Set<String> ALLOWED_HOSTS = Set.of(
    "api.cbr.ru", "api.partner.com"
);

@GetMapping("/api/fetch")
public String fetch(@RequestParam String url) {
    URI uri = URI.create(url);
    if (!ALLOWED_HOSTS.contains(uri.getHost())) {
        throw new SecurityException("Запрещённый хост");
    }
    return restTemplate.getForObject(url, String.class);
}
```

В банке знание OWASP Top 10 — обязательное требование. Код проходит security review и статический анализ (SonarQube, Checkmarx) перед выходом в production.

[к оглавлению](#Настройка-и-безопасность-сети)

## Как работает CORS и как его настроить?

**CORS (Cross-Origin Resource Sharing)** — механизм, позволяющий браузеру делать запросы к серверу, расположенному на другом домене (origin). По умолчанию браузер блокирует такие запросы из соображений безопасности (Same-Origin Policy).

### Origin (источник)

Origin состоит из трёх частей: **протокол + домен + порт**.

```
https://app.mybank.com:443  — один origin
https://api.mybank.com:443  — другой origin (другой домен)
http://app.mybank.com:80    — другой origin (другой протокол и порт)
```

### Простые и preflight-запросы

**Простой запрос** (не требует preflight):
- Метод: GET, HEAD, POST.
- Заголовки: только стандартные (Accept, Content-Type с типом `text/plain`, `multipart/form-data`, `application/x-www-form-urlencoded`).

**Preflight-запрос** (OPTIONS) выполняется перед основным запросом, если:
- Метод: PUT, DELETE, PATCH.
- Нестандартные заголовки (например, `Authorization`).
- `Content-Type: application/json`.

```
Браузер                                            Сервер
   │                                                  │
   │──── OPTIONS /api/transfer ──────────────────────>│
   │     Origin: https://app.mybank.com               │
   │     Access-Control-Request-Method: POST           │
   │     Access-Control-Request-Headers: Authorization │
   │                                                  │
   │<─── 200 OK ─────────────────────────────────────│
   │     Access-Control-Allow-Origin: https://app.mybank.com
   │     Access-Control-Allow-Methods: GET,POST,PUT   │
   │     Access-Control-Allow-Headers: Authorization   │
   │     Access-Control-Max-Age: 3600                  │
   │                                                  │
   │──── POST /api/transfer ─────────────────────────>│
   │     Origin: https://app.mybank.com               │
   │     Authorization: Bearer eyJhbG...              │
   │                                                  │
   │<─── 200 OK ─────────────────────────────────────│
   │     Access-Control-Allow-Origin: https://app.mybank.com
```

### Настройка CORS в Spring Boot

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
            .allowedOrigins(
                "https://app.mybank.com",
                "https://admin.mybank.com"
            )
            .allowedMethods("GET", "POST", "PUT", "DELETE")
            .allowedHeaders("Authorization", "Content-Type", "X-Request-Id")
            .exposedHeaders("X-Total-Count")
            .allowCredentials(true)       // разрешить отправку cookies
            .maxAge(3600);                // кеширование preflight на 1 час
    }
}
```

Или через Spring Security:

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.cors(cors -> cors.configurationSource(request -> {
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowedOrigins(List.of("https://app.mybank.com"));
        config.setAllowedMethods(List.of("GET", "POST", "PUT", "DELETE"));
        config.setAllowedHeaders(List.of("Authorization", "Content-Type"));
        config.setAllowCredentials(true);
        return config;
    }));
    return http.build();
}
```

### Настройка CORS в Nginx

```nginx
location /api/ {
    if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' 'https://app.mybank.com';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type';
        add_header 'Access-Control-Max-Age' 3600;
        return 204;
    }

    add_header 'Access-Control-Allow-Origin' 'https://app.mybank.com' always;
    add_header 'Access-Control-Allow-Credentials' 'true' always;

    proxy_pass http://backend;
}
```

### Важные правила безопасности

- **Никогда не используйте `*` в `Access-Control-Allow-Origin` для банковских приложений** — указывайте конкретные домены.
- `allowCredentials(true)` несовместим с `allowedOrigins("*")` — это требование спецификации.
- Preflight-ответ кешируется на стороне браузера в течение `Access-Control-Max-Age` секунд.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое JWT, какова его структура и лучшие практики использования?

**JWT (JSON Web Token)** — компактный и самодостаточный токен для безопасной передачи информации между сторонами в формате JSON. Широко используется для аутентификации и авторизации в REST API.

### Структура JWT

JWT состоит из трёх частей, разделённых точкой: `header.payload.signature`

```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiJ1c2VyMTIzIiwicm9sZSI6Ik1BTkFHRVIiLCJpYXQiOjE3MDAw
MDAwMDAsImV4cCI6MTcwMDAwMzYwMH0.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**Header** (заголовок) — алгоритм подписи и тип токена:

```json
{
  "alg": "RS256",
  "typ": "JWT"
}
```

**Payload** (полезная нагрузка) — утверждения (claims):

```json
{
  "sub": "user123",
  "name": "Иван Петров",
  "role": "MANAGER",
  "department": "credit",
  "iat": 1700000000,
  "exp": 1700003600,
  "iss": "auth.mybank.com"
}
```

Стандартные claims: `sub` (субъект), `iss` (издатель), `exp` (срок действия), `iat` (время выдачи), `aud` (аудитория), `jti` (уникальный идентификатор).

**Signature** (подпись):

```
RSASHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  privateKey
)
```

### Алгоритмы подписи

| Алгоритм | Тип | Описание |
|----------|-----|----------|
| **HS256** | Симметричный | HMAC + SHA-256, один секретный ключ для подписи и проверки |
| **RS256** | Асимметричный | RSA + SHA-256, приватный ключ для подписи, публичный для проверки |
| **ES256** | Асимметричный | ECDSA + SHA-256, компактнее RSA при той же надёжности |

Для банковских систем рекомендуется **RS256** или **ES256** — публичный ключ можно безопасно распространять между сервисами.

### Реализация в Spring Boot

```java
// Генерация JWT
public String generateToken(UserDetails user) {
    return Jwts.builder()
        .setSubject(user.getUsername())
        .claim("role", user.getAuthorities())
        .setIssuedAt(new Date())
        .setExpiration(new Date(System.currentTimeMillis() + 3600_000)) // 1 час
        .setIssuer("auth.mybank.com")
        .setId(UUID.randomUUID().toString()) // jti для предотвращения replay-атак
        .signWith(privateKey, SignatureAlgorithm.RS256)
        .compact();
}

// Валидация JWT
public Claims validateToken(String token) {
    return Jwts.parserBuilder()
        .setSigningKey(publicKey)
        .requireIssuer("auth.mybank.com")
        .build()
        .parseClaimsJws(token)
        .getBody();
}
```

### Хранение токенов на клиенте

| Хранилище | Плюсы | Минусы |
|-----------|-------|--------|
| **HttpOnly Cookie** | Защита от XSS | Уязвим к CSRF (нужна CSRF-защита) |
| **localStorage** | Простота использования | Уязвим к XSS |
| **sessionStorage** | Очищается при закрытии вкладки | Уязвим к XSS |
| **В памяти (JS-переменная)** | Наибольшая безопасность | Теряется при обновлении страницы |

Для банковских приложений рекомендуется: **access token в памяти + refresh token в HttpOnly Secure Cookie**.

### Best Practices

1. **Короткое время жизни access token** — 5-15 минут.
2. **Refresh token** — для обновления access token (хранить в HttpOnly cookie).
3. **Не храните чувствительные данные в payload** — JWT можно декодировать без ключа (Base64).
4. **Используйте `jti` (JWT ID)** — для возможности отзыва токенов.
5. **Ротируйте ключи** — периодическая смена ключей подписи.
6. **Проверяйте все claims** — `exp`, `iss`, `aud`, `nbf`.
7. **Blacklist для отзыва** — при logout добавляйте `jti` в Redis blacklist.

```java
// Проверка отозванных токенов
public boolean isTokenRevoked(String jti) {
    return redisTemplate.hasKey("revoked:" + jti);
}

public void revokeToken(String jti, long expSeconds) {
    redisTemplate.opsForValue().set("revoked:" + jti, "true",
        expSeconds, TimeUnit.SECONDS);
}
```

[к оглавлению](#Настройка-и-безопасность-сети)

## Как работает OAuth 2.0 и OpenID Connect?

**OAuth 2.0** — протокол авторизации, позволяющий приложению получить ограниченный доступ к ресурсам пользователя без передачи его пароля. **OpenID Connect (OIDC)** — надстройка над OAuth 2.0, добавляющая аутентификацию.

### Роли в OAuth 2.0

| Роль | Описание | Пример |
|------|----------|--------|
| **Resource Owner** | Владелец ресурса (пользователь) | Клиент банка |
| **Client** | Приложение, запрашивающее доступ | Мобильное приложение банка |
| **Authorization Server** | Сервер авторизации | Keycloak, Auth0 |
| **Resource Server** | Сервер с защищёнными ресурсами | API банка (Spring Boot) |

### Authorization Code Flow (основной поток)

Используется для серверных веб-приложений. Самый безопасный поток.

```
Пользователь       Приложение (Client)       Auth Server         API (Resource Server)
     │                    │                      │                       │
     │── Нажимает "Войти"─>│                      │                       │
     │                    │── Redirect ──────────>│                       │
     │                    │   /authorize?          │                       │
     │                    │   response_type=code&  │                       │
     │                    │   client_id=XXX&       │                       │
     │                    │   redirect_uri=...&    │                       │
     │                    │   scope=openid profile │                       │
     │                    │                      │                       │
     │<────── Страница логина ──────────────────│                       │
     │── Вводит логин/пароль ──────────────────>│                       │
     │                    │                      │                       │
     │<── Redirect ───────│                      │                       │
     │    callback?code=AUTH_CODE                 │                       │
     │                    │                      │                       │
     │                    │── POST /token ───────>│                       │
     │                    │   grant_type=          │                       │
     │                    │   authorization_code&  │                       │
     │                    │   code=AUTH_CODE&      │                       │
     │                    │   client_secret=YYY    │                       │
     │                    │                      │                       │
     │                    │<── access_token ──────│                       │
     │                    │    refresh_token       │                       │
     │                    │    id_token (OIDC)     │                       │
     │                    │                      │                       │
     │                    │── GET /api/accounts ──────────────────────────>│
     │                    │   Authorization: Bearer access_token           │
     │                    │<── Данные ───────────────────────────────────│
```

### Authorization Code + PKCE (для SPA и мобильных приложений)

PKCE (Proof Key for Code Exchange) защищает от перехвата authorization code.

```java
// 1. Клиент генерирует code_verifier (случайная строка)
String codeVerifier = generateRandomString(128);

// 2. Вычисляет code_challenge
String codeChallenge = Base64.getUrlEncoder()
    .withoutPadding()
    .encodeToString(
        MessageDigest.getInstance("SHA-256").digest(codeVerifier.getBytes())
    );

// 3. Отправляет code_challenge в /authorize
// GET /authorize?...&code_challenge=XXX&code_challenge_method=S256

// 4. При обмене code на token отправляет code_verifier
// POST /token { code_verifier: "..." }
// Сервер проверяет: SHA256(code_verifier) == code_challenge
```

### Client Credentials Flow (сервис-к-сервису)

Используется для межсервисного взаимодействия без участия пользователя.

```bash
# Запрос токена
curl -X POST https://auth.mybank.com/oauth/token \
  -d "grant_type=client_credentials" \
  -d "client_id=payment-service" \
  -d "client_secret=SECRET" \
  -d "scope=transfer.create transfer.read"
```

```java
// Spring Boot: настройка Client Credentials
@Bean
public OAuth2AuthorizedClientManager authorizedClientManager(
        ClientRegistrationRepository clientRegistrationRepository,
        OAuth2AuthorizedClientRepository authorizedClientRepository) {

    OAuth2AuthorizedClientProvider provider =
        OAuth2AuthorizedClientProviderBuilder.builder()
            .clientCredentials()
            .build();

    DefaultOAuth2AuthorizedClientManager manager =
        new DefaultOAuth2AuthorizedClientManager(
            clientRegistrationRepository, authorizedClientRepository);
    manager.setAuthorizedClientProvider(provider);
    return manager;
}
```

### OpenID Connect (OIDC)

OIDC добавляет к OAuth 2.0:
- **ID Token** — JWT с информацией о пользователе (sub, email, name).
- **UserInfo Endpoint** — `/userinfo` для получения расширенной информации.
- **Discovery** — `/.well-known/openid-configuration` с описанием всех эндпоинтов.
- Стандартные scopes: `openid`, `profile`, `email`.

### Настройка Spring Security + OAuth 2.0 Resource Server

```yaml
# application.yml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://auth.mybank.com/realms/bank
          # или явно указать JWK
          jwk-set-uri: https://auth.mybank.com/realms/bank/protocol/openid-connect/certs
```

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/accounts/**").hasRole("MANAGER")
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt
                    .jwtAuthenticationConverter(jwtAuthenticationConverter())
                )
            );
        return http.build();
    }
}
```

В банках OAuth 2.0 / OIDC является стандартом для аутентификации пользователей и авторизации межсервисных запросов. Как правило, используется Keycloak или аналогичный сервер авторизации.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое NetworkPolicy в Kubernetes?

**NetworkPolicy** — ресурс Kubernetes, позволяющий контролировать сетевой трафик между подами на уровне IP-адресов и портов. По умолчанию в Kubernetes все поды могут обмениваться трафиком друг с другом — NetworkPolicy ограничивает это поведение.

### Важно

NetworkPolicy работает только если в кластере установлен сетевой плагин (CNI), поддерживающий политики: **Calico**, **Cilium**, **Weave Net**. Стандартный Flannel политики не поддерживает.

### Типы политик

- **Ingress** — контроль входящего трафика к подам.
- **Egress** — контроль исходящего трафика от подов.

### Пример: изоляция пространства имён

```yaml
# Запретить весь входящий трафик в namespace payment
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-ingress
  namespace: payment
spec:
  podSelector: {}       # Применяется ко всем подам в namespace
  policyTypes:
    - Ingress            # Запрещаем весь входящий трафик
```

### Пример: разрешить трафик только от конкретного сервиса

```yaml
# Разрешить трафик к payment-service только от api-gateway
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-from-gateway
  namespace: payment
spec:
  podSelector:
    matchLabels:
      app: payment-service
  policyTypes:
    - Ingress
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: gateway
          podSelector:
            matchLabels:
              app: api-gateway
      ports:
        - protocol: TCP
          port: 8080
```

### Пример: ограничение исходящего трафика

```yaml
# payment-service может обращаться только к PostgreSQL и Kafka
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: payment-egress
  namespace: payment
spec:
  podSelector:
    matchLabels:
      app: payment-service
  policyTypes:
    - Egress
  egress:
    - to:
        - podSelector:
            matchLabels:
              app: postgresql
      ports:
        - protocol: TCP
          port: 5432
    - to:
        - podSelector:
            matchLabels:
              app: kafka
      ports:
        - protocol: TCP
          port: 9092
    # Разрешить DNS
    - to: []
      ports:
        - protocol: UDP
          port: 53
        - protocol: TCP
          port: 53
```

### Полная изоляция с точечными разрешениями (банковский подход)

```yaml
# 1. Запретить ВСЁ
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
  namespace: payment
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
---
# 2. Разрешить только необходимое
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-specific
  namespace: payment
spec:
  podSelector:
    matchLabels:
      app: payment-service
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: gateway
      ports:
        - port: 8080
  egress:
    - to:
        - namespaceSelector:
            matchLabels:
              name: data
          podSelector:
            matchLabels:
              app: postgresql
      ports:
        - port: 5432
```

В банковском Kubernetes-кластере NetworkPolicy — обязательный элемент безопасности. Принцип: «deny all, allow explicit» — запрещено всё, что явно не разрешено.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое Service Mesh и как он обеспечивает безопасность?

**Service Mesh** — инфраструктурный слой, управляющий межсервисным взаимодействием в микросервисной архитектуре. Реализуется через sidecar-прокси (Envoy), которые внедряются в каждый под и перехватывают весь сетевой трафик.

### Основные реализации

| Решение | Описание |
|---------|----------|
| **Istio** | Наиболее функциональный, использует Envoy, поддержка Google |
| **Linkerd** | Легковесный, написан на Rust (прокси), проще в эксплуатации |
| **Consul Connect** | От HashiCorp, интегрируется с Consul для service discovery |

### Архитектура Service Mesh (Istio)

```
┌──────────────── Control Plane (istiod) ─────────────────┐
│  Pilot (конфигурация маршрутов)                          │
│  Citadel (управление сертификатами, mTLS)                │
│  Galley (валидация конфигурации)                         │
└─────────────────────────────────────────────────────────┘
         │                              │
    ┌────▼─────────────┐     ┌──────────▼──────────┐
    │  Pod              │     │  Pod                 │
    │ ┌───────────────┐ │     │ ┌──────────────────┐ │
    │ │ payment-svc   │ │     │ │ account-svc      │ │
    │ └───────┬───────┘ │     │ └───────┬──────────┘ │
    │         │         │     │         │            │
    │ ┌───────▼───────┐ │     │ ┌───────▼──────────┐ │
    │ │ Envoy (sidecar)│◄────►│ │ Envoy (sidecar)  │ │
    │ └───────────────┘ │     │ └──────────────────┘ │
    └───────────────────┘     └──────────────────────┘
          mTLS-соединение
```

### Автоматический mTLS между сервисами

```yaml
# Istio PeerAuthentication: включение mTLS для всего mesh
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: istio-system
spec:
  mtls:
    mode: STRICT    # Весь трафик только через mTLS
```

```yaml
# Разрешить трафик только от конкретных сервисов
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: payment-policy
  namespace: payment
spec:
  selector:
    matchLabels:
      app: payment-service
  action: ALLOW
  rules:
    - from:
        - source:
            principals:
              - "cluster.local/ns/gateway/sa/api-gateway"
      to:
        - operation:
            methods: ["POST"]
            paths: ["/api/transfers/*"]
```

### Возможности Service Mesh для безопасности

1. **Автоматический mTLS** — шифрование и аутентификация всех межсервисных соединений без изменения кода приложений.
2. **Авторизация на уровне сервисов** (AuthorizationPolicy) — какой сервис может вызывать какой.
3. **Наблюдаемость** — метрики, логи, трассировки для всего трафика.
4. **Rate Limiting** — ограничение запросов между сервисами.
5. **Ротация сертификатов** — автоматическая, без простоя.

### Linkerd (более простая альтернатива)

```bash
# Установка
curl -sL https://run.linkerd.io/install | sh
linkerd install | kubectl apply -f -

# Добавление sidecar к деплойменту
kubectl get deploy payment-service -o yaml | linkerd inject - | kubectl apply -f -

# Проверка mTLS
linkerd viz edges deployment -n payment
```

В банковской среде Service Mesh решает ключевую задачу — обеспечивает шифрование и аутентификацию трафика между микросервисами автоматически, без необходимости настраивать TLS в каждом сервисе вручную.

[к оглавлению](#Настройка-и-безопасность-сети)

## Как безопасно хранить секреты в приложении?

Секреты (пароли, ключи API, сертификаты, токены) — одна из самых уязвимых частей любого приложения. В банковской среде утечка секретов может привести к катастрофическим последствиям.

### Что НЕЛЬЗЯ делать

```java
// КАТЕГОРИЧЕСКИ ЗАПРЕЩЕНО: секреты в коде
public class DatabaseConfig {
    private static final String DB_PASSWORD = "SuperSecret123!"; // Попадёт в Git
}
```

```yaml
# НЕПРАВИЛЬНО: секреты в application.yml в репозитории
spring:
  datasource:
    password: SuperSecret123!
```

### Уровни хранения секретов (от простого к сложному)

### 1. Переменные окружения

Простейший способ, подходит для начального уровня:

```yaml
# application.yml
spring:
  datasource:
    password: ${DB_PASSWORD}
```

```bash
# Запуск с переменной окружения
export DB_PASSWORD=SuperSecret123!
java -jar app.jar

# Или в Docker
docker run -e DB_PASSWORD=SuperSecret123! myapp:latest
```

**Минусы:** переменные видны через `/proc/<pid>/environ`, в docker inspect, в логах систем оркестрации.

### 2. Kubernetes Secrets

```yaml
# Создание секрета
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
  namespace: payment
type: Opaque
data:
  password: U3VwZXJTZWNyZXQxMjMh  # base64-encoded (НЕ шифрование!)
```

```yaml
# Использование в Pod
spec:
  containers:
    - name: payment-service
      env:
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: password
```

**Важно:** Kubernetes Secrets по умолчанию хранятся в etcd в base64 (не зашифровано!). Необходимо включить шифрование etcd at rest:

```yaml
# /etc/kubernetes/encryption-config.yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <base64-encoded-32-byte-key>
      - identity: {}
```

### 3. HashiCorp Vault

Vault — специализированное решение для управления секретами, используемое в крупных банках.

```bash
# Запись секрета
vault kv put secret/payment-service db_password=SuperSecret123!

# Чтение секрета
vault kv get secret/payment-service
```

Интеграция с Spring Boot через **Spring Cloud Vault**:

```yaml
# bootstrap.yml
spring:
  cloud:
    vault:
      uri: https://vault.mybank.local:8200
      authentication: KUBERNETES  # аутентификация через K8s Service Account
      kubernetes:
        role: payment-service
      kv:
        backend: secret
        default-context: payment-service
```

```java
// Секреты автоматически подставляются в Spring-конфигурацию
@Value("${db_password}")
private String dbPassword;
```

### Возможности Vault

- **Динамические секреты** — Vault генерирует временные учётные данные к БД, которые автоматически отзываются.
- **Ротация секретов** — автоматическая смена паролей по расписанию.
- **Аудит** — полный лог доступа к секретам.
- **Шифрование как сервис** (Transit engine) — Vault шифрует/расшифровывает данные без раскрытия ключа приложению.

```bash
# Динамические секреты для PostgreSQL
vault write database/config/mydb \
    plugin_name=postgresql-database-plugin \
    connection_url="postgresql://{{username}}:{{password}}@db:5432/mydb" \
    allowed_roles="payment-role" \
    username="vault" \
    password="vaultpass"

vault write database/roles/payment-role \
    db_name=mydb \
    creation_statements="CREATE ROLE \"{{name}}\" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}'; GRANT SELECT, INSERT ON ALL TABLES IN SCHEMA public TO \"{{name}}\";" \
    default_ttl="1h" \
    max_ttl="24h"

# Получение временных учётных данных
vault read database/creds/payment-role
```

### Рекомендации для банковской среды

1. **Production** — использовать HashiCorp Vault или аналог (CyberArk, AWS Secrets Manager).
2. **Staging/Dev** — Kubernetes Secrets с шифрованием etcd.
3. **Никогда** — секреты в Git, даже в приватных репозиториях.
4. **Ротация** — регулярная смена всех секретов (минимум раз в квартал).
5. **Аудит** — логирование всех операций с секретами.
6. **Sealed Secrets** — для GitOps-подхода (секреты в Git, но зашифрованные).

[к оглавлению](#Настройка-и-безопасность-сети)

## Чем отличается шифрование данных at rest и in transit?

### Шифрование in transit (при передаче)

Защита данных во время передачи по сети — от отправителя к получателю.

**Протоколы:**
- **TLS 1.2 / 1.3** — основной протокол для HTTPS, межсервисных вызовов.
- **mTLS** — взаимная аутентификация (см. раздел выше).
- **SSH** — для административного доступа.

**Примеры в банковской среде:**

```yaml
# Spring Boot: подключение к PostgreSQL через SSL
spring:
  datasource:
    url: jdbc:postgresql://db-master:5432/banking?ssl=true&sslmode=verify-full&sslrootcert=/certs/ca.crt

# Spring Boot: подключение к Kafka через SSL
spring:
  kafka:
    ssl:
      trust-store-location: classpath:truststore.jks
      trust-store-password: ${KAFKA_TRUSTSTORE_PASSWORD}
      key-store-location: classpath:keystore.jks
      key-store-password: ${KAFKA_KEYSTORE_PASSWORD}
    security:
      protocol: SSL
```

```yaml
# PostgreSQL: включение SSL
# postgresql.conf
ssl = on
ssl_cert_file = '/certs/server.crt'
ssl_key_file = '/certs/server.key'
ssl_ca_file = '/certs/ca.crt'

# pg_hba.conf — требовать SSL для всех подключений
hostssl all all 0.0.0.0/0 cert clientcert=verify-full
```

### Шифрование at rest (при хранении)

Защита данных, хранящихся на диске — в базах данных, файловых системах, резервных копиях.

### Уровни шифрования at rest

**1. Шифрование на уровне диска (Full Disk Encryption):**

```bash
# LUKS (Linux Unified Key Setup)
sudo cryptsetup luksFormat /dev/sdb
sudo cryptsetup luksOpen /dev/sdb encrypted_data
sudo mkfs.ext4 /dev/mapper/encrypted_data
```

**2. Шифрование на уровне базы данных:**

```sql
-- PostgreSQL: Transparent Data Encryption (TDE) через расширение
-- Или шифрование на уровне tablespace

-- Шифрование конкретных столбцов с помощью pgcrypto
CREATE EXTENSION pgcrypto;

-- Шифрование данных
INSERT INTO clients (name, card_number) VALUES (
    'Иванов',
    pgp_sym_encrypt('4111111111111111', 'encryption_key')
);

-- Расшифрование
SELECT name, pgp_sym_decrypt(card_number::bytea, 'encryption_key')
FROM clients;
```

**3. Шифрование на уровне приложения (Application-Level Encryption):**

```java
// Шифрование AES-256-GCM
public class EncryptionService {
    private static final String ALGORITHM = "AES/GCM/NoPadding";
    private static final int TAG_LENGTH = 128;
    private static final int IV_LENGTH = 12;

    public byte[] encrypt(byte[] data, SecretKey key) throws Exception {
        byte[] iv = new byte[IV_LENGTH];
        SecureRandom.getInstanceStrong().nextBytes(iv);

        Cipher cipher = Cipher.getInstance(ALGORITHM);
        cipher.init(Cipher.ENCRYPT_MODE, key, new GCMParameterSpec(TAG_LENGTH, iv));
        byte[] encrypted = cipher.doFinal(data);

        // Возвращаем IV + зашифрованные данные
        return ByteBuffer.allocate(iv.length + encrypted.length)
            .put(iv).put(encrypted).array();
    }

    public byte[] decrypt(byte[] encryptedData, SecretKey key) throws Exception {
        ByteBuffer buffer = ByteBuffer.wrap(encryptedData);
        byte[] iv = new byte[IV_LENGTH];
        buffer.get(iv);
        byte[] cipherText = new byte[buffer.remaining()];
        buffer.get(cipherText);

        Cipher cipher = Cipher.getInstance(ALGORITHM);
        cipher.init(Cipher.DECRYPT_MODE, key, new GCMParameterSpec(TAG_LENGTH, iv));
        return cipher.doFinal(cipherText);
    }
}
```

**4. Шифрование через Vault Transit Engine:**

```bash
# Шифрование данных через Vault (ключ не покидает Vault)
vault write transit/encrypt/payment-key \
    plaintext=$(echo -n "4111111111111111" | base64)

# Расшифрование
vault write transit/decrypt/payment-key \
    ciphertext="vault:v1:AbcDef..."
```

### Что шифровать в банковской среде

| Данные | At rest | In transit |
|--------|---------|-----------|
| Номера карт (PAN) | AES-256, маскирование | TLS 1.2+ |
| Персональные данные (ПДн) | AES-256 | TLS 1.2+ |
| Пароли | Bcrypt/Argon2 (хеширование, не шифрование!) | TLS 1.2+ |
| Резервные копии БД | AES-256 | TLS/SSH |
| Логи | Шифрование диска | TLS (syslog-TLS) |

Требование PCI DSS: данные карт обязательно должны быть зашифрованы и при хранении, и при передаче.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что и как логировать с точки зрения безопасности?

Правильное логирование — ключевой элемент информационной безопасности. В банковской среде audit log (журнал аудита) — обязательное требование регуляторов.

### Что НЕОБХОДИМО логировать

**1. События аутентификации:**

```java
log.info("Успешная аутентификация: user={}, ip={}, userAgent={}",
    username, request.getRemoteAddr(), request.getHeader("User-Agent"));

log.warn("Неудачная попытка аутентификации: user={}, ip={}, причина={}",
    username, request.getRemoteAddr(), failureReason);

log.warn("Блокировка учётной записи: user={}, причина=превышение попыток",
    username);
```

**2. Действия авторизации:**

```java
log.info("Доступ разрешён: user={}, action={}, resource={}",
    username, "VIEW_ACCOUNT", accountId);

log.warn("Доступ запрещён: user={}, action={}, resource={}, причина={}",
    username, "TRANSFER", accountId, "insufficient_role");
```

**3. Критичные бизнес-операции:**

```java
log.info("Создание перевода: user={}, from={}, to={}, amount={}, currency={}, traceId={}",
    username, fromAccount, toAccount, amount, currency, traceId);

log.info("Подтверждение перевода: transferId={}, user={}, method=SMS",
    transferId, username);
```

**4. Системные события безопасности:**

```java
log.warn("Подозрительная активность: user={}, ip={}, причина=множественные запросы к разным счетам",
    username, ip);

log.error("Ошибка валидации сертификата: host={}, причина={}", host, e.getMessage());
```

### Что НЕЛЬЗЯ логировать

```java
// ЗАПРЕЩЕНО: пароли
log.info("Логин: user={}, password={}", user, password); // НЕЛЬЗЯ!

// ЗАПРЕЩЕНО: полные номера карт
log.info("Оплата картой: {}", cardNumber); // НЕЛЬЗЯ!

// ЗАПРЕЩЕНО: токены доступа
log.info("Токен: {}", accessToken); // НЕЛЬЗЯ!

// ЗАПРЕЩЕНО: CVV/CVC, PIN-коды
// ЗАПРЕЩЕНО: секретные ключи, сертификаты приватные
// ЗАПРЕЩЕНО: полные персональные данные (паспорт, СНИЛС)
```

### Маскирование данных в логах

```java
// Правильный подход: маскирование
public class MaskingUtil {
    public static String maskCardNumber(String pan) {
        if (pan == null || pan.length() < 10) return "****";
        return pan.substring(0, 6) + "****" + pan.substring(pan.length() - 4);
        // 411111****1111
    }

    public static String maskEmail(String email) {
        return email.replaceAll("(^[^@]{2})[^@]*(@.*)", "$1****$2");
        // iv****@mail.ru
    }
}

log.info("Оплата картой: {}", MaskingUtil.maskCardNumber(cardNumber));
```

### Logback: автоматическое маскирование

```xml
<!-- logback-spring.xml -->
<configuration>
    <conversionRule conversionWord="mask"
        converterClass="com.mybank.logging.MaskingConverter" />

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>%d{ISO8601} [%thread] %-5level %logger - %mask(%msg)%n</pattern>
        </encoder>
    </appender>
</configuration>
```

### Структурированное логирование (JSON)

Для SIEM-систем (Splunk, ELK) предпочтительны структурированные логи:

```java
// Использование MDC для контекста
MDC.put("userId", username);
MDC.put("traceId", traceId);
MDC.put("ip", request.getRemoteAddr());

log.info("Перевод выполнен: transferId={}, amount={}", transferId, amount);

// В JSON-формате:
// {
//   "timestamp": "2024-01-15T10:30:00Z",
//   "level": "INFO",
//   "userId": "ivanov",
//   "traceId": "abc-123",
//   "ip": "10.0.1.50",
//   "message": "Перевод выполнен: transferId=T-001, amount=50000"
// }
```

### Хранение и защита логов

1. **Centralized logging** — отправлять логи в централизованную систему (ELK, Splunk, Graylog).
2. **Неизменяемость** — логи должны быть защищены от модификации (append-only).
3. **Ретенция** — хранить логи аудита не менее 1-3 лет (требование регуляторов).
4. **Контроль доступа** — доступ к логам ограничен (не все разработчики видят production-логи).
5. **Шифрование** — при хранении и при передаче.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое Zero Trust Architecture?

**Zero Trust («нулевое доверие»)** — архитектурный подход к безопасности, основанный на принципе **«никогда не доверяй, всегда проверяй»**. В отличие от традиционной модели (доверяем всему внутри периметра), Zero Trust предполагает, что угроза может исходить откуда угодно — в том числе из внутренней сети.

### Традиционная модель vs Zero Trust

```
Традиционная модель (Castle and Moat):
┌─────────────────────────────────┐
│      Корпоративная сеть         │
│  Все доверяют друг другу        │
│  Сервис A ←→ Сервис B (без     │
│  аутентификации, HTTP)          │
└─────────────┬───────────────────┘
              │ Firewall
         [Интернет]

Zero Trust:
┌─────────────────────────────────┐
│  Каждый запрос проверяется:     │
│  • Аутентификация (кто?)        │
│  • Авторизация (имеет право?)   │
│  • Шифрование (TLS/mTLS)       │
│  • Контекст (откуда? когда?)    │
│  Сервис A ──mTLS──→ Сервис B   │
└─────────────────────────────────┘
```

### Принципы Zero Trust

1. **Проверяй каждый запрос** — аутентификация и авторизация на каждом шаге, а не только на периметре.
2. **Минимальные привилегии** — каждый сервис/пользователь получает только необходимые права.
3. **Предполагай компрометацию** — проектируй систему так, будто злоумышленник уже внутри.
4. **Микросегментация** — каждый сервис изолирован, трафик между ними контролируется.
5. **Непрерывный мониторинг** — все действия логируются и анализируются.

### Реализация Zero Trust для Java-микросервисов

**1. Идентификация сервисов (mTLS через Service Mesh):**

```yaml
# Istio: строгий mTLS для всего mesh
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: strict-mtls
  namespace: istio-system
spec:
  mtls:
    mode: STRICT
```

**2. Авторизация на каждом уровне:**

```java
// Spring Security: проверка роли и контекста
@PreAuthorize("hasRole('PAYMENT_SERVICE') and #request.amount <= 1000000")
public TransferResult processTransfer(TransferRequest request) {
    // ...
}
```

```yaml
# Istio AuthorizationPolicy: только api-gateway может вызывать payment-service
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: payment-authz
spec:
  selector:
    matchLabels:
      app: payment-service
  rules:
    - from:
        - source:
            principals: ["cluster.local/ns/gateway/sa/api-gateway"]
      to:
        - operation:
            methods: ["POST"]
            paths: ["/api/transfers"]
```

**3. Сетевая микросегментация:**

```yaml
# Kubernetes NetworkPolicy: каждый сервис в своём сегменте
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: payment-isolation
spec:
  podSelector:
    matchLabels:
      app: payment-service
  policyTypes: [Ingress, Egress]
  ingress:
    - from:
        - podSelector: { matchLabels: { app: api-gateway } }
  egress:
    - to:
        - podSelector: { matchLabels: { app: postgresql } }
```

**4. Контекстная авторизация:**

```java
// Проверяем не только "кто", но и "откуда", "когда", "на каком устройстве"
@Component
public class ZeroTrustFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest request, ...) {
        String ip = request.getRemoteAddr();
        String userAgent = request.getHeader("User-Agent");
        String geoLocation = geoService.getLocation(ip);

        // Аномальная геолокация?
        if (!isKnownLocation(userId, geoLocation)) {
            log.warn("Подозрительный вход: user={}, geo={}", userId, geoLocation);
            requireAdditionalVerification(userId);
        }
    }
}
```

### Zero Trust в банковском контексте

- **PCI DSS** — требует сегментацию сети и контроль доступа между системами, обрабатывающими данные карт.
- **ЦБ РФ (ГОСТ Р 57580)** — требует разграничение доступа и мониторинг всех операций.
- **Практика** — в банках Zero Trust реализуется поэтапно: mTLS → NetworkPolicy → авторизация сервисов → контекстная аналитика.

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое VPN и как настроить базовое VPN-соединение?

**VPN (Virtual Private Network)** — технология создания защищённого сетевого соединения поверх публичной сети (интернет). Весь трафик между участниками VPN шифруется.

### Типы VPN

| Тип | Описание | Применение |
|-----|----------|-----------|
| **Site-to-Site** | Соединение двух сетей (офисов) | Связь между ЦОД банка |
| **Remote Access** | Подключение удалённого пользователя к сети | Удалённая работа сотрудников |
| **Client-to-Site** | Аналог Remote Access | Разработчики подключаются к dev-среде |

### Основные протоколы

| Протокол | Описание |
|----------|----------|
| **IPSec** | Классический, работает на сетевом уровне (L3), поддержка IKEv2 |
| **WireGuard** | Современный, простой, быстрый, минимум кода (4000 строк vs 600K у OpenVPN) |
| **OpenVPN** | Работает через SSL/TLS, кроссплатформенный |

### Настройка WireGuard

WireGuard — наиболее современный и простой VPN-протокол.

**Сервер:**

```bash
# Установка
sudo apt install wireguard

# Генерация ключей сервера
wg genkey | tee /etc/wireguard/server_private.key | wg pubkey > /etc/wireguard/server_public.key
chmod 600 /etc/wireguard/server_private.key
```

```ini
# /etc/wireguard/wg0.conf (сервер)
[Interface]
PrivateKey = <server_private_key>
Address = 10.200.0.1/24
ListenPort = 51820
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

# Клиент: разработчик
[Peer]
PublicKey = <client_public_key>
AllowedIPs = 10.200.0.2/32
```

**Клиент:**

```bash
# Генерация ключей клиента
wg genkey | tee client_private.key | wg pubkey > client_public.key
```

```ini
# /etc/wireguard/wg0.conf (клиент)
[Interface]
PrivateKey = <client_private_key>
Address = 10.200.0.2/24
DNS = 10.0.0.1    # DNS банковской сети

[Peer]
PublicKey = <server_public_key>
Endpoint = vpn.mybank.com:51820
AllowedIPs = 10.0.0.0/8, 172.16.0.0/12    # Маршрутизировать корпоративные подсети через VPN
PersistentKeepalive = 25
```

```bash
# Запуск
sudo wg-quick up wg0

# Автозапуск
sudo systemctl enable wg-quick@wg0

# Проверка
sudo wg show
```

### Настройка IPSec (StrongSwan)

```bash
sudo apt install strongswan

# /etc/ipsec.conf
conn bank-site-to-site
    type=tunnel
    left=203.0.113.1         # Внешний IP нашего сервера
    leftsubnet=10.0.1.0/24   # Наша подсеть
    right=198.51.100.1       # Внешний IP удалённого сервера
    rightsubnet=10.0.2.0/24  # Удалённая подсеть
    authby=secret
    auto=start
    ike=aes256-sha256-modp2048
    esp=aes256-sha256

# /etc/ipsec.secrets
203.0.113.1 198.51.100.1 : PSK "super-secret-psk"
```

```bash
sudo systemctl restart strongswan
sudo ipsec statusall
```

### Применение в банке

- **Site-to-Site VPN** — связь между основным ЦОД и резервным.
- **Remote Access VPN** — подключение разработчиков к dev/staging-средам.
- **VPN для партнёров** — подключение контрагентов к интеграционным шлюзам.
- **Split Tunneling** — только корпоративный трафик через VPN, остальной напрямую (или полный туннель для безопасности).

[к оглавлению](#Настройка-и-безопасность-сети)

## Что такое сетевая сегментация и зачем она нужна?

**Сетевая сегментация** — разделение сети на изолированные сегменты (подсети), между которыми трафик контролируется файрволами и маршрутизаторами. Это фундаментальный принцип безопасности, особенно важный в банковской инфраструктуре.

### Зачем нужна сегментация

1. **Ограничение blast radius** — при компрометации одного сервиса злоумышленник не получает доступ ко всей сети.
2. **Соответствие PCI DSS** — данные карт должны обрабатываться в изолированном сегменте (CDE — Cardholder Data Environment).
3. **Разграничение сред** — production, staging, development изолированы друг от друга.
4. **Контроль трафика** — легче отслеживать и фильтровать подозрительный трафик.

### VLAN (Virtual LAN)

VLAN позволяет разделить физическую сеть на логические сегменты на уровне коммутатора (L2):

```
┌────────── VLAN 10: Серверы приложений ──────────┐
│  10.0.10.0/24                                    │
│  payment-service  account-service  auth-service  │
└──────────────────────────────────────────────────┘

┌────────── VLAN 20: Базы данных ─────────────────┐
│  10.0.20.0/24                                    │
│  postgresql-master  postgresql-replica  redis     │
└──────────────────────────────────────────────────┘

┌────────── VLAN 30: Управление ──────────────────┐
│  10.0.30.0/24                                    │
│  monitoring  logging  ci-cd  bastion-host        │
└──────────────────────────────────────────────────┘

┌────────── VLAN 40: DMZ ─────────────────────────┐
│  10.0.40.0/24                                    │
│  nginx  api-gateway  waf                         │
└──────────────────────────────────────────────────┘
```

### Подсети (Subnetting)

Разделение на подсети на уровне IP (L3):

```bash
# Пример структуры подсетей банка
10.0.0.0/16          # Общая сеть банка

10.0.10.0/24         # Серверы приложений (production)
10.0.20.0/24         # Базы данных (production)
10.0.30.0/24         # Управление и мониторинг
10.0.40.0/24         # DMZ
10.0.50.0/24         # Kafka, RabbitMQ (message brokers)
10.0.100.0/24        # Staging
10.0.200.0/24        # Development
```

### Правила маршрутизации между сегментами

```bash
# На маршрутизаторе / файрволе:

# Серверы приложений могут обращаться к БД
iptables -A FORWARD -s 10.0.10.0/24 -d 10.0.20.0/24 -p tcp --dport 5432 -j ACCEPT

# DMZ может обращаться к серверам приложений
iptables -A FORWARD -s 10.0.40.0/24 -d 10.0.10.0/24 -p tcp --dport 8080 -j ACCEPT

# DMZ НЕ может обращаться к БД напрямую
iptables -A FORWARD -s 10.0.40.0/24 -d 10.0.20.0/24 -j DROP

# Development НЕ может обращаться к production
iptables -A FORWARD -s 10.0.200.0/24 -d 10.0.10.0/24 -j DROP
iptables -A FORWARD -s 10.0.200.0/24 -d 10.0.20.0/24 -j DROP
```

### Сегментация в Kubernetes

В Kubernetes сегментация реализуется через:

1. **Namespaces** — логическое разделение.
2. **NetworkPolicy** — контроль трафика между подами.
3. **Service Mesh** — дополнительный уровень контроля.

```yaml
# Namespace для каждого домена
apiVersion: v1
kind: Namespace
metadata:
  name: payment
  labels:
    environment: production
    pci-scope: "true"       # Помечаем как PCI DSS scope
---
apiVersion: v1
kind: Namespace
metadata:
  name: reporting
  labels:
    environment: production
    pci-scope: "false"
```

### Матрица доступа (пример для банка)

| Источник \ Назначение | DMZ | App Servers | Databases | Monitoring | Dev |
|----------------------|-----|-------------|-----------|-----------|-----|
| **Интернет** | 80,443 | - | - | - | - |
| **DMZ** | - | 8080 | - | - | - |
| **App Servers** | - | - | 5432,6379 | 9090 | - |
| **Monitoring** | ICMP | ICMP,9090 | ICMP,9090 | - | - |
| **Dev** | - | - | - | - | Все |

Сетевая сегментация — обязательное требование стандартов PCI DSS, ГОСТ Р 57580 и рекомендаций ЦБ РФ. Для Java-разработчика важно понимать, в каком сегменте работает его приложение и к каким ресурсам оно имеет доступ, чтобы корректно настраивать соединения и обрабатывать ситуации, когда сетевой доступ ограничен.

[к оглавлению](#Настройка-и-безопасность-сети)
