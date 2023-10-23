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
    ports: 
      - 5432:5432
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: password
      POSTGRES_DB: netology
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

![Задание1](https://github.com/SSitkarev/06-db-02-sql/blob/main/img/1.jpg)