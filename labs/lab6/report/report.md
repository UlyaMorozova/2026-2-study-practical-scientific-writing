---
## Front matter
title: "Отчет по лабораторной работе №6"
subtitle: "*дисциплина: Computer Skills for Scientific Writing*"
author: "Морозова Ульяна"

## Generic options
lang: ru-RU
toc-title: "Содержание"
reference-section-title: "Список литературы"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: false # List of figures
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
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
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

# **Цель работы**

Освоить методы создания внешних библиографических баз данных формата `.bib` и автоматического формирования внутритекстовых ссылок и списков литературы в LaTeX с использованием `natbib` + `BibTeX` и `biblatex` + `Biber` [@practical_scientific_writing].

# Выполнение лабораторной работы

Мы подготовили библиографическую базу данных `dect.bib`, содержащая научные публикации и спецификацию стандарта по тематике выпускной квалификационной работы (моделирование узла сети DECT-2020 NR с помощью систем массового обслуживания в дискретном времени) [@ETSI103636-1; @Samuylov2024; @Asif2025; @Morozova2025Comparison; @Morozova2025AoI].

**1. Использование `natbib` + `BibTeX` и проблема кодировки кириллицы**

Сначала мы протестировали работу `natbib` + `BibTeX` с использованием стиля библиографии `plainnat` (не нумерованный список) и команд текстового (`\citet`) и скобочного (`\citep`) цитирования [@practical_scientific_writing].

При попытке включить в цитирование русскоязычную публикацию (`Morozova2025AoI`) при порядке кодировок `\usepackage[T2A,T1]{fontenc}` компилятор `pdfLaTeX` выдал ошибку `! LaTeX Error: Command \CYRU unavailable in encoding T1` (а также ошибку при обработке кириллицы восьмибитным процессором `BibTeX`). Это связано с тем, что активной по умолчанию становится последняя указанная кодировка (`T1`), не содержащая кириллических символов, а `BibTeX` рассчитан преимущественно на кодировку US-ASCII.

![Ошибка кодировки кириллицы в `natbib` и структура файла `dect.bib`](image/1.png){ #fig:001 width=95% }

После исключения кириллической записи из выборки `natbib` и выполнения первого прогона `pdfLaTeX` (до запуска `BibTeX` и повторных прогонов `pdfLaTeX`) все внутритекстовые ссылки отображаются в виде знаков вопроса `[?]`, несмотря на уже сформированный раздел *References*.

![Промежуточный этап компиляции `natbib`: неразрешённые ссылки `[?]` до повторного прогона `pdfLaTeX`](image/2.png){ #fig:002 width=80% }

После завершения полного цикла компиляции (`pdfLaTeX` - `BibTeX` - `pdfLaTeX` - `pdfLaTeX`) все перекрёстные ссылки корректно разрешились в формат «автор–год».

![Результат успешной компиляции `natbib` со стилем `plainnat`](image/3.png){ #fig:003 width=95% }

**2. Поведение `natbib` при цитировании отсутствующего ключа**

Чтобы проверить, как система LaTeX отреагирует на несуществующую запись в `dect.bib`, мы процитировали `\citep{NonExistentKey2026}`. При сборке `BibTeX` вывел предупреждение в журнал, а в итоговом PDF на месте ссылки отобразился символ `[?]`.

![Отображение отсутствующего ключа цитирования `[?]` в `natbib`](image/4.png){ #fig:004 width=95% }

**3. Использование `biblatex` + `Biber`**

На следующем этапе мы протестировали `biblatex` с бэкендом `biber` (команды `\textcite`, `\parencite`, `\autocite`). В отличие от `BibTeX`, процессор `Biber` без проблем работает с кодировкой UTF-8 и поддерживает сортировку смешанных англо-русских списков литературы [@practical_scientific_writing].

На первом этапе (после первого запуска `pdfLaTeX`, но до запуска `biber` и финального прогона `pdfLaTeX`) на месте ссылок в тексте отображаются ключи цитирования, выделенные полужирным шрифтом, а сам список литературы отсутствует.

![Промежуточный этап компиляции `biblatex`: отображение ключей полужирным шрифтом до запуска `biber`](image/5.png){ #fig:005 width=95% }

Псоле выполнения цепочки `pdfLaTeX` - `biber` - `pdfLaTeX` документ успешно скомпилировался: в список литературы корректно включена и отсортирована кириллическая публикация «Морозова, У. К. и Ю. В. Гайдамака (2025)».

![Успешная компиляция `biblatex` со стилем `authoryear` и поддержкой кириллицы](image/6.png){ #fig:006 width=95% }

**4. Проверка отсутствующего ключа и переключение на числовой стиль (`style=numeric`) в `biblatex`**

При добавлении ссылки на несуществующий ключ `\autocite{NonExistentKey2026}` в `biblatex` в итоговом PDF вместо знака вопроса выводится сам ключ цитирования полужирным шрифтом (**NonExistentKey2026**), что упрощает диагностику опечаток в ключах.

![Отображение отсутствующего ключа `(NonExistentKey2026)` в рабочем процессе `biblatex`](image/7.png){ #fig:007 width=95% }

Также в пакетах цитирования есть возможность использования нумерованных списков. Для этого при подключении пакета `biblatex` мы изменили стиль «автор–год» (`style=authoryear`) на числовой стиль (`style=numeric`). После сборки все команды цитирования (`\textcite`, `\parencite`, `\autocite`) автоматически перестроились на нумерованные ссылки в квадратных скобках (`[1]`, `[2]`, `[3, 5]`) без изменения основного текста документа.

![Результат компиляции `biblatex` с опцией `style=numeric`](image/8.png){ #fig:008 width=95% }

# Выводы

В ходе практической работы мы на практике изучили два основных подхода к управлению библиографией в LaTeX (`natbib` + `BibTeX` и `biblatex` + `Biber`).

# Список литературы

::: {#refs}
:::