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







