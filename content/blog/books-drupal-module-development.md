---
title: "Books/ Drupal 9 Module Development"
date: 2020-09-12
draft: false

# post thumb
image: "images/post/davidjguru_books_drupal_module_development_main.png"

# meta description
description: "Review of the book Drupal 9 Module Development by Daniel Sipos"

# taxonomies
categories: 
  - "Reading"
tags:
  - "Books"
  - "Drupal Development"
  - "Backend"
  - "PHP"
  - "Drupal Plugins"

# post type
type: "post"
---


For a long time I was thinking about write or not a review of the Drupal 8 Module Development (the former edition of the current). For me there were two very important keys: on the one hand, it was a very ambitious book in terms of scope and content (so it was an important challenge to make a synthesis on its simple review). On the other hand, Drupal 9 was already on its way and it was possible that it would be deprecated quickly. Luckily, Drupal 9 has already arrived and the transition from 8 to 9 has not only not been at all traumatic (there are still many people trapped in the crack between Drupal 7 and Drupal 8), but it has also been an easy road. There won't be substantial or profound differences in how to approach the creation of custom modules in Drupal 9 respect to Drupal 8 and with this new edition adapted to Drupal 9, everything is ready to review this ambitious book. There we go!  

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Erol Ahmed, @erol](https://unsplash.com/@erol)**

  
---------------------------------------------------------------------------------

**Table of Contents**  
<!-- TOC -->  
[1- Introduction](#1--introduction)  
[2- The Book](#2--the-book)  
[3- Recommendations](#3--recommendations)  
[4- Book Information](#4--book-information)  
[5- Fast Review](#5--fast-review)  
[6- Ratings](#6--ratings)  
[7- :wq!](#7--wq)  
<!-- /TOC -->

-------------------------------------------------------------------------------

## 1- Introduction

**Daniel Sipos** (or just Danny Sipos, or [Upchuk in Drupal.org](https://www.drupal.org/u/upchuk) or [@drupalexp in twitter](https://twitter.com/drupalexp)) is a developer related to Drupal for many years. He is also a regular speaker at Drupal events and is also the person behind the website called Web Omelette ([https://www.webomelette.com](https://www.webomelette.com/)), such a popular website that you should get to know it if you're a Drupal developer (put the URL in your favorites if you don't know).  
I already knew his website from looking for doubts, consulting references and obtaining examples (almost as a study and training online resource), when I had the opportunity to be a little closer to his work through a project in the context of the European Commission, where I had to use a [Drupal distribution and set of tools and resources, called **Open Europa**](https://github.com/openeuropa) to generate new platforms for the European Commission. At that time I left some written lines [here in my notebook](https://davidjguru.github.io/blog/openeuropa-the-european-commission-in-drupal-8) about this.  

As it is easy to contextualize, the author has a lot of experience in the field of knowledge about Drupal, and that shows well. Perhaps that is why he has written successive editions (Drupal 9 would be the third iteration) of what can perhaps be considered the fundamental book for Drupal-based development. In fact, because of its scope and size, this book should be considered the **Holy Bible of Drupal**. A resource that should be part of any developer's shelf or ebook/kindle/tablet. But if the metaphor about the bible were accurate, then we could extend it a little further. Because basically, a sacred text requires its own ["hermeneutics"](https://en.wikipedia.org/wiki/Hermeneutics) in order to be interpreted correctly, that is, its own study of meaning. We hope that this article will be useful to unravel its essence and value. Let's continue.  



## 2- The Book

Imagine a tutorial that contains all the most important issues of Drupal. A review through its diverse concepts, systems, subsystems, elements...Well here you have about six hundred pages with all of it. As we said in the previous section by way of introduction, the true Drupal bible. Here you will have access to all the ins and outs of Drupal, even those parts that are not well known even to many senior developers (such as the TypedData API).  
**The paradox, the real tension of this book** is to assume a length and depth that sometimes does not meet the expectations of didactics for people who want to start in Drupal. In my experience, I have tried to share the book with fellow juniors and it has been difficult for them to advance: first those who are not fluent in English, but then those who were. In all the cases observed, the colleagues considered the book too hard, rough, complex... however, when they commented and shared it with the older profiles, they made a much more agile and profitable use of the book. They did know how to get into the inner workings of the book. This is important and marks my view of the book in some ways.  

**Based on these observations**, I have concluded that if Drupal were a medicine, I would say that this extensive manual is something like a help for healthy people. It is in itself a treatment for people who are already initiated, those who are already able to place themselves within the world of Drupal. It is really curious, since it would be easy to accuse or blame the book of anti - didactic (in the second chapter, right when you create your first Drupal module the content are already talking about services or Form API, this is a very, very fast trip). You could say that everything (or almost everything) is sacrificed to try to make the ambitious goal of explaining Drupal deeply (or to demonstrate the author's mastery of the subject, but that belongs to another set of interpretations). This book contains the same information as a thousand articles, hundreds of posts, and dozens of online guides. It is the perfect accumulation of Drupal content, and in order not to get lost in the way, it is better to be in the hands of a senior profile that acts [as a sherpa](https://en.wikipedia.org/wiki/Sherpa_people).  

The book -by the way- is still edition after edition the reference guide on Drupal, as you can see in the books section of the Drupal.org website: [https://www.drupal.org/books](https://www.drupal.org/books).  

**Some APIs that are discussed in this book:**  
* **Mail API:** Send emails programmatically, by code (Chapter 3: Logging and Mailing).  
* **Token API:** How to manage formatted placeholders (Chapter 3: Logging and Mailing).  
* **State API:** Simple Storage by key/value pairs (Chapter 6: Data Modeling and Storage).  
* **UserData API:** Storage of some pieces of data related with users (Chapter 6: Data Modeling and Storage).  
* **Configuration API:** Related with this important subsystem of Drupal, set of methods (Chapter 6: Data Modeling and Storage).  
* **TypedData API:** Low level object oriented API for descriptive data processing over PHP (Chapter 6: Data Modeling and Storage).  
* **Entity API:** The most important API for interactions with Entities (Chapter 6: Data Modeling and Storage).  
* **Database API:** Describes methods and resources for manage the database, including queries (Chapter 8: The Database API).  
* **Schema API:** Allows defining database table structures (Chapter 8: The Database API).  
* **Cache API:** Creating, Reading and Invalidating caching entries (Chapter 11: Caching).  
* **AJAX API:** Client-Side interactions using PHP and without JavaScript (Chapter 12: JavaScript and the AJAX API).  
* **Translation API:** Working with entity translations (Chapter 13: Internationalization and Languages).  
* **Lock API:** Low level resource for processes (Chapter 14: Batches, Queues, and Cron).  



## 3- Recommendations
Mainly, this book is indicated for someone who wants to deepen their knowledge of Drupal.  

In principle, there are some requirements to enjoy this book, related with some kind of previous experiencie in the Drupal World, or it runs the risk of becoming a "book - door", since you will have to go to the Internet to find many concepts and other examples in order to establish the knowledge set out in this great manual.  
On the other hand, we will say: Ok, the book it's a MUST. All teams should have a copy nearby, for consultations, to resolve doubts, to clarify ideas. So if you've decided that you want to make Drupal the best way to make a living (and earn your salary). I would recommend you take this tutorial and handle it as what it is: an excellent cartographic compendium, with plenty of maps to go through all the routes of Drupal.  

Read it, study it, practice it and repeat all the examples. It's a master's degree in Drupal whose execution only depends on you (and with a very good price).  


## 4- Book Information

| Field         | Description   |
| ------------- |:-------------:|
| Title         | Drupal 9 Module Development. |
| Author      | Daniel Sipos.      |
| Publisher | Packt.      |
| Date | August, 2020.      |
| Pages | 626      |
| Overview | Comprehensive guide to Drupal 9 development.      |
| Keywords | Drupal, Backend, API, PHP, Symfony, Testing.      |
| Price | 30$ // 29.99€ (aprox)      |
| Links | [Packt](https://www.packtpub.com/product/drupal-9-module-development-third-edition/9781800204621), [Amazon](https://www.amazon.com/dp/1800204620/?tag=drupal0a-20)      |


## 5- Fast Review

| Question // Answer         | 
| ------------- |
| **1- Is this book progressive, Iterative and Incremental?**         |
| No, the book tries to be lineal using the idea of implementing a custom module, but it's more complex than this.         |
| **2- Does it offer specific solutions to particular problems or concrete issues?**         |
| Yes, the book is full-filled with a lot of proposals, solutions and ideas.         |
| **3- Does it explain well the original problems or needs it aims to solve?**       |
| Yes, Yes, although it does not have a problem-solution structure it's rather a topic-exposition.        |
| **4- Is this book rich in examples?**         |
| Yes, it contains many examples and practical demonstrations.          |
| **5- Is this book written in plain English, suitable for non-English speakers?**          |
| No, it contains set phrases, expressions, usages that sometimes take it away from plain English. .         |
| **6- Is it up to date?**         |
| Yes, is a recent issue from August 2019.          |


## 6- Ratings

![Table of Book Ratings for DDEV](../../images/post/davidjguru_drupal_9_module_development_rating_table_books.png)


## 7- :wq!


##### Recommended song

{{< youtube LP7z7rOphMU >}}
