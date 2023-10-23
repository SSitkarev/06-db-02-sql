# Домашнее задание к занятию 2. «SQL» - Сергей Ситкарёв

## Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose-манифест.

```yaml
version: "3.8"

services:
  database:
    image: postgres:12
    container_name: pg_database
    user: root
    ports: 
      - 5432:5432
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: password
    volumes:
      - data:/var/lib/postgresql/data
      - backup:/var/lib/postgresql/backup
    networks:
      clusternetwork:
        ipv4_address: 172.30.240.10
      
volumes:
  data:
  backup:

networks:
  clusternetwork:
    name: local
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 172.30.240.0/24
```

## Задача 2

В БД из задачи 1:

создайте пользователя test-admin-user и БД test_db;

в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);

предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;

создайте пользователя test-simple-user;

предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.

Приведите:

итоговый список БД после выполнения пунктов выше;

![Задание2](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/2-1.jpg)

описание таблиц (describe);

![Задание2](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/2-2.jpg)

SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;

список пользователей с правами над таблицами test_db.

![Задание2](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/2-3.jpg)

## Задача 3

Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными:

Таблица orders

Наименование	цена
Шоколад	10
Принтер	3000
Книга	500
Монитор	7000
Гитара	4000
Таблица clients

ФИО	Страна проживания
Иванов Иван Иванович	USA
Петров Петр Петрович	Canada
Иоганн Себастьян Бах	Japan
Ронни Джеймс Дио	Russia
Ritchie Blackmore	Russia

Используя SQL-синтаксис:

вычислите количество записей для каждой таблицы.
Приведите в ответе:

- запросы,
- результаты их выполнения.

![Задание3](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/3.jpg)

# Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys, свяжите записи из таблиц, согласно таблице:

ФИО	Заказ
Иванов Иван Иванович	Книга
Петров Петр Петрович	Монитор
Иоганн Себастьян Бах	Гитара
Приведите SQL-запросы для выполнения этих операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса.

Подсказка: используйте директиву UPDATE.

![Задание4](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/4.jpg)

# Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).
Приведите получившийся результат и объясните что значат полученные значения.

![Задание5](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/5.jpg)

Эта команда отображает план выполнения, который PostgreSQL planner генерирует для предоставленной инструкции.
В нашем случае была построчно прочитана таблица "orders", создан кэш по столбцу "id", прочитана таблица "clients", для каждой строки по столбцу "заказ" проверено соответствие с кэшем.
Так же видна "стоимость"(нагрузка) запроса.

# Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).

![Задание6](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/6-1.jpg)

Остановите контейнер с PostgreSQL, но не удаляйте volumes.

- Удалил контейнер и volume data.

Поднимите новый пустой контейнер с PostgreSQL.

- Заново запустил контейнер и volume data (volume backup сохранил)

Восстановите БД test_db в новом контейнере.

![Задание6](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/6-2.jpg)

- База восстановлена 

![Задание6](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/6-3.jpg)