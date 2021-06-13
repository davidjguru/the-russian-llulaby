---
title: "Everything you can populate in a Drupal node by code"
date: 2021-06-20
draft: true

# post thumb
image: "images/post/davidjguru_drupal_8_9_everything_you_can_populate_in_a_drupal_node_by_code_main.png"

# meta description
description: "How to populating a node in Drupal programmatically, fullfilling all its fields by coding."

# taxonomies
categories: 
  - "Development"
tags:
  - "Backend"
  - "Drupal Development"
  - "PHP"
  - "Entity API"
  - "Drupal Examples"

# post type
type: "post"
---


Perhaps of all the Drupal available APIs, the Migrate API is one of the areas of knowledge most hidden from the average Drupal user. It's even possible that as you read this post, you know for the first time that there is a whole set of Drupal modules both in core and contributed, and fully dedicated to the design and execution of migration processes within Drupal. You can move data from an old Drupal to the most recent version but also from others databases, files, systems, frameworks, CMS, DXPs... and all these options are aggregated around the Drupal Migrate API, a set of resources that requires much time of dedication, among other things, due to the disintegration of the available information, until now. The book that I will discuss today comes to bring order and unification in all this. Pay attention.  

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Bruna Araujo, @brucaraujo](https://unsplash.com/@brucaraujo)**

  
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

Migration processes are a fairly frequent issue in project development: it is quite normal to build a new Drupal-based website for a client that already has its platform implemented in a different technology (not kidding, this hapens). As part of these processes, a client or customer may need to move their data to the new platform using a new data model as Drupal does with its tables and relations.  
 
In these situations, several scenarios open up for these processes: the study of the origin of the data, the processing possibilities, the methodology to sanitize the information and how to store it in a stable way in our new environment. What I have just briefly described is what is involved in [an ETL process](https://en.wikipedia.org/wiki/Extract,_transform,_load): Extraction - Transformation and Load for the data migration, and this is the spirit of the book I am discussing today: the exhaustive study of the design and execution for ETL process in Drupal.  

For those of us who at some point have had to learn to migrate data to Drupal by the hard way, I'm sure it has been recurrent a) to search for information on Google at some point. and b) Reach Mauricio Dinarte's blog: [understanddrupal.com](https://understanddrupal.com/). For me, this website has been a very important reference (almost mandatory) to understand how to perform Drupal Migrations. In fact, it was one of the recommended authors that I considered fundamental to learn how to execute Drupal Migrations, when more than a year ago I started writing about this deep topic here [in The Russian Lullaby](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/#6--authors-you-should-know), linking to his website and the platform of [Agaric.coop](https://agaric.coop/tags/migrate), with shared related content, too. So the author and his work is not alien to me, but what happens is that today I want to talk about a novelty discovered recently: the compilation of all the great work around this topic in his book about migration processes in Drupal: [31 days of Drupal Migrations](https://gumroad.com/l/31-days-of-drupal-migrations).  



## 2- The Book

It's difficult for me to evaluate this book as it deserves. So I guess maybe the first thing I want to say is that I think this is the first time I'm faced with such an integrated and compact body of knowledge/experience. This ir very important, because in many cases Drupal documentation is not very extensive or not sufficiently updated. 
When it comes related to a complex or advanced topic, this becomes a real and deep problem:  It's necessary to consult outdated blogs, review contributed modules, review code and read deprecated documentation... in order to be able to go step by step building some ideas about how you can do this or that...  

![Cover of 31 Days of Drupal Migrations, by Mauricio Dinarte](../../images/post/davidjguru_drupal_8_9_books_31_days_of_drupal_migrations_one.png)

Maybe because of this is why initiatives like this from [Mauricio Dinarte, @dinarcon](https://twitter.com/dinarcon) from the initiative [Understand Drupal](https://twitter.com/udrupalcom) are so important and generate a very high value (only surmountable if the same information becomes part of the official Drupal.org documentation, of course).  

As I said at the beginning of this section, it's difficult for me to describe how much this resource has helped me, but I would like write down some special points that you can learn from this book, some key points of interest, something like:  

**Key Points:**  
________________________


* Perhaps the first fundamental learning here: to know how a migration process works, what are the workflows and its possibilities.  

* Learn about how to migrate data in a lot of entities and resources: files, images, paragraphs, fields and subfields...All full of a) theory and concepts and b) examples, examples, and more examples.  

* Know the way to perform migrations from diverse sources and origins: XML files, CSV, JSON... executing processing ad-hoc for the values.  

* Get the most complete list of the most valuable contributed modules related with Migrations: Media Handler, Media Migration, Geofield, Migrate Devel...core modules, contrib modules, modules that includes plugins from Migrations...the set is long and pretty interesting. This book contains the most comprehensive and centralized list of all.  



## 3- Recommendations

Actually, I guess I could say - in a nutshell - that any work team implementing Drupal-based projects should have a copy of this book available. And I wouldn't be exaggerating, I swear. I think this huge, extensive and thorough work compiled by Mauricio Dinarte has too much importance and it's quite interesting too. This can't to go unnoticed.  

On the other hand, being "Migrations" a topic that actually encompasses and relates to other Drupal APIs, it can also provide quite a lot of knowledge to people in a technical team who want to learn more about Drupal internals: Plugins, Services, Entities, Drush, Debugging...these are cross-cuttings topics in this book and they are also very important issues. Maybe you wanna learn more about  that.  

For everyone who has to go through ETL processes in Drupal, this is a must-have resource: you can't get through the maze without using a good map of the territory and this chart is the best compilation yet of all the key issues when defining a migration process: Do you know under which criteria to decide how to define your migration? code or configuration? advantages and disadvantages?... These are just the initial questions. Along the way there are many more, and this is the best guide to deal with it. The fact that this e-book is also available for only $10 makes it even more obvious: there are no excuses. It's a must. And then, if you want you can be supporter of the Understand Drupal initiative, sponsoring the tutorials production: [understanddrupal.com/supporters](https://understanddrupal.com/supporters). Think about it.  



## 4- Book Information

| Field         | Description   |
| ------------- |:-------------:|
| Title         | 31 Days of Drupal Migrations. |
| Author      | Mauricio Dinarte.      |
| Publisher | Leanpub.      |
| Date | October, 2020.      |
| Pages |   193   |
| Overview | Complete guide to implement migrations and ETL processes in Drupal.      |
| Keywords | Drupal, Migration, Migrate API, Plugins, Source, Process, Destination.      |
| Price | 10$      |
| Links | [Gumroad](https://gumroad.com/l/31-days-of-drupal-migrations),      |



## 5- Fast Review

| Question // Answer         | 
| ------------- |
| **1- Is this book progressive, Iterative and Incremental?**         |
| Yes, it is. It's constructed in a very didactic and useful way.        |
| **2- Does it offer specific solutions to particular problems or concrete issues?**         |
| Yes, it offers solutions. The book contains many (many) practical examples and errors.         |
| **3- Does it explain well the original problems or needs it aims to solve?**       |
| Yes, the book is built from the needs that normally originate migration tasks.       |
| **4- Is this book rich in examples?**         |
| Yes, for each situation, case, resource, it offers a real and practical example, downloadable from a repository. .          |
| **5- Is this book written in plain English, suitable for non-English speakers?**          |
| Yes, this book is an easy read for non-English speakers, very pleasant.         |
| **6- Is it up to date?**         |
| Yes, seems to be aligned with the last big changes related to the Migrate API of Drupal, November 2020.          |



## 6- Ratings

![Table of Book Ratings for 31 Days of Drupal Migrations](../../images/post/davidjguru_drupal_8_9_books_31_days_of_drupal_migrations_rating_table.png)



## 7- :wq!



##### Recommended song: Sonny Rollins - Alfie's Theme

{{< youtube sW2aDwXO7Dg >}}

