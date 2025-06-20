# ETL-пайплайн для работы с Avito-объявлениями через автозагрузку

## Описание проекта
Данный проект реализует полноценный ETL-процесс (Extract → Transform → Load) для обработки данных автозагрузки объявлений на платформу Avito.
Используется в ежедневной работе компании, занимающейся продажей техники на Avito.
Обрабатывает объёмы более 100 000 объявлений.

### Функциональность пайплайна:
• Автоматическая очистка и нормализация данных.
• Проверка уникальности и целостности.
• Валидация дилерских ограничений по городам и брендам.
• Логирование ключевых проблем для ручного анализа.
• Вывод аналитической информации для корректировки бизнес-стратегии.

### Цели, которые решает проект:
• Упростить, ускорить и стандартизировать процесс обработки данных.
• Повысить точность и эффективность размещения объявлений.
• Предоставить бизнесу данные для принятия решений по построению стратегии продаж.

## Стек
- Python 3.10+
- pandas
- openpyxl

## Структура проекта
```bash
etl_avito/
│
├── data/                    # папка
├── extract.py               # extract
├── extract.py               # extract
├── extract.py               # extract

├── transform.py             # очистка, нормализация, валидация, фильтрация
├── load.py                  # сохранение результатов
├── dealerships.py           # справочник городов и брендов
├── requirements.txt         # зависимости
├── README.md                # описание проекта
└── scripts/                 # отдельные скрипты
```

## Этапы обработки
### 1. Extract
Чтение Excel-файла с объявлениями (`.xlsx`).

### 2. Transform
- удаление мусорных/пустых строк
- проверка на уникальность значений столбцов `AvitoId` и `Id`
- нормализация строк по столбцу `Title`
- фильтрация и корректировка значений `Address` в соответствии со справочником городов
- удаление строк, нарушающих дилерство
- обеспечение полного размещения техники по разрешённым адресам

Все действия логируются. Проблемные записи (дубликаты, неизвестные адреса, нарушения) сохраняются отдельно или выводятся в лог.

### 3. Load
Создание на основе обработанного DataFrame реляционной базы данных для дальнейших действий - создания дашбордов, аналитики, выполнение SQL-запросов.

## Логика дилерства
Справочник `dealerships` указывает, в каких городах разрешено размещение объявлений для каждого бренда. Все нарушения автоматически отсеиваются и логируются по `AvitoID`.

## Установка
```bash
pip install -r requirements.txt
```
## Дополнительно
Папка `scripts` содержит набор различных практических единичных скриптов для обработки и автоматизации данных из `Avito` через Автозагрузку.
Решают задачи нормализации описаний, фильтрации, проверки и замены определенных изображений, работы с API Avito.

# To do:
LOAD
1) запуска автозагрузки файла через авито апи (по необходимости через параметр в CLI или флаг)
2) создать на основе обработанного датафрейма реляционную базу данных по нормальной форме NF3
3) создать schema.sql - документация по структуре БД

BI (дашборд)
1) сводная информация - об изменениях по результатам выполнения ETL
2) сколько всего объявлений
3) сколько объявлений по каждой категории - мототехника, квадро и т.п. (сделать график)
4) сколько объявлений по каждому адресу (сделать график)
5) сколько объявлений с прикрепленным видео (столбец VideoURL)
6) сколько объявлений с прикрепленным коротким видео (столбец VideoFilesURL)

Utility
1) Unit-тесты
2) Docker
3) CI/CD
4) Airflow
5) Уведомление на Telegram о результатах работы DAG
6) Метрики производительности обработки 100 000 записей
8) Писать логи в файлы
9) HTML-отчёт, пригодный для отправки по почте/Telegram
