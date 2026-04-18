---
## Front matter
title: "Лабораторная работа №10"
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

Освоить настройку прав доступа пользователей к ресурсам сети.

# Выполнение лабораторной работы

Был настроен ноутбук администратора с IP-адресом 10.128.6.200 и соответствующими параметрами шлюза и DNS. Все правила фильтрации трафика настраивались на центральном маршрутизаторе msk-donskaya-gw-1.

![Меняем adm](image/1.png){#fig:001 width=70%}

Для управления трафиком был создан расширенный именованный список доступа servers-out, который был применен к исходящему трафику интерфейса, ведущего к подсети серверов.

Были созданы правила, разрешающие доступ к веб-сервисам, почтовым протоколам и DNS-серверу.
Для администратора были добавлены исключения, позволяющие использовать протоколы удаленного управления (Telnet, FTP).
Для корректной работы файлового сервера была настроена фильтрация по маске подсети (wildcard mask) для разделения доступа к SMB и FTP.
В начало списка было добавлено правило, разрешающее ICMP-трафик для возможности диагностики сети.

Для сегмента «Other» был создан ACL other-in, который блокирует любой исходящий трафик, за исключением запросов с устройства администратора. Список применен на входящий трафик интерфейса.
Для защиты сети управления оборудованием был создан ACL management-out, ограничивающий доступ к этому сегменту только для IP-адреса администратора.

![FTP](image/2.png){#fig:002 width=70%}

![Запрос по ip](image/3.png){#fig:003 width=70%}

![Запрос по имени](image/4.png){#fig:004 width=70%}

![Ввод комманд](image/5.png){#fig:005 width=70%}

![Ввод комманд](image/6.png){#fig:006 width=70%}

![Ввод комманд](image/7.png){#fig:007 width=70%}

Результаты проверки

Проверка корректности настроек проводилась путем тестирования с разных узлов сети:

Веб-доступ: Подтверждено, что обычные пользователи видят веб-страницу, но не имеют доступа по FTP. Администратор имеет полный доступ.
Сетевые службы: Проверена работоспособность DNS-запросов и почтовых протоколов.
Изоляция: Подтверждено, что устройства из сети «Other» не могут выходить за пределы своего сегмента, в то время как администратор сохраняет доступ.
Диагностика: Команда ping работает во всех разрешенных направлениях.
Управление: Доступ к сетевому оборудованию ограничен и доступен только с ноутбука администратора.

![Ввод комманд](image/8.png){#fig:008 width=70%}

![Проверка](image/9.png){#fig:009 width=70%}


# Вывод  

В ходе лабораторной работы была освоена настройка прав доступа пользователей к ресурсам сети.


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