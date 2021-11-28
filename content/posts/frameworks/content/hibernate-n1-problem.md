---
title: 🇷🇺 Hibernate N+1 Проблема
date: "2021-11-10T19:51:58+04:00"
menu:
    sidebar:
        name: Hibernate N+1 Problem
        identifier: hibernate1
        parent: frameworks
        weight: 20
tags: ["hibernate"]
keywords: ["hibernate", "n+1", "n+1 problem"]
---

### Пример N+1 проблемы
Проще всего проблему проиллюстрировать используя классический пример с Книгой (book) и Автором (author). Предположим, что у нас в модели есть сущность Book. У каждой сущности Book есть свой Author.

В Java, с использованием Hibernate это отношение можно смоделировать следующим образом:
```java
@Data
@Entity
@Table(name = "book")
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private int id;

    private String title;

    @OneToOne(fetch = FetchType.LAZY)
    private Author author;
}

----------------------------------------------------------

@Data
@Entity
@Table(name = "author")
public class Author {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private int id;

    private String name;

    @OneToOne(mappedBy = "author")
    private Book book;
}

```
Поле `book` в классе `Author` указывает на то, что владельцем отношения является сущность `Book`:
```java
    @OneToOne(mappedBy = "author")
    private Book book;
```

Предположим, что нам необходимо загрузить из базы данных несколько книг.  Для доступа к базе данных, мы используем простейший Spring Repository:
```java
public interface BookRepository extends CrudRepository<Book, Integer> {
}
```
C репозиторием, подобному тому, что Я привожу выше, уже можно зачитать из базы данных все сохранённые Книги (book). Сделать это можно, просто вызвав следующий метод:
```java
repository.findAll();
```
При наличии следующей конфигурации:
```yaml
spring:
  jpa:
    show-sql: true
```
В логах приложения мы увидим:
```logs
Hibernate: select book0_.id as id1_1_, book0_.author_id as author_i3_1_, book0_.title as title2_1_ from book book0_

Hibernate: select author0_.id as id1_0_0_, author0_.name as name2_0_0_, book1_.id as id1_1_1_, book1_.author_id as author_i3_1_1_, book1_.title as title2_1_1_ 
from author author0_ left outer join book book1_ on author0_.id=book1_.author_id where author0_.id=?

Hibernate: select author0_.id as id1_0_0_, author0_.name as name2_0_0_, book1_.id as id1_1_1_, book1_.author_id as author_i3_1_1_, book1_.title as title2_1_1_ 
from author author0_ left outer join book book1_ on author0_.id=book1_.author_id where author0_.id=?

Hibernate: select author0_.id as id1_0_0_, author0_.name as name2_0_0_, book1_.id as id1_1_1_, book1_.author_id as author_i3_1_1_, book1_.title as title2_1_1_ 
from author author0_ left outer join book book1_ on author0_.id=book1_.author_id where author0_.id=?
```

Разберём подробнее, что же в мы тут видим.

Первый запрос:
```sql
select book0_.id as id1_1_, book0_.author_id as author_i3_1_, book0_.title as title2_1_ from book book0_
```
Зачитывает все имеющиеся записи в таблице `book`. Для своего эксперимента Я сохранил 3 сущности типа Book. У каждой из этих сущностей, был установлен свой, уникальный автор - Author. Итого, в базе данных сохранено 3 записи в таблице `book` и 3 записи в таблице `author`.

После того, как Hibernate зачитал все имеющиеся книги одним запросом, он начал зачитывать записи из таблицы с авторами по одному, с использованием outer join конструкции.

Это и есть проблема - N+1.

### Что такое N+1?
Проблема N+1 - это проблема неоптимального доступа к сохранённым данным, при которой каждой из записей зачитанных одним sql запросом связные данные будут зачитаны по одному.

N+1 можно так же расшифровать как - один запрос, чтобы зачитать множество сущностей одного типа, но N запросов, чтобы зачитать все связные сущности.

### Почему это происходит?
Потому что, в момент когда мы используем репозиторий для загрузки всех сущностей типа `Book`, мы не сообщаем о том, что нам так же нужны связные сущности типа `Author`.

Запрос за каждой связной сущностью `Author` происходит в момент доступа к ней, через поле `author` объектов типа `Book`. 

### Как обойти проблему?
Обойти проблему можно используя нативную фичу Hibernate под названием «Batch Fetching» (пакетная выборка).

Достаточно добавить следующую аннотацию на объявление класса сущности `Author`:
```java
@BatchSize(size = 100)
```
И запросы в логах будут выглядеть следующим образом:
```logs
Hibernate: select book0_.id as id1_1_, book0_.author_id as author_i3_1_, book0_.title as title2_1_ from book book0_

Hibernate: select author0_.id as id1_0_0_, author0_.name as name2_0_0_, book1_.id as id1_1_1_, book1_.author_id as author_i3_1_1_, book1_.title as title2_1_1_ from author author0_ left outer join book book1_ on author0_.id=book1_.author_id where author0_.id in (?, ?, ?)
```
Первый запрос зачитает все записи из таблицы `book`, а второй запрос зачитает все связные записи из таблицы `author`. 

### Полезные материалы
* [1] [GitHub - anverbogatov/hibernate-n1-problem: This repository contains simple Spring Boot application that is intended to illustrate N+1 problem in Hibernate.](https://github.com/anverbogatov/hibernate-n1-problem) - полный код приложения, которое Я написал, чтобы продемонстрировать проблему
* [2] [HIBERNATE - Relational Persistence for Idiomatic Java](https://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html_single/#performance-fetching-batch) - документация о пакетной выборке данных в Hibernate