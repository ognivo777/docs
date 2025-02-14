# Формат входных данных {{ brand-voice-full-name }} Adaptive

{{ brand-voice-name }} Adaptive — это платформа для построения диалоговых роботов. Она синтезирует фразы на основе записей голоса диктора по шаблонам. Шаблоны состоят из двух частей: 
* Статическая часть — структура и интонация произносимой фразы.
* Динамическая часть — переменная часть фразы, которая зависит от каждого конкретного диалога.

Для обучения голосовой модели необходимы:
* ZIP-архив аудиозаписей в формате [WAV](https://ru.wikipedia.org/wiki/WAV), в котором содержатся:
   1. Основной набор (базовая корзина) состоит из фраз, специально подготовленных командой {{ yandex-cloud }}, которые нужно проговорить и записать. Его изменение невозможно, допускается только пропуск фраз.
   1. Набор для построения фраз (специфичная корзина). В качестве текстов нужны записи шаблонов, которые потом необходимо синтезировать, с учетом переменных. Этот набор формируется на основе пользовательских данных и должен пройти рецензирование перед началом записи.
* Текстовые расшифровки аудиозаписей в виде таблицы [TSV](https://ru.wikipedia.org/wiki/TSV). Таблица с текстами должна состоять из двух колонок:
   1. `recording` — имя файла с аудиозаписью, на которой диктор произносит текст — шаблон с подставленной переменной.
   1. `text`  — строка, содержащая дословную расшифровку аудиозаписи. Переменная часть шаблона должна быть представлена в формате `{variable_name = variable_value}`. Имена переменных не должны меняться в рамках одного шаблона.

## Требования к аудиофайлам {#audio-requirements}

* Одна фраза — одна запись (файл). 
* Неточности, микроповторы, оговорки и замены слов недопустимы.
* Для наименования файлов необходимо использовать номер фразы.

В качестве входных данных принимаются файлы с аудиозаписями в формате [LPCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) с WAV-заголовком.

Характеристики аудио:
* Дискретизация — 48 кГц.
* Разрядность квантования — 16 бит.
* Порядок байтов — обратный (little-endian).
* Аудиоданные хранятся как знаковые числа (signed integer). 
* Количество каналов — 1 (моно).

## Требования к текстам {#text-requirements}

1. Все данные должны быть нормализованы, то есть строки текста для синтеза не могут содержать цифр и сокращений вида `ул. Строителей, кв.15 к.3` или `13 руб 10 коп`. Цифры и числа необходимо записать прописью, сокращения — полностью раскрыть. 
   
1. В словах-омографах, где ударение может быть поставлено неоднозначно, необходимо явно указать ударную гласную знаком `+`.
   > _<q>Он вставил ключ в зам+ок.</q>_ — Ударение падает на второй слог.
   > _<q>Михайловский з+амок — главный корпус Пушкинского музея.</q>_ — Ударение падает на первый слог.

1. Обязательно явно указывать букву <q>ё</q>.

1. В вопросительных предложениях необходимо указать, на какое слово приходится \*\*логическое ударение\*\* — вопросительная интонация предложения. 

   {% note warning %}

   Логическое ударение не должно падать на переменную часть фразы.

   {% endnote %}

   > Предложение <q>Кот пошел в лес?</q> можно прочитать тремя разными способами:
   > * \*\*Кот\*\* пошел в лес? — со смыслом <q>Кто пошел в лес? Это был действительно кот?</q>
   > * Кот \*\*пошел\*\* в лес? — со смыслом <q>Кот пошел или побежал?</q> или <q>Было ли совершено само действие? Кот ушел или нет?</q>
   > * Кот пошел \*\*в лес\*\*? — со смыслом <q>Куда или за чем пошел кот? В лес, в школу, за колбасой?</q>
   >
   > Во всех предложениях логическое ударение выделяет основной смысл предложения.

### Пример сводной текстовой таблицы {#example}

| recording | text |
|---|---|
| 0.wav | \*\*Кот\*\* пошел {place=в школу}? |
| 1.wav | \*\*Кот\*\* пошел {place=за колбасой}? |
| 2.wav | Мы предлагаем вам замечательную книгу {book_name=Русские народные сказки} всего за \{price=тысяча сто один рубль одиннадцать копеек}. |
| 3.wav | Мы предлагаем вам замечательную книгу {book_name=Приключения Алисы в Стране чудес} всего за \{price=двести тридцать пять рублей}. |
| 4.wav | Мы предлагаем вам замечательную книгу {book_name=Евгений Онегин} всего за \{price=пятьсот рублей сорок восемь копеек}.  |
| 5.wav | Вы записаны на {date=завтра} в \{time=пятнадцать ноль ноль}. |
| 6.wav | Вы записаны на {date=четвёртое октября} в \{time=девять утра}. |
