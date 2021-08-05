---
title: Что такое OLAP?
toc_hidden: true
toc_priority: 100
---

# Что такое OLAP? {#what-is-olap}

[OLAP](https://ru.wikipedia.org/wiki/OLAP) (OnLine Analytical Processing) переводится как обработка данных в реальном времени. Это широкий термин, который можно рассмотреть с двух сторон: с технической и с точки зрения бизнеса. Для самого общего понимания можно просто прочитать его с конца:

**Processing**
    Обрабатываются некие исходные данные…

**Analytical**
:   … чтобы получить какие-то аналитические отчеты или новые знания…

**OnLine**
:   … в реальном времени, практически без задержек на обработку.

## OLAP с точки зрения бизнеса {#olap-from-the-business-perspective}

В последние годы бизнес-сообщество стало осознавать ценность данных. Компании, которые принимают решения вслепую, чаще всего отстают от конкурентов. Управление бизнесом на основе данных, которое применяется успешными компаниями, побуждает собирать все данные, которые могут быть полезны в будущем для принятия бизнес-решений, а также подбирать механизмы, чтобы своевременно эти данные анализировать. Именно для этого и нужны СУБД с OLAP. 

С точки зрения бизнеса, OLAP позволяет компаниям постоянно планировать, анализировать и оценивать операционную деятельность, чтобы повышать её эффективность, уменьшать затраты и как следствие — увеличивать долю рынка. Это можно делать как в собственной системе, так и в облачной (SaaS), в веб или мобильных аналитических приложениях, CRM-системах и т.д. Технология OLAP используется во многих приложениях BI (Business Intelligence — бизнес-аналитика).

ClickHouse — это СУБД с OLAP, которая часто используется для поддержки SaaS-решений для анализа данных в различных предметных областях. Но поскольку некоторые компании все еще не слишком охотно размещают свои данные в облаке (у сторонних провайдеров), ClickHouse может быть развернут и на собственных серверах заказчика.

## OLAP с технической точки зрения {#olap-from-the-technical-perspective}

Все СУБД можно разделить на две группы: OLAP (**аналитическая** обработка в реальном времени) и OLTP (обработка **транзакций** в реальном времени). OLAP используются для построения отчетов на основе больших объемов накопленных исторических данных, но эти отчеты обновляются не слишком часто. OLTP обычно применяются для обработки непрерывных потоков операций (транзакций), каждая из которых изменяет состояние данных.

На практике OLAP и OLTP — это не строго разделённые категории, а скорее спектр возможностей. Большинство СУБД специализируются на каком-то одном виде обработки данных, но имеют инструменты и для выполнения других операций, когда это необходимо. Из-за такой специализации часто приходится использовать несколько СУБД и интегрировать их между собой. Это вполне реальная и решаемая задача, но, как известно, чем больше систем, тем выше расходы на их содержание. Поэтому в последние годы становятся популярны гибридные СУБД — HTAP (**Hybrid Transactional/Analytical Processing**), которые одинаково эффективно выполняют оба вида операций по обработке данных.

Даже если СУБД сначала развивались исключительно как OLAP или как OLTP, разработчики постепенно двигаются в сторону HTAP, чтобы сохранять конкурентоспособность. И ClickHouse не исключение. Изначально он создавался как [OLAP СУБД с максимальной производительностью](../../faq/general/why-clickhouse-is-so-fast.md), и на сегодняшний день в нем нет полноценной поддержки обработки тразакций, но уже реализованы некоторые возможности, такие как постоянная скорость чтения/записи данных и мутации при изменении/удалении данных.

Принципиальное "разделение труда" между OLAP и OLTP СУБД сохраняется:

-   Чтобы эффективно строить аналитические отчеты, нужно уметь обрабатывать колонки по отдельности, поэтому большинство OLAP СУБД — [столбцовые](../../faq/general/columnar-database.md).
-   Хранение данных по столбцам снижает скорость выполнения операций над строками (таких как добавление или изменение данных) пропорционально числу столбцов, а это число может быть огромным для систем, ориентированных на сбор разнообразных детальных данных о событиях. Поэтому большинство OLTP систем используют строковые СУБД.