---
## Front matter
lang: ru-RU
title: Лабораторная работа №6
author:
  - Морозова Ульяна
institute:
  - Российский университет дружбы народов, Москва, Россия

date: 22 november 2026

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
- \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

## **Цель работы**

Освоить методы создания внешних библиографических баз данных формата `.bib` и автоматического формирования внутритекстовых ссылок и списков литературы в LaTeX с использованием `natbib` + `BibTeX` и `biblatex` + `Biber`.

## Использование `natbib` + `BibTeX` и проблема кодировки кириллицы 1/3

Сначала мы протестировали работу `natbib` + `BibTeX` с использованием стиля библиографии `plainnat` (не нумерованный список) и команд текстового (`\citet`) и скобочного (`\citep`) цитирования

![Ошибка кодировки кириллицы в `natbib` и структура файла `dect.bib`](image/1.png){ #fig:001 width=95% }

## Использование `natbib` + `BibTeX` и проблема кодировки кириллицы 2/3

![Промежуточный этап компиляции `natbib`: неразрешённые ссылки `[?]` до повторного прогона `pdfLaTeX`](image/2.png){ #fig:002 width=80% }

## Использование `natbib` + `BibTeX` и проблема кодировки кириллицы 3/3

После завершения полного цикла компиляции (`pdfLaTeX` - `BibTeX` - `pdfLaTeX` - `pdfLaTeX`) все перекрёстные ссылки корректно разрешились в формат «автор–год».

![Результат успешной компиляции `natbib` со стилем `plainnat`](image/3.png){ #fig:003 width=95% }


## Поведение `natbib` при цитировании отсутствующего ключа

![Отображение отсутствующего ключа цитирования `[?]` в `natbib`](image/4.png){ #fig:004 width=95% }

## Использование `biblatex` + `Biber` 1/2

![Промежуточный этап компиляции `biblatex`: отображение ключей полужирным шрифтом до запуска `biber`](image/5.png){ #fig:005 width=95% }

## Использование `biblatex` + `Biber` 2/2

Псоле выполнения цепочки `pdfLaTeX` - `biber` - `pdfLaTeX` документ успешно скомпилировался: в список литературы корректно включена и отсортирована кириллическая публикация «Морозова, У. К. и Ю. В. Гайдамака (2025)».

![Успешная компиляция `biblatex` со стилем `authoryear` и поддержкой кириллицы](image/6.png){ #fig:006 width=95% }


## Переключение на числовой стиль (`style=numeric`) в `biblatex`

![Результат компиляции `biblatex` с опцией `style=numeric`](image/8.png){ #fig:008 width=95% }

## Выводы

В ходе практической работы мы на практике изучили два основных подхода к управлению библиографией в LaTeX (`natbib` + `BibTeX` и `biblatex` + `Biber`).

::: {#refs}
:::