---
## Front matter
title: "Лабораторная работа №2"
author: "Михайлова Регина Алексеевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Получить основные навыки по начальному конфигурированию оборудования Cisco.

# Выполнение лабораторной работы

![Картинка 1](image/1.png){#fig:001 width=70%}

## Настройка маршрутизатора

**Команды конфигурации маршрутизатора**
```bash
Router> enable
Router# configure terminal
Router(config)# hostname msk-donskaya-gw-1

msk-donskaya-gw-1(config)# interface f0/0
msk-donskaya-gw-1(config-if)# no shutdown
msk-donskaya-gw-1(config-if)# ip address 192.168.1.254 255.255.255.0

msk-donskaya-gw-1(config)# line vty 0 4
msk-donskaya-gw-1(config-line)# password cisco
msk-donskaya-gw-1(config-line)# login

msk-donskaya-gw-1(config)# line console 0
msk-donskaya-gw-1(config-line)# password cisco
msk-donskaya-gw-1(config-line)# login

msk-donskaya-gw-1(config)# enable secret cisco
msk-donskaya-gw-1(config)# service password-encryption

msk-donskaya-gw-1(config)# username admin privilege 1 secret cisco
msk-donskaya-gw-1(config)# ip domain-name donskaya.rudn.edu
msk-donskaya-gw-1(config)# crypto key generate rsa

msk-donskaya-gw-1(config)# line vty 0 4
msk-donskaya-gw-1(config-line)# transport input ssh
```

**Результат:**
- Интерфейс **Fa0/0** поднят (`no shutdown`) и получил IP **192.168.1.254/24**.
- Для **console** и **vty** задан пароль `cisco`, включён `login`.
- Включено шифрование паролей: `service password-encryption`.
- Задан пользователь `admin` и доменное имя, сгенерированы RSA-ключи.
- На VTY разрешён только **SSH**.

![Картинка 3](image/3.png){#fig:003 width=70%}

## Настройка коммутатора

**Команды конфигурации коммутатора (по заданию):**
```bash
Switch> enable
Switch# configure terminal
Switch(config)# hostname msk-donskaya-sw-1

msk-donskaya-sw-1(config)# interface vlan2
msk-donskaya-sw-1(config-if)# no shutdown
msk-donskaya-sw-1(config-if)# ip address 192.168.2.1 255.255.255.0

msk-donskaya-sw-1(config)# interface f0/1
msk-donskaya-sw-1(config-if)# switchport mode access
msk-donskaya-sw-1(config-if)# switchport access vlan 2

msk-donskaya-sw-1(config)# ip default-gateway 192.168.2.254

msk-donskaya-sw-1(config)# line vty 0 4
msk-donskaya-sw-1(config-line)# password cisco
msk-donskaya-sw-1(config-line)# login

msk-donskaya-sw-1(config)# line console 0
msk-donskaya-sw-1(config-line)# password cisco
msk-donskaya-sw-1(config-line)# login

msk-donskaya-sw-1(config)# enable secret cisco
msk-donskaya-sw-1(config)# service password-encryption

msk-donskaya-sw-1(config)# username admin privilege 1 secret cisco
msk-donskaya-sw-1(config)# ip domain-name donskaya.rudn.edu
msk-donskaya-sw-1(config)# crypto key generate rsa
```

**Результат:**
- SVI **VLAN 2** включён и получил IP **192.168.2.1/24**.
- Порт **Fa0/1** переведён в access и назначен в **VLAN 2**.
- Указан шлюз управления: `ip default-gateway 192.168.2.254`.
- Настроены пароли на console и vty, включено шифрование, создан пользователь и RSA-ключи (подготовка к SSH).

![Картинка 2](image/2.png){#fig:002 width=70%}

### Проверка работоспособности

**Проверки:**
- С ПК, подключенного к маршрутизатору: `ping 192.168.1.254` — ответы получены (маршрутизатор доступен).
- С ПК, подключенного к коммутатору: `ping 192.168.2.1` — ответы получены (SVI коммутатора доступен).

Вывод: базовая IP-доступность в пределах своих сегментов подтверждена.


### Подключение к устройствам разными способами

#### Подключение по консоли
- Подключала PC к **Console** порту устройства консольным кабелем.
- В Terminal на ПК выбирала параметры по умолчанию (9600, 8N1).
- Доступ получен, после запроса пароля вводила `cisco`.

#### Подключение по Telnet
- На маршрутизаторе в строках VTY в конце задано `transport input ssh`, поэтому **telnet к маршрутизатору не проходит** (ожидаемо по настройке).
- На коммутаторе `transport input` явно не ограничивался, поэтому telnet теоретически возможен, если в PT он разрешён и есть IP-доступ к SVI. При подключении требовался пароль линии VTY `cisco`.

#### Подключение по SSH
- Для SSH заранее были заданы: `ip domain-name`, пользователь `admin`, сгенерированы RSA-ключи.
- Подключение выполняла с ПК командой вида:
  - `ssh -l admin 192.168.1.254` (маршрутизатор)
  - `ssh -l admin 192.168.2.1` (коммутатор)
- Авторизация проходила с паролем `cisco`.

# Вывод:  

В ходе лабораторной работы были получены основные навыки по начальному конфигурированию оборудования Cisco.