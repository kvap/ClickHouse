#Семейство Log

Движки разработаны для сценариев, когда необходимо записывать много таблиц с небольшим объемом данных (менее 1 миллиона строк).

Движки семейства:

- [StripeLog](stripelog.md)
- [Log](log.md)
- [TinyLog](tinylog.md)

## Общие свойства

Движки:

- Хранят данные на диске.

- Добавляют данные в конец файла при записи.

- Не поддерживают операции [мутации](../../query_language/alter.md#alter-mutations).

- Не поддерживают индексы.

    Это означает, что запросы `SELECT` не эффективны для выборки диапазонов данных.

- Записывают данные не атомарно.

    Вы можете получить таблицу с повреждёнными данными, если что-то нарушит операцию записи (например, аварийное завершение работы сервера).

## Отличия

Движки `Log` и `StripeLog` поддерживают:

- Блокировки для конкурентного доступа к данным.

    Во время выполнения запроса `INSERT` таблица заблокирована и другие запросы на чтение и запись данных ожидают снятия блокировки. При отсутствии запросов на запись данных можно одновременно выполнять любое количество запросов на чтение данных.

- Параллельное чтение данных.

    ClickHouse читает данные в несколько потоков. Каждый поток обрабатывает отдельный блок данных.

Движок `Log` сохраняет каждый столбец таблицы в отдельном файле. Движок `StripeLog` хранит все данные в одном файле. Таким образом, движок `StripeLog` использует меньше дескрипторов в операционной системе, а движок `Log` обеспечивает более эффективное считывание данных.

Движок `TinyLog` самый простой в семье и обеспечивает самые низкие функциональность и эффективность. Движок `TinyLog` не поддерживает ни параллельного чтения данных, ни конкурентного доступа к данным. Он хранит каждый столбец в отдельном файле. Движок читает данные медленнее, чем оба других движка с параллельным чтением, и использует почти столько же дескрипторов, сколько и движок `Log`. Его можно использовать в простых сценариях с низкой нагрузкой.

[Оригинальная статья](https://clickhouse.yandex/docs/ru/operations/table_engines/log_family/) <!--hide-->
