---
## Front matter
title: "Научная практика"
subtitle: "Установка интрнет-соединения с NAT через маршрутизатор"
author: "Хватов Максим Григорьевич"

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

Установить соединение с NAT для получения доступа в интрнет

# Выполнение работы

Здесь я установил следующие интерфейсы: 

- eth0 — 192.168.10.124/24 — (внутренняя сеть, клиенты)

- eth1 — 192.168.122.10/24 — (в сторону NAT)

![Интерфейсы VYOS](image/1.png){#fig:001 width=70%}

Далее пробую пропинговать адрес NAT 192.168.122.1, которое прошло успешно

![Пингование 192.168.122.1](image/2.png){#fig:004 width=70%}

Проверил маршрут по умолчанию

![Вывод резултата проверки маршрута по умолчанию](image/3.png){#fig:003 width=70%}

Проверка NAT-правила

![Результат проврки NAT-правила](image/4.png){width=70%}

Теперь я проверил маршрут на сервере, пропинговал адрес NAT (рис. [-@fig:005]) и 8.8.8.8 (рис. [-@fig:006]).

Информация об IP на сервере:

![Реузльтат команды ip route](image/7.png){#fig:007 width=70%}

![Пингование 192.168.122.1](image/5.png){#fig:005 width=70%}

![Пингование 8.8.8.8](image/6.png){#fig:006 width=70%}

Дописал в файл resolved.conf 

```
DNS=8.8.8.8
FallbackDNS=1.1.1.1
```

![Содержимое resolved.conf](image/8.png){#fig:008 width=70%}

![Проверка IP-форвардинга в vyos](image/9.png){#fig:009 width=70%}

Далее я попробовал удалить лишний маршрут, прописал правильный и сохранил изменения, далее пропинговал через traceroute 8.8.8.8

![Доступа в интернет всё также нет](image/10.png){#fig:010 width=70%}

При это до 192.168.122.1 (NAT) достучаться получается. 

Далее я попробовал использовать следующущю конфигурацию:

![Доступ в интрнет получен](image/11.png){#fig:011 width=70%}

Дальше я протестировал tftp, создав файл testfile.txt в директории /srv/tftp/
На другой машине из этой же сети получил его, скачав предварительно соответствующий пакет

![Результат получения файла через tftp](image/12.png){#fig:012 width=70%}

Дальше я установил sali, скачав пакет с gitlab.

![Вывод команды ls](image/13.png){#fig:013 width=70%}

# Выводы

В результате проделанной работы я получил доступ в интрнет, а также склонировал на сервер пакет sali из gitlab.