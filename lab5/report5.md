---
## Front matter
title: "Лабораторная работа №5"
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

Получить основные навыки по настройке VLAN на коммутаторах сети.

# Выполнение лабораторной работы

## 1. Исходные данные и топология сети
В соответствии с заданием была собрана топология сети в симуляторе Cisco Packet Tracer. 

![Картинка 1](image/1.png){#fig:001 width=70%}

**Шаг 1. Настройка главного коммутатора (VTP-сервера) и магистральных портов**
Для централизованного управления VLAN коммутатор `msk-donskaya-ramikhailova-sw-1` был настроен в режиме VTP Server. Задано имя домена `donskaya` и пароль `cisco`. В базе данных коммутатора были созданы следующие VLAN с соответствующими именами: 2 (management), 3 (servers), 101 (dk), 102 (departaments), 103 (adm), 104 (other). 
Интерфейсы, связывающие коммутаторы между собой (f0/24, g0/1, g0/2), переведены в режим Trunk, на них разрешена передача трафика соответствующих VLAN.

![Картинка 2](image/2.png){#fig:002 width=70%}

Для проверки успешного создания виртуальных локальных сетей была выполнена команда `show vlan`.

![Картинка 16](image/16.png){#fig:016 width=70%}

**Шаг 2. Настройка коммутаторов-клиентов VTP и портов доступа**
Остальные коммутаторы сети были переведены в режим VTP Client с указанием того же домена и пароля. Это позволило им автоматически получить информацию о созданных VLAN от сервера. 
Магистральные порты были настроены в режим Trunk. Порты, к которым подключены конечные устройства (серверы и ПК), переведены в режим доступа (Access) и привязаны к соответствующим VLAN согласно заданию.

![Картинка 3](image/3.png){#fig:003 width=70%}

![Картинка 4](image/4.png){#fig:004 width=70%}

![Картинка 5](image/5.png){#fig:005 width=70%}

![Картинка 6](image/6.png){#fig:006 width=70%}

Для проверки того, что настройки VTP применились успешно и порты привязаны верно, на клиентском коммутаторе SW-2 была вызвана таблица VLAN. Как видно из вывода, коммутатор получил информацию о всех сетях, а порты Fa0/1 и Fa0/2 успешно назначены в VLAN 3 (servers).

![Картинка 17](image/17.png){#fig:017 width=70%}

**Шаг 3. Настройка IP-адресации конечных устройств**
В соответствии с таблицами маршрутизации серверам и рабочим станциям были назначены статические IP-адреса, маски подсети и адреса шлюзов по умолчанию.

![Картинка 7](image/7.png){#fig:007 width=70%}

![Картинка 8](image/8.png){#fig:008 width=70%}

![Картинка 9](image/9.png){#fig:009 width=70%}

![Картинка 10](image/10.png){#fig:010 width=70%}

![Картинка 11](image/11.png){#fig:011 width=70%}

![Картинка 12](image/12.png){#fig:012 width=70%}

![Картинка 13](image/13.png){#fig:013 width=70%}

![Картинка 14](image/14.png){#fig:014 width=70%}

![Картинка 15](image/15.png){#fig:015 width=70%}

**Шаг 4. Проверка связности сети**
Согласно принципам работы VLAN, устройства, находящиеся в одном широковещательном домене (в одном VLAN), должны пинговать друг друга. Устройства в разных VLAN не должны иметь прямой связи без использования маршрутизатора (которого в данной топологии нет).

Был проведен тест с компьютера `dk-pavlovskaya-ramikhailova-1`:
1. Пинг на адрес `10.128.3.3` прошел успешно (0% loss), так как оба устройства находятся в одной подсети и одном VLAN.
2. Пинг на адрес `10.128.4.2` завершился ошибкой "Request timed out" (100% loss), так как целевое устройство находится в другом VLAN, и трафик изолируется на уровне коммутаторов.

![Картинка 18](image/18.png){#fig:018 width=70%}

## Изучение процесса передвижения пакета ICMP в режиме симуляции

В ходе выполнения задания в режиме симуляции был успешно отслежен обмен сообщениями протокола ICMP.
Было установлено, что процесс проверки связности состоит из двух этапов: отправки пакета с типом ICMP 8 (Echo Request) и получения пакета с типом ICMP 0 (Echo Reply). Анализ PDU подтвердил, что при формировании ответа сетевые уровни инвертируют адреса источника и назначения (как MAC, так и IP). Наличие заголовков Ethernet 802.1q с идентификатором TCI 0x0002 доказывает, что трафик между коммутаторами передается тегированным в рамках VLAN 2.

![Картинка 19](image/19.png){#fig:019 width=70%}

![Картинка 20](image/20.png){#fig:020 width=70%}

![Картинка 21](image/21.png){#fig:021 width=70%}

![Картинка 22](image/22.png){#fig:022 width=70%}

# Вывод  

В ходе лабораторной работы были получены основные навыки по настройке VLAN на коммутаторах сети.

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