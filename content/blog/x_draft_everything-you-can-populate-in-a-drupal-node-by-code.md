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



## 2- Basic scaffolding for a node 



![Cover of 31 Days of Drupal Migrations, by Mauricio Dinarte](../../images/post/davidjguru_drupal_8_9_books_31_days_of_drupal_migrations_one.png)




## 3- Vocabularies and taxonomy terms by code

* [Drupal Techniques: Creating nodes by code](https://davidjguru.github.io/blog/drupal-snippets-creating-nodes-by-code)  

## 4- Adding Paragraphs by code



## 5- Media items by code


* [Drupal 8 || 9 - How to create a media (remote video) entity node programmatically ](https://gitlab.com/-/snippets/2132938)  

## 6- Loading files in an image field


* [Drupal 8 || 9 - Loading a set of files into an image field programmatically ](https://gitlab.com/-/snippets/2123460)  

## 7- Patterns and aliases by code

* [Patterns and aliases by code](https://davidjguru.github.io/blog/drupal-techniques-patterns-and-aliases-by-code)  

![Table of Book Ratings for 31 Days of Drupal Migrations](../../images/post/davidjguru_drupal_8_9_books_31_days_of_drupal_migrations_rating_table.png)

## 8- Menu Links by code 

* [Create Menu Link](https://gist.github.com/davidjguru/6288a528d22c2b24b274d84e1b96c965#file-create_menu_link-php)  

## 9- Custom constraints and validations


* [Entity Validation API overview](https://www.drupal.org/node/2015613)  
* [Providing a custom validation constraint](https://www.drupal.org/docs/drupal-apis/entity-api/entity-validation-api/providing-a-custom-validation-constraint)  
* [Entity validation in migrations](https://www.webomelette.com/entity-validation-migrations-drupal-88)

## 10- :wq!


##### Recommended song: Troublemakers - Get Misunderstood

{{< youtube ETvT2kmeml4 >}}

