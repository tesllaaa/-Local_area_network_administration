---
## Front matter
title: "Лабораторная работа №7"
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

Получить навыки работы с физической рабочей областью Packet Tracer, а также учесть физические параметры сети.

# Выполнение лабораторной работы

2. Перейдите в физическую рабочую область Packet Tracer. Присвойте название городу — Moscow.

![Картинка 1](image/1.png){#fig:001 width=70%}

3. Присвойте ему название Donskaya. Добавьте здание для территории Pavlovskaya.

![Картинка 2](image/2.png){#fig:002 width=70%}

4. Щёлкнув на изображении здания Donskaya, переместите изображение, обозначающее серверное помещение, в него.

![Картинка 3](image/3.png){#fig:003 width=70%}

6. Переместите коммутатор msk-pavlovskaya-sw-1 и два оконечных устройства dk-pavlovskaya-1 и other-pavlovskaya-1 на территорию Pavlovskaya,  
используя меню Move физической рабочей области Packet Tracer.

![Картинка 4](image/4.png){#fig:004 width=70%}

![Картинка 5](image/5.png){#fig:005 width=70%}

7. Вернувшись в логическую рабочую область Packet Tracer, пропингуйте с коммутатора msk-donskaya-sw-1 коммутатор msk-pavlovskaya-sw-1.
Убедитесь в работоспособности соединения.

![Картинка 6](image/6.png){#fig:006 width=70%}

8. В меню Options , Preferences во вкладке Interface активируйте разрешение на учёт физических характеристик среды передачи (Enable Cable Length
Effects).

9. В физической рабочей области Packet Tracer разместите две территории на
расстоянии более 100 м друг от друга (рекомендуемое расстояние — около
1000 м или более).

![Картинка 7](image/7.png){#fig:007 width=70%}

10. Вернувшись в логическую рабочую область Packet Tracer, пропингуйте с коммутатора msk-donskaya-sw-1 коммутатор msk-pavlovskaya-sw-1.
Убедитесь в неработоспособности соединения.

![Картинка 8](image/8.png){#fig:008 width=70%}

11. Удалите соединение между msk-donskaya-sw-1 и msk-pavlovskaya-sw-1.
Добавьте в логическую рабочую область два повторителя (RepeaterPT). Присвойте им соответствующие названия msk-donskaya-mc-1
и msk-pavlovskaya-mc-1. Замените имеющиеся модули на PT-REPEATERNM-1FFE и PT-REPEATER-NM-1CFE для подключения оптоволокна
и витой пары по технологии Fast Ethernet.

![Картинка 9](image/9.png){#fig:009 width=70%}

![Картинка 10](image/10.png){#fig:010 width=70%}

12. Переместите msk-pavlovskaya-mc-1 на территорию Pavlovskaya (в физической рабочей области Packet Tracer).

![Картинка 11](image/11.png){#fig:011 width=70%}

13. Подключите коммутатор msk-donskaya-sw-1 к msk-donskaya-mc-1 по витой паре, msk-donskaya-mc-1 и msk-pavlovskaya-mc-1 — по оптоволокну,
msk-pavlovskaya-sw-1 к msk-pavlovskaya-mc-1 — по витой паре 

![Картинка 13](image/13.png){#fig:013 width=70%}

14. Убедитесь в работоспособности соединения между msk-donskaya-sw-1
и msk-pavlovskaya-sw-1.

![Картинка 14](image/14.png){#fig:014 width=70%}

# Вывод  

В ходе лабораторной работы были получены навыки работы с физической рабочей областью Packet Tracer.


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