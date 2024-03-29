---
title: Domain Storytelling
date: 2021-12-22T07:46:15+04:00
hero: images/posts/domain-storytelling/cover.png
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

## Scopes
Every domain story drawing session should be started with defining scope of the diagram. 
Scopes help us understand what kind of diagrams we are drawing and what kind of model we are discussing.
There are three dimensions in Domain Storytelling scopes:

### Dimensions

#### Granularity
There are two expressions that can explain level of details drawn on your diagram:
* COARSE-GRAINED
* FINE-GRAINED

**COARSE-GRAINED** diagrams as it said in the name represent only big, significant steps of the business process.
Each particular activity there can represent multiple small steps. Diagrams with that level of granularity can help understand
big picture of the business process. How the business process flow goes through multiple departments of the company, for example.

**FINE-GRAINED** diagrams represent significant level of details of business process. They can be used to represent
some particular business scenario within borders of one department, for example.

#### Purity
There are two expressions that means domain story purity:
* PURE
* DIGITALIZED

**PURE** domain stories represent and essence of the business process. That means such domain stories can contain only pure representation
of the business without any software tools. To have an understanding of what pure business process is try to imagine
that we live in a world without computers. How in that case your business process will look like? The outcome is your pure business process.

**DIGITALIZED** domain stories represent business process with involvement of software elements. Basically, such diagrams 
should show how the business process is transformed when software is used.

#### Time
Third dimension is about timing:
* AS-IS
* TO-BE

**AS-IS** domain stories represent current state of the business process. How the business process goes at the moment.
Such diagrams can be used in case you need to document existing business processes or as an input for **TO-BE** diagrams.

**TO-BE** domain stories represent future state of the business process. Basically, such diagrams answer onto following question: 
"How the business process will be changed in the future?". For example, how the business process will be changed in case
you will develop and add some software component to the flow.

### Result scope
Taking into account scope dimensions, result scope of the domain story diagram can be calculated using
following formula:
```text
result scope = <granularity> + <purity> + <time>
```

You can use all three dimensions in different combinations to get the scope you need. For example, following 
scopes I personally found as very useful:
* **COARSE-GRAINED, PURE, AS-IS** - diagrams with that scope are really useful in case you want to do initial domain discovery. AS-IS & PURE means that you will have vanilla representation of how your business process looks like at the moment of discovery, and COARSE-GRAINED granularity means that you will have a big-picture representation that will help you understand how the business process you model goes through different areas of the business.
* **COARSE-GRAINED, DIGITALIZED, AS-IS** - diagrams with that scope are useful if you are modeling business process that involves some software components. In that case DIGITALIZED domain stories can show you what place these software components take in the business process flow.
* **FINE-GRAINED, DIGITALIZED, TO-BE** - diagrams with that scope can be helpful in modeling of future state of the business process and to answer onto following question: "How the business process was changed by your software?"

## Example
Now, when you know everything about Domain Storytelling, let's try to put this knowledge to use. 

Imagine you need to model process of table reservation for one of local restaurants. Domain Expert who is restaurant owner told you 
what need to be done to reserve a table in his restaurant. He also told you that he would like to know how reservation process looks like
in the eyes of his potential customers, and he told you how he ask his potential customers for a feedback. And finally,
he told you that if there is no possibility to reserve a table on desire time he always suggests thinking about another time.

That story can be drawn like this:
![full-domain-story](/images/domain-storytelling/6.png)
This diagram has FINE-GRAINED, PURE, AS-IS scope. 

If you are up to do domain-driven design (DDD) this diagram can give you a clue which bounded contexts your business process comprised of.
Using same domain story you can group elements of the story on bounded contexts:
![full-domain-story](/images/domain-storytelling/7.png)
And with that we found out that there can be three separate bounded contexts in your domain:
* visit context
* reservation context
* rating context

That information is invaluable if you try to build microservices-based system.

And finally, let's try to model how business process will look in the future when we will implement some software system that will help restaurant owner to automate his business.
The diagram we need will have FINE-GRAINED, DIGITALIZED, TO-BE scope:
![full-domain-story-final](/images/domain-storytelling/8.png)

Actors marked by red colour are new software we are going to introduce for the business. Changes were made for all three bounded contexts.

_Visitors context_ is now fully covered by restaurant's mobile application. We no longer need to have dedicated person on a position of Administrator. All operations in visitors context will be automated.

_Reservation context_ also changed. In new version of the process we are going to automate work with the reservation catalogue that previously was done with paper work. Automation will be possible with introduction of another software component - Reservation service. The service we can develop for the business.

And finally, _rating context_ can be enriched by outsourcing that functionality to one of existing service with which we can integrate our own solution. For the sake of example, I have
randomly chosen [Podium](https://www.podium.com/surveys/) as a service to collect visitors' feedback. 

## Recap
As you could from the article, Domain Storytelling is a very simple technique that can help you make 
strategical decisions about your software solutions and it's architecture. It can be also used as a form 
of documentation to spread the knowledge about business domains.

The tooling is also in place. In this article I have used official modelling tool, but you are not limited to use only it.
You are free to use any kind of visual modeling tool you want. And there is also support of Domain Storytelling dsl in plantuml - a tool which
you can use Diagrams-as-Code approach.

## Useful materials
* [1] [Domain Storytelling website](https://domainstorytelling.org/)
* [2] [Domain Storytelling modelling tool](https://www.wps.de/modeler/)