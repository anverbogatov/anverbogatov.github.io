---
title: GraphQL Introduction
date: 2021-12-02T07:14:03+04:00
hero: images/posts/graphql-introduction/cover.png
menu:
    sidebar:
        name: GraphQL
        identifier: graphql1
        parent: technologies
tags: ["graphql"]
keywords: ["graphql", "api", "graphql api"]
---

This article contains introduction to GraphQL, and it's concepts. If you want to start learning GraphQL, but do not know
from what place to start - this article will be a perfect introduction for you.

## What is GraphQL?
"GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data" - says project's website.
But what these words do really mean? 

They mean that GraphQL defines own DSL with which clients can access data. They also mean that GraphQL defines internal tools that you can extend in attempt to organise data access operations in an optimal way that is specific for particular case.

You should also know that GraphQL is language agnostic. You can find many implementations for many programming languages that can be used in different projects. No matter what technology you use, you need to work with same concepts to be able to work with GraphQL.

## Concepts

There are 4 main concepts that you should be familiar with to be able to work with GraphQL.

#### Schema

If you have ever seen GraphQL queries written in it's DSL, you know that GraphQL querying is mainly about selecting fields on objects that we want to fetch. The shape of the result will look similar to the query, but how to know what fields or objects we can use in the queries? That is where Schema goes in.

**Schema** is a **core concept** of GraphQL. Literally:

```plaintext
If you do not have schema - you do not have GraphQL.
```

Every GraphQL application has one object of **Query** type and optionally can have object of **Mutation** type. These two types are same as usual objects, but they are special because they define _an entry point_ of every GraphQL query.
```graphql
schema {
  query: Query
  mutation: Mutation
}
```

#### Query
**Query** defines an interface for your data access logic. It describes what type of objects you can fetch from underlying system, what parameters must be send as an input and what type of output you can have as the result. 

Example of Query object can be following:
```graphql
query {
    documents(page: 0, size: 5) {
        id
        name
    }
}
```
In the example above `documents` is name of the method in internal implementation of the application that will be called. In GraphQL terminology `documents` is a **field**.

`page` & `size` are input arguments that will be used while calling `documents` method. You can also spot exact values that will be transmitted to internal method for both arguments.

And finally, `id` & `name` are names of fields that must be retrevied in response for objects that will be fetched. 

GraphQL's Query can describe multiple fields, each of them can have own signature. For example, let's add additional field that will represent querying document by id.

```graphql
query {
    documents(page: 0, size: 5) {
        id
        name
    }

    document(id: ID) {
        id
        name
        createDate
    }
}
```
Pay attention that input argument for `document` field has `ID` type. That is one of scalar types defined by GraphQL standard. We will talk about that in details a bit later. 

`document` field's output is a bit different. There is another field added to the list - `createDate`. So, the object that will be returned as the result of that query will be described with these 3 fields.

And finally, we can omit `query` word. As it was said above, **Query** is a special type, is an entry point to GraphQL. So, if you will omit `query` term in your request definition, GraphQL will know that you meant `query`.
```graphql
{
    documents(page: 0, size: 5) {
        id
        name
    }

    document(id: ID) {
        id
        name
        createDate
    }
}
```

In example above, we have defined our GraphQL request. There is one strange thing that you probably interested in - if that is request, how it will work? We have defined 2 separate fields - one that should find five first documents and second that should find one document by id. And yes - with GraphQL that is completely valid case. You can describe multiple methods to be used within one query. And GraphQL will handle that internally. From client perspective it will be one http call and the client will receive one response from the server. That response will contain all the data requested in the query.

#### Mutation

**Mutation** is second special type in GraphQL. Mutations are responsible for descrbing an interface of operations for data modifications. For example, to continue example with documents, **Mutation** that allows adding new document to the application will look like this:
```graphql
mutation addNewDocument($name: String, $docType: String) {
    addDocument(name: $name, docType: $docType) {
        id
        name
        createDate
        docType
    }
}
```

#### Subscription

**Subscription** is third special type in GraphQL. Quries allow reading data. Mutations allow writing data. But oftentimes clients want to get pushed updates from the server when data they care about changes. And that is what Subscriptions responsible for.

## Types

As it was mentioned before **Schema - is a core concept** of GraphQL. You define own schema by expressing your data and data access / modification cases through fields and types. 

There are three kinds of data types in GraphQL available for use:
* Specific types
* Scalar types
* Object types

![schema-and-types](/images/graphql-introduction/1.png)

Every type is nullable by default (except specific ones). And also it is possible to define list type where all the elements will be of particular scalar type or object type.

Specific types are **Query**, **Mutation** and **Subscription**. They described above.

#### Scalar types
In GraphQL every field in schema must have defined type. Scalar types are basic types that are defined in GraphQL specification. Here they are:
* String
* Int
* Float
* Boolean
* ID

#### Object types
Object types are types with own complex structure and behavior. They can be defined same way as Query type in Schema:
```graphql
type RandomNum {
    random(max: Int): Int
}
```

#### Custom types
You probably already noticed that list of scalar types do not include Date type. To be honest, I do not know the story behind that decision, but
I think that implementation of some types (like Date, for example) depends on a programming language used under the hood. And creators of 
GraphQL suggest extending list of types by adding support for whatever type you need by yourself. 

Luckily, with many GraphQL implementations for different platforms (JVM platform for example) there are a variety of libraries
that close the gap in that area.

## The process
In the last section of the article, I would like to describe how the process of building and working GraphQL support in software can look like. That was not so transparent 
when I touched GraphQL topic for the first time in my life. So, I make an assumption that you also interested in how technically work with GraphQL looks like.

Briefly the process will look like this:
![graphql-integration-process](/images/graphql-introduction/2.png)

There are not so many steps to be done before you will have GraphQL in your application. Let's briefly describe what need to be done on each of the steps of the process on a simple example -
let's pretend that we are building GraphQL API for querying books for local Library.
1. First, we need to _define schema_. Schema models our business domain through graph.
As a core entity of our domain we will have Book. Every book has own Author. Let's model that through GraphQL schema:
```graphql
type Book {
    year: Int!
    title: String!
    author: Author!
}

type Author {
    name: String!
    books: [Book]
}
```
If you remember, each GraphQL schema must include Query object. Let's write two fields to express two data access scenarios - when we get books by title and next read all
available books:
```graphql
type Query {
    books:[Book]!
    books(title: Sring):[Book]!
}
```
Our schema is ready. It needs to be saved as a file with `graphqls` extension.

2. Next, under the hood, software to which we are integrating GraphQL must _implement data access_.
Our application must provide two things:
* implementation of types specified in your schema (`Book` & `Author`), so that GraphQL can map types between own representation and application's
* implement so called `resolvers`. Resolvers are special services written on programming language of your application. They are responsible for
handling data access / modification scenarios. You can think about them as a concrete implementation of the interfaces defined in your GraphQL schema.

3. And finally, after schema and resolvers are ready, you can start using your integration on client side.

By default, GraphQL uses http, its only end point `/graphql` is available in your application and listens for incoming requests. So, every client that would like
to use GraphQL to query data in our application need to send POST requests to that end point.

To get all available books without their author the client can send following request:
```graphql
{
    books {
        title
        year
    }
}
```
and as the result, GraphQL API will return list of available books without their authors. That can be useful if, for example, we need to present only list of available books without any details.
But for case where some specific book must be presented along with all details, the query can look like that one:
```graphql
{
    books(title: "Java programming") {
        title
        year
        author {
            name
        }
    }
}
```
And that query will return information on book along with its author.

## Useful links
[1] - [GraphQL web site](https://graphql.org/)
[2] - [GraphQL Playground web site](https://github.com/graphql/graphql-playground) - Playground is a perfect tool that can be used for testing your GraphQL integration + act as documentation for your clients