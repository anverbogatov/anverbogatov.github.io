---
title: Domain Storytelling
date: 2021-12-22T07:46:15+04:00
hero: images/posts/domain-storytelling/cover.png
draft: true
menu:
    sidebar:
        name: Domain Storytelling
        identifier: domain-storytelling
        parent: modeling
        weight: 20
tags: ["modeling", "domain-storytelling"]
keywords: ["modeling", "domain", "storytelling", "domainstorytelling", "software", "softwarearchitecture", "architecture", "ddd"]
---

## What is Domain Storytelling?
Is a modelling technique that helps you visualize business processes of any business domains.
It comprised of picto-graphic language and set of rules with which you can visualize core business processes and 
tell stories about your business domain.

## When it can be useful?
Domain Storytelling can be used in following cases:
* **when you need discover your business domain**. In that case, you need to have domain experts at your disposal. Modelling session should be contacted along with live discussion with domain expert who can tell you stories about business domain. Usually, that kind of work is performed in a format of workshop where domain expert tells stories about domain and modeller draws Domain Storytelling diagrams aligned with what domain expert said.
* **when you need to share the knowledge about your business domain**. Outcome of Domain Storytelling modelling sessions is always one or many diagrams that represent business processes. These diagrams are very easy to read and understand. That makes Domain Storytelling a perfect tool to do a knowledge transfer about domain. For example, you can document your domain with it with domain expert and then use these diagrams as an input for product development. 
* **when you need to design software architecture**. Domain Storytelling is more about business then software, but it can be also useful close to the software implementation. With Domain Storytelling diagrams you can easily spot separate bounded context in business process flow. That information can be used when you will do context mapping session (of course if you work with DDD)
* and finally - **when you want to look onto the future**. Domain Storytelling can be useful not only when you work with current state of the business, but it also can be useful to model how your software can change existing business processes in the future

## How it is connected to Domain-Driven Design (DDD)?
Concept of DDD describes two spaces:
* problem space
* solution space

Solution space is where software architecture, context maps and so on live. Solution space is close to the end solution, to the software.

Problem space is where business lives. So, Domain Storytelling lives there either. Before you will think on "How to solve business process?" question, you need to
understand what kind of business problem in what domain you want to solve. And for doing that Domain Storytelling can be a first step in domain and problems discovery.

## How to start?
If you want to follow along, you can use special tool for Domain Storytelling drawings [available here](https://www.wps.de/modeler/).

### Base elements
There are only 3 base elements available in Domain Storytelling:
* actor
* work object
* activity

Basically, that is all you need to represent any business process you want - from smaller one to biggest.

#### Actor
Actor is a protagonist in the story you want to represent. Every Domain Storytelling diagram should represent communication between different actors. That is the core part of any business process you can think about.
In different cases there can be only one protagonist or a group of them.
Actors can be drawn like here and usually they are named with nouns:
![actor-image](/images/domain-storytelling/1.png)

#### Work Object
Work Object is a piece of information, a data or an item that can be passed from one actor to another. Work Objects
usually used to represent transmittable object without which business process could not be done. So, passing work objects around is from what your business process comprised of.
Work Objects can be drawn like here and usually they are named with nouns:
![work-object-image](/images/domain-storytelling/2.png)

#### Activity
Activity is an action that is originated by actors. Activities are executed in certain order and they also represent a direction in which
your business process goes. 
Activities can be drawn like arrows and usually they are named with verbs, but you also can name them the way that will help you to read your domain story aloud:
![activity-image](/images/domain-storytelling/3.png)

#### Annotation
There is one auxiliary element that you can use to add comments or context specific information to elements of your diagram. It looks like this:
![annotation-image](/images/domain-storytelling/4.png)

### Tell the story
Now, with having theory about Domain Storytelling on our minds we can start drawing real stories! 

As I said, Domain Storytelling sessions usually conducted in a form of workshop where one person who takes a role of a modeller
draws domain stories in a diagrams and communication with another persons who take role of domain experts about the business. In other words, domain experts tells modeller about business processes of his domain and modeller draws domain stories diagram in response. Let's model that story with Domain Storytelling.
Here is how it can look like:
![domain-storytelling-workshop](/images/domain-storytelling/5.png)

Pay attention to small numbers in cyan circles located above activities. They point to an order of activities in which represented business process is executed. 
What is even more, they help you read your domain story as a story written in English. Just try to read aloud whole story by composing sentences using order and names of actors, work objects and activities.
In the example above you can have a story like this:
1. Domain Expert tells a story to Modeler
2. Modeler draws domain story diagram
3. Modeler validates domain story diagram by showing it to Domain Expert

And now, imagine that you need to explain how to do a Domain Storytelling workshop to somebody who had never before heard about Domain Storytelling.
With diagram like this you can even share it and do not explain things by yourself.

That is a good example of how Domain Storytelling can save and transmit knowledge about domain.

## Rules
There are some rules you should know about to do Domain Storytelling right.

### 1. One story = one diagram
One domain story diagram must represent only one business process. Just one end to end flow. 
That means that all actors, work objects and their activities are moved / executed toward one defined goal.
Domain Storytelling is not about representing everything that happens in the company or in business domain. It is more about details
vital for understanding of the business.

So, keep in mind when you start draw a story you need to think about goal that must be achieved in your particular scenario.
### 2. Actors must be unique, Work Objects can be duplicated
Domain stories must contain only one instance each of actors drawn on a diagram. If you think that there are too many activities 
drawn on a diagram, and it became a mess - then you should consider drawing another diagram to represent part of the business process
drawn on current one or consider changing level of details you used while drawing. Because maybe you just wanted to represent 
every small step of the business process on the diagram.

And there is another situation with Work Objects. You can duplicate Work Objects on your domain story diagram for any part of the business process drawn there.
### 3. No conditions
One domain story diagram must represent only one business flow. If there are situations where some part of the business process
can happen only in case of certain conditions met - then you should draw such variations as separate domain stories.
### 4. 80% cases, no corner cases
Taking into account that most of the business process will have many conditions through which the process can go, you should know that 
amount of diagrams that represent every particular business process flow can be very high. That does not mean
that you should draw every variant of your business process. 

Domain Storytelling is not about covering 100% of variants of business process. You should not think about corner cases and 
rather concentrate your efforts on drawing 80%-cases (happy cases and variants significant for the business).

## Useful materials
* [1] [Domain Storytelling website](https://domainstorytelling.org/)
* [2] [Domain Storytelling modelling tool](https://www.wps.de/modeler/)