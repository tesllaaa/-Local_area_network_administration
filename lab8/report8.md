---
## Front matter
title: "Лабораторная работа №8"
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

Приобретение практических навыков по настройке динамического распределения IP-адресов посредством протокола DHCP (Dynamic Host Configuration
Protocol) в локальной сети.

# Выполнение лабораторной работы

Шаг 1. Добавление и базовая настройка DNS-сервера
В логическую рабочую область проекта я добавила сервер dns и подключила его к коммутатору msk-donskaya-sw-3 через порт Fa0/2. 

![Картинка 1](image/1.png){#fig:001 width=70%}

Порт коммутатора был предварительно активирован.
В конфигурации сервера я задала статические сетевые настройки:

IP-адрес: 10.128.0.5
Маска подсети: 255.255.255.0
Шлюз по умолчанию: 10.128.0.1

![Картинка 2](image/2.png){#fig:002 width=70%}


Шаг 2. Настройка службы DNS
Затем я перешел(ла) во вкладку Services -> DNS на сервере. Я включила службу, переведя переключатель в положение On.
В качестве типа записи (Type) был выбран A Record. Далее я добавила в базу данных DNS-сервера следующие ресурсные записи для домена donskaya.rudn.ru, связывающие имена узлов с их IP-адресами:

dns.donskaya.rudn.ru — 10.128.0.5
file.donskaya.rudn.ru — 10.128.0.3
mail.donskaya.rudn.ru — 10.128.0.4
www.donskaya.rudn.ru — 10.128.0.2

![Картинка 3](image/3.png){#fig:003 width=70%}


Шаг 3. Настройка DHCP-сервера на маршрутизаторе
Для динамической выдачи IP-адресов пользователям сети я настроила DHCP-сервис на главном маршрутизаторе msk-donskaya-gw-1.
В режиме глобальной конфигурации я включила сервис DHCP, указала глобальный адрес DNS-сервера, а затем создала пулы адресов для каждой из подсетей (dk, departments, adm, other). Для каждого пула была задана своя сеть, шлюз по умолчанию и исключены адреса (шлюзы и серверы), чтобы избежать конфликта IP-адресов при их выдаче.

Введенные команды:

text

msk-donskaya-gw-1>enable
msk-donskaya-gw-1#configure terminal
msk-donskaya-gw-1(config)#ip name-server 10.128.0.5
msk-donskaya-gw-1(config)#service dhcp

! Настройка пула для подсети dk
msk-donskaya-gw-1(config)#ip dhcp pool dk
msk-donskaya-gw-1(dhcp-config)#network 10.128.3.0 255.255.255.0
msk-donskaya-gw-1(dhcp-config)#default-router 10.128.3.1
msk-donskaya-gw-1(dhcp-config)#dns-server 10.128.0.5
msk-donskaya-gw-1(dhcp-config)#exit
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.3.1 10.128.3.29
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.3.200 10.128.3.254

! Настройка пула для подсети departments
msk-donskaya-gw-1(config)#ip dhcp pool departments
msk-donskaya-gw-1(dhcp-config)#network 10.128.4.0 255.255.255.0
msk-donskaya-gw-1(dhcp-config)#default-router 10.128.4.1
msk-donskaya-gw-1(dhcp-config)#dns-server 10.128.0.5
msk-donskaya-gw-1(dhcp-config)#exit
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.4.1 10.128.4.29
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.4.200 10.128.4.254

! Настройка пула для подсети adm
msk-donskaya-gw-1(config)#ip dhcp pool adm
msk-donskaya-gw-1(dhcp-config)#network 10.128.5.0 255.255.255.0
msk-donskaya-gw-1(dhcp-config)#default-router 10.128.5.1
msk-donskaya-gw-1(dhcp-config)#dns-server 10.128.0.5
msk-donskaya-gw-1(dhcp-config)#exit
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.5.1 10.128.5.29
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.5.200 10.128.5.254

! Настройка пула для подсети other
msk-donskaya-gw-1(config)#ip dhcp pool other
msk-donskaya-gw-1(dhcp-config)#network 10.128.6.0 255.255.255.0
msk-donskaya-gw-1(dhcp-config)#default-router 10.128.6.1
msk-donskaya-gw-1(dhcp-config)#dns-server 10.128.0.5
msk-donskaya-gw-1(dhcp-config)#exit
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.6.1 10.128.6.29
msk-donskaya-gw-1(config)#ip dhcp excluded-address 10.128.6.200 10.128.6.254

![Картинка 4](image/4.png){#fig:004 width=70%}

![Картинка 5](image/5.png){#fig:005 width=70%}


Шаг 4. Настройка оконечных устройств и проверка работы (Результаты)
На всех компьютерах (ПК) в локальной сети я изменила настройку IP-конфигурации со Static на DHCP. 

![Картинка 6](image/6.png){#fig:006 width=70%}

Для проверки корректности я выполнила команду:

![Картинка 7](image/7.png){#fig:007 width=70%}

В режиме симуляции (Simulation Mode) я изучила процесс обмена DHCP. Я наблюдала классический процесс DORA:

DHCP Discover — широковещательный запрос от ПК для поиска DHCP-сервера.
DHCP Offer — ответ от маршрутизатора с предложением IP-адреса.
DHCP Request — запрос от ПК на использование предложенного адреса.
DHCP ACK — подтверждение от сервера о закреплении адреса за ПК.
Также я успешно проверила связность сети с помощью команды ping между узлами из разных подсетей и к доменным именам (например, ping www.donskaya.rudn.ru), что подтвердило работоспособность как DHCP, так и DNS серверов.

![Картинка 8](image/8.png){#fig:008 width=70%}

# Вывод  

В ходе лабораторной работы были приобретены практические навыки по настройке динамического распределения IP-адресов посредством протокола DHCP (Dynamic Host Configuration
Protocol) в локальной сети.


# Источники

1. 802.1D-2004 - IEEE Standard for Local and Metropolitan Area Networks.
Media Access Control (MAC) Bridges : тех. отч. / IEEE. — 2004. — С. 1—
277. — DOI: 10.1109/IEEESTD.2004.94569. — URL: http://ieeexplore.
ieee.org/servlet/opac?punumber=9155.
2. 802.1Q - Virtual LANs. — URL: http://www.ieee802.org/1/pages/802.
1Q.html.
3. A J. Packet Tracer Network Simulator. — Packt Publishing, 2014. —
ISBN 9781782170426. — URL: https://books.google.com/books?id=
eVOcAgAAQBAJ&dq=cisco+packet+tracer&hl=es&source=gbs_navlinks_
s.
4. Cotton M., Vegoda L. Special Use IPv4 Addresses : RFC / RFC Editor. —
01.2010. — С. 1—11. — № 5735. — DOI: 10.17487/rfc5735. — URL: https:
//www.rfc-editor.org/info/rfc5735.
5. Droms R. Dynamic Host Configuration Protocol : RFC / RFC Editor. —
03.1997. — С. 1—45. — № 2136. — DOI: 10.17487/rfc2131. — URL: https:
//www.ietf.org/rfc/rfc2131.txt%20https://www.rfc-editor.org/
info/rfc2131.
6. McPherson D., Dykes B. VLAN Aggregation for Efficient IP Address
Allocation, RFC 3069. — 2001. — URL: http : / / www . ietf . org / rfc /
rfc3069.txt.
7. Moy J. OSPF Version 2 : RFC / RFC Editor. — 1998. — С. 244. — DOI: 10.
17487/rfc2328. — URL: https://www.rfc-editor.org/info/rfc2328.
8. NAT Order of Operation. — URL: https://www.cisco.com/c/en/us/
support/docs/ip/network-address-translation-nat/6209-5.html.
9. NAT: вопросы и ответы / Сайт поддержки продуктов и технологий
компании Cisco. — URL: https://www.cisco.com/cisco/web/support/
RU/9/92/92029_nat-faq.html.
10. Neumann J. C. Cisco Routers for the Small Business A Practical Guide for
IT Professionals. — Apress, 2009.