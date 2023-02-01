# Разворачиваем keycloak в докере
Сначала развернем PostgeSQL:
```
docker run --rm --name postgres15 -p 5432:5432 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -d postgres:15
```
Теперь развернем сам Keycloak:
```
docker run -d --rm --name keycloak -p 8080:8080 \
        -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin \
        -e KC_HTTP_ENABLED=true -e KC_HOSTNAME_STRICT=false -e KC_HOSTNAME_STRICT_HTTPS=false \
        quay.io/keycloak/keycloak:17.0.1 \
        start \
        --auto-build \
        --db=postgres \
        --db-url=jdbc:postgresql://192.168.0.103:5432/admin --db-username=admin --db-password=admin --http-enabled=true --http-port=8080
```
как там поживает Keyclok смотрим через логи:
```
docker logs e99ff2a00d7f9412bf35595a8f6814a6e1d81f4e43a05370a5f4c76f1ac200ff
```
где этот рабор цифр идентификатор контейнера.

# Заметки по работе с БД:
##### Подсоединиться к БД (порт по умолчанию 5432, но тут в докере он прокинут на 5433)
psql -h localhost -p 5433 -U postgres

##### Кирилица в терминале
psql -d mydb -U postgres
psql \! chcp 1251

CREATE DATABASE mydb;
DROP DATABASE mydb;
##### Подсоединиться к БД
\c mydb
##### Информация 
\conninfo

##### CREATE TABLE mew_table (Column name + data type + constraints (if any))
CREATE TABLE new_table (
id BIGSERIAL,
first_name VARCHAR(50),
last_name VARCHAR(50),
gender VARCHAR(50),
data_of_birth DATE
);

Просмтреть базы данных
\l
##### Просмтреть таблицы
\d
##### Просмтреть поля таблицы
\d new_table
##### Удалить таблицу
DROP TABLE new_table;

##### Заполняем таблицу
INSERT INTO new_table (date_of_birth)

VALUES ('200-01-01');

##### Заполнить таблицу с помощью сайта https://www.mockaroo.com/  и импортировать файл
\i D:/my_table.sql   #слеш должен быть прямой, а не как в windows