+++

title = "How to design a REST API?"

date = "2021-11-20T18:55:34+04:00"

author = "Anver Bogatov"

authorTwitter = "" #do not include @

cover = "https://miro.medium.com/max/10944/0*7Uc8p5VVguHJA6GU"

tags = ["design", "restapi"]

keywords = ["api", "design", "restapi"]

description = "The article explains in a very easy and accessible manner simple and what is more important working way to design new REST API for applications."

showFullContent = false

+++

# What is API?

**API or Application Programming Interface** — is a description of communication between two programs.

To be even more precise, an API is a set of classes, interfaces, or data structures that are defined by the program that owns the API.

With the API, you can make a program do something. What exactly - defined by the API.

**There are many APIs exist**.

For example, Java itself gives access to dozens of APIs to developers: Core API, Concurrency API, Collection API and many more.

Other frameworks define their own APIs: Hibernate API, JPA (Java Persistence API), RestEasy, and so on.

**Each of the APIs has its own goal**.

APIs are not created just for fun. Each of the APIs is like a TV remote control. It gives an opportunity to control the application.

For example, an application for disk space usage analysis can contain an API that allows getting statistics on files and the space they take; this will point to the heaviest file on the disk drive and so on.

At the same time, the API does not allow doing any random stuff. All the capabilities of the API are consistent with its goal.

**APIs do not belong to concrete technology**.

If an application is written in Java, that does not mean that the API will be limited to that particular programming language. Applications can communicate through APIs written using different technologies and programming languages.

For example, REST API.

# What is REST API?

REST or Representational State Transfer is an architectural approach based on the HTTP protocol.

# Why do you need to design a REST API?

**Application development is like collecting a puzzle without hard rules**.

Everybody can win that game. But the true mastery of software engineering is writing software applications that will work quickly, without errors, and will be extensible.

That is why, as in house building, you need to know everything beforehand, because the cost of a mistake on

design stage cheaper than the cost of a mistake at the moment when the house is built.

# Domain driven design of REST API

**Technique that I am going to share today** is well described in Manning's book - "The Design of Web APIs" [1].

The link to the book can be found at the bottom of the page.

**Small input**, before I will explain the method. All the ideas below are highly oriented onto practice.

The method is not just a dry theory, but rather a live method of REST API design. Thus, we will talk about it using live

example. Imagine that we need to design a REST API for some blog service. To be precise, we will work with

posts and their comments. Using that requirement as an example, we will use the method.

**What goals such API can have?** There can be even multiple goals:

* Read posts. There should be a way to read existing posts.

* Modify posts. What if there is a mistake in a particular post? It's author must have a way to fix the mistake after post was published.

* Create post. Posts must be added to our service somehow.

* Delete post. When author decided to remove own post.

* Read post's comments. For brevity's sake, let's agree that comments are added to our service some other way.

These goals we will translate to our REST API.

**Now, let's define main rules**:

* Goals are translated to **resources** and **actions**

* **Resources** are expressed as paths in REST API, and **actions** are expressed as HTTP protocol's methods.

**What is a resource?** Resource is an object, a view, or an entity - literally it can be anything that means some data

in the service. There are two resources in our blog service - Post and Comment.

**What is action?** Action is a way to ask our blog service to do something, for example, to get existing posts or

to create a new one. Actions are expressed as methods of HTTP protocol, as was said above. The HTTP protocol defines

multiple such methods, but we are interested in standard ones: GET, POST, PUT, DELETE.

**Now, let's go to the method**. Let's break it down into steps:

* First of all, we need to define all the resources and sub-resources with which our API must work. In our case, these are **Post** and **Comment**.

* Next, we need to define a list of actions that our API must allow us to do. For us, the list is the following:

1) GET Post — for access to existing posts

2) POST Post — for new post creation

3) PUT Post — for existing post modification

4) DELETE Post — for existing post deletion

5) GET Comment — for access to the existing post's comments

* Let's define a list of parameters for the actions from above:

1) GET posts action can be called without parameters, and that will lead to access to all of the existing posts. It can also be called with the identifier of a concrete post and will return only the post with the identifier specified as a parameter.

2) POST action implies that there should be new post data used for new post creation.

3) PUT action requires two parameters —the identifier of the post that we will modify and the post's data that will replace existing data.

4) DELETE action requires to have post's id to know which of the posts must be deleted

5) GET post's comments action requires identifier of the post for which comments must be returned

**Let's stop for a second**.

We did a huge preparation to get as much information as we can about our future API. And now we know how our API should work.

But that information is not enough, and we need to collect additional information. That we will do right now.

What else do we need to know? Basically, we need to answer onto following questions:

* Who will use our API?

* What and how exactly that 'who' will do with our API?

There is a special structure we can use specifically to collect all information we need. It can be represented as a table

with predefined list of columns:

* **Who** — means users, persons, roles, clients and so on; it means who will use our API

* **What** — means expectations of the user about our API ("what to expect?")

* **How** — means how we will reach his goal

* **Input** — data that must be given to our API so that it can help our user reach his goal

* **Output** — data that will be returned to the user as the result

* **Goal** — describes goal of particular operation

* **HTTP method** — points onto HTTP method that describes an action that our user does

* **URI** — path to particular API operation

![design-rest-api-table-1](/images/design-rest-api/1.en.png)

**All is left to do is fill in the table** with data based on the information we gathered. Let's try to do that for GET posts operation:

![design-rest-api-table-2](/images/design-rest-api/2.en.png)

**Let's look closer on it**. We work with posts. That means that somebody needs to write them. So, every post has its own Author.

The author can get access to all of his posts. For that, we do not need to provide any specific input and under the hood implementation of the API

can detect current user and detect his posts. As the output we will have a list of posts written by current user (Author).

Read operation in HTTP protocol expressed as GET method. And finally according to commonly used approach URI must end with

name of the resource. In our case it will be `.../posts`.

**During filling in the table, always aks yourself following questions** - where are resources from? who works with them? and so on.

The bigger picture you will try to see, the more cases you can identify on the design stage. Simple example:

We have designed operation of getting the posts, but who had added them before? Author, of course. Let's design that as well:

![design-rest-api-table-3](/images/design-rest-api/3.en.png)

Using same approach **let's design rest of operations**:

![design-rest-api-table-4](/images/design-rest-api/4.en.png)

**Pay attention onto last row**. We have discussed sub-resources before. On the screenshot from above you can notice how URI

for sub-resources access can look like. In our example, how URI for accessing post's comments can look like. At the beginning of the path

name of the parent resource must be given and only after that name of sub-resource can be used. According to that

the schema for URI can look like this: `/api/[resource]/{id}/[sub-resource]`. This is a commonly used practice and URIs like this allow

your users to understand which of resources depends on others.

**As an outro, we need to ask ourselves one question - Will the API we have designed fit all the cases in any situations? Of course - not**.

If you look onto the table closer, it becomes clear for you that the API we have designed is intended to be used only by Authors.

Author can read only own posts, but users of blog service must have access to all posts of all authors. Such requirements (you can also call them as constraints)

become visible during design stage, only if you try to get the big picture by asking yourself questions about your resources, actions, users, cases and so on.

**I hope that this article will help you** to understand that REST APIs can be designed using simple and formalised algorithm.

# Useful links

* [1] "The Design of Web APIs" book [on Manning](https://www.manning.com/books/the-design-of-web-apis)