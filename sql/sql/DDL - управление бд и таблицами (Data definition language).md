## **Создание и удаление базы данных:**

`CREATE DATABASE testdb;`

`DROP DATABASE testdb;`

## **Создание и удаление таблицы:**

##### CREATE TABLE (создание таблицы):
`CREATE TABLE publisher`
`(`
	`publisher_id integer PRIMARY KEY,`
	`org_name varchar(128) NOT NULL,`
	`address text NOT NULL`
`);`
##### DELETE TABLE (удаление таблицы):
`DROP TABLE publisher;`

##### CLEAR TABLE (удаление данных из таблицы):
`TRUNCATE TABLE faculty;`
###### перезапустить идентификатор
`TRUNCATE TABLE faculty RESTART IDENTITY;`


## **Изменение таблицы:**

## TABLE
##### RENAME TABLE (переименование таблицы):
`ALTER TABLE cathedra`
`RENAME TO chair;`


## COLUMN
##### ADD COLUMN (добавление колонки в таблицу):
`ALTER TABLE student`
`ADD COLUMN middle_name varchar;`

##### DELETE COLUMN (удаление колонки из таблицы):
`ALTER TABLE student`
`DROP COLUMN middle_name;`

##### RENAME COLUMN (переименование колонки в таблице):
`ALTER TABLE chair`
`RENAME cathedra_id TO chair_id;`

##### CHANGE DATA TYPE (изменение типа данных в колонке):
`ALTER TABLE student`
`ALTER COLUMN first_name SET DATA TYPE varchar(64);`


## CONSTRAINT
##### ADD CONSTRAINT ( добавление ограничений )
`ALTER TABLE publisher`
`ADD CONSTRAINT FK_books_publisher FOREIGN KEY(publisher_id) REFERENCES publisher(publisher_id);`

##### DELETE CONSTRAINT ( удаление ограничений )
`ALTER TABLE publisher`
`DROP CONSTRAINT FK_books_publisher;`
