# Домашнее задание к занятию «Работа с данными (DDL/DML)». Потапчук Сергей.

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp. 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

1.4. Дайте все права для пользователя sys_temp. 

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

### Решение

```sql
CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'sys_temp';
SELECT user, host FROM mysql.user;
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';
SHOW GRANTS FOR 'sys_temp'@'localhost' \G
```

![](img/img-01-01.png)

![](img/img-01-02.png)

```Bash
mysql -u sys_temp -p
```

![](img/img-01-03.png)

```Bash
wget https://downloads.mysql.com/docs/sakila-db.zip
unzip sakila-db.zip
```

![](img/img-01-04.png)

Проверим, создается ли в скрипте БД

```Bash
head -30 sakila-schema.sql | grep -i "create database"
```

![](img/img-01-05.png)

Пустой вывод означает, что нужно создать базу вручную.

```sql
CREATE DATABASE sakila;
USE sakila;
SOURCE /home/sergey/sakila-db/sakila-schema.sql
```

![](img/img-01-06.png)

Или по-другому

```Bash
mysql -u sys_temp -p sakila < sakila-db/sakila-data.sql
```

![](img/img-01-07.png)

```sql
USE sakila;
SHOW TABLES;
SELECT * FROM actor LIMIT 5;
```

![](img/img-01-08.png)

![](img/img-01-09.png)

---

### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```

### Решение

```sql
SELECT 
    TABLE_NAME,
    COLUMN_NAME,
    ORDINAL_POSITION
FROM 
    information_schema.KEY_COLUMN_USAGE
WHERE 
    TABLE_SCHEMA = 'sakila' 
    AND CONSTRAINT_NAME = 'PRIMARY'
ORDER BY 
    TABLE_NAME, 
    ORDINAL_POSITION;
```

![](img/img-02-01.png)

| TABLE_NAME    | COLUMN_NAME  | ORDINAL_POSITION |
|---------------|--------------|------------------|
| actor         | actor_id     |                1 |
| address       | address_id   |                1 |
| category      | category_id  |                1 |
| city          | city_id      |                1 |
| country       | country_id   |                1 |
| customer      | customer_id  |                1 |
| film          | film_id      |                1 |
| film_actor    | actor_id     |                1 |
| film_actor    | film_id      |                2 |
| film_category | film_id      |                1 |
| film_category | category_id  |                2 |
| film_text     | film_id      |                1 |
| inventory     | inventory_id |                1 |
| language      | language_id  |                1 |
| payment       | payment_id   |                1 |
| rental        | rental_id    |                1 |
| staff         | staff_id     |                1 |
| store         | store_id     |                1 |

---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

### Решение

```sql
REVOKE INSERT, UPDATE, DELETE ON *.* FROM 'sys_temp'@'localhost';
```

![](img/img-03-01.png)
