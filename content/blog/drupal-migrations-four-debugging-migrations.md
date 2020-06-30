---
title: "Drupal Migrations (IV): Debugging Migrations"
date: 2020-06-29
draft: true

# post thumb
image: "images/post/davidjguru_drupal_migrations_debugging_migrations_main.jpg"

# meta description
description: "Drupal Migrations (IV): Debugging Migrations"

# taxonomies
categories: 
  - "Migrations"  
tags:
  - "Drupal Migrations"
  - "Migrate API"
  - "Contrib Modules"
  - "Drupal Plugins"
  - "ETL"

# post type
type: "post"
---

The Drupal migrations, despite their linearity in terms of definitions, contain a lot of inherited complexity. The reason is very intuitive: although the Migrate API is a supersystem that offers a very simple "interface" of interactions for the user-developer who wants to build migration processes, in reality several subsystems work by interacting with each other throughout a migration process: Entities, Database, Plugins...There are a lot of classes involved in even the simplest migration process. If we add the irrefutable fact that a migration will tend to generate errors in many cases until it has been refined, it's clear then that one of our first needs will be to learn...how to debug migrations.
 
--------------------------------------------------------------------------------------

**Picture from Unsplash, user [Krzysztof Niewolny, @epan5](https://unsplash.com/@epan5)**

  
---------------------------------------------------------------------------------
**Table of Contents**

<!-- TOC -->
[1- Introduction](#1--introduction)  
[2- Basic Debugging: Keep an eye on your file](#2--basic-debugging-keep-an-eye-on-your-files)  
[3- Average Debugging: Modules, Plugins and Feedbacks](#3--average-debugging-modules-plugins-and-feedbacks)  
[4- Advanced Debugging: Xdebug, Methods and some combinations](#4--advanced-debugging-xdebug-methods-and-some-combinations)  
[5- :wq!](#5--wq)
<!-- /TOC -->
-----------------------------------------------------------------------------------------

**This article is part of a series of posts about Drupal Migrations:**

<!-- TOC -->
[1- Drupal Migrations (I): Basic Resources](https://www.therussianlullaby.com/blog/drupal-migrations-one-basic-resources/)  

[2- Drupal Migrations (II): Examples](https://www.therussianlullaby.com/blog/drupal-migrations-two-examples/)  

[3- Drupal Migrations (III): Migrating from Google Spreadsheet](https://www.therussianlullaby.com/blog/drupal-migrations-three-migrating-from-google-spreadsheet/)  
<!-- /TOC -->

---------------------------------------------------------------------------------

## 1- Introduction
In the wake of the latest articles, I wanted to continue expanding information about migration in Drupal. 

## 2- Basic Debugging (Keep an eye on your files)

First, we will start with a very primary approach to error detection during a migration. To begin with, it is essential to keep the focus on reducing the range of error possibilities as much as possible by approaching the migration in an iterative and incremental manner. In other words: we will go step by step and expand our migrated data. 

## 2.1- Reviewing your Migration description file
First of all we are going to comment on the most intuitive step of all we will take, since sometimes there are errors that occur at first sight and not because they are recurrent but end up being more obvious. 

The first steps in our process of debugging a migration will be a review of two fundamental issues that usually appear in many migrations. So before anything else, we'll do a quick review of: 

**Whitespaces:** Any extra whitespace may be causing us problems at the level of the migration description file: we review all lines of the file in a quick scan in order to detect extra whitespace.

**Errors in the indentation:** The migration description file has a format based on YAML, a language for data serialization based on a key scheme: a value where it is structured by parent - child levels and an indentation of two spaces to the right at each level down in the hierarchy. It is very frequent that some indentation is not the right one and this ends up producing an error in the processing of the file. As a measure, as in the previous case, we will review all the cases of indentations registered in the file. 

You can rely on a YAML syntax review service such as [www.yamllint.com](http://www.yamllint.com), but you will have to monitor the result as well. 

## 2.2- Reviewing registers in database
If you're in a basic Drupal installation (standard profile) we have seventy-three tables, after the activation of the basic modules related to migrations: migrate, migrate_plus, migrate_tools and in this case the custom *migration_google_sheet_wrong* the number of tables in the database is seventy-five. Two more tables have been generated: 

```yaml
cache_migrate
cache_discovery_migration
```

But also, later, after executing the migration with ID taxonomy_google_sheet_wrong contained in our custom module, we see in the database that two new tables have been generated related to the executed migration:  

* **migrate_map_taxonomy_google_sheet**  
This table contains the information related to the movements of a row of data (migrations are operated 'row' to 'row'). Migrate API is in charge of storing in this table the source ID, the destination ID and a hash related to the 'row' in this data mapping table. Combinations between the source ID and the hash of the row operation then make it easier to track changes, progressively update information, and cross dependencies when performing a batch migration (see below for how they are articulated).  
 The lookup processes for migrations are supported by this data: for example, to load a taxonomy term you must first lookup its "parent" term to maintain the hierarchy of terms. 
If we go to our database and we do not see recorded results after launching a migration, no data was stored and the migration requires debugging.  

* **migrate_message_taxonomy_google_sheet**
In this table, messages associated to the executed migration will be stored, structured in the same way as the previous table (based on the processing of a 'row' of the migration), each message with its own identifier and an association to the id_hash of the 'row' of origin of the data:  

![Drupal Migration columns from table Messages](../../images/post/davidjguru_drupal_migrations_debugging_two.jpg)  

This information can be obtained through Drush, since the content of this table is what is shown on the screen when we execute the instruction:  

```yaml
drush migrate:messages id-migration
```

And this can be a useful way to start getting information about the errors that occurred during our migration.  


## 3- Average Debugging (Modules, Plugins and feedbacks)

Well, we have looked closely at the data as we saw in the previous section and yet our migration of taxonomy terms from a Google Spreadsheet seems not to work. 

We have to resort to intermediate techniques in order to obtain more information about the process. In this phase our allies will be some modules and plugins that can help us to better visualize the migration process.  

## 3.1- Migrate Devel 

Migrate Devel [https://www.drupal.org/project/migrate_devel](https://www.drupal.org/project/migrate_devel) is a contributed module that brings some extra functionality to the migration processes from new options for drush. This module works with *migrate_tools* and *migrate_run*.  

The particularity is that it's optimized for a previous version of Drush (8) and it does not seem to have closed its portability to Drush 9 and higher.  

There is a necessary patch in its Issues section to be able to use it in versions of Drush > 9 and if we make use of this module this patch [https://www.drupal.org/node/2938677 ](https://www.drupal.org/node/2938677) will be almost mandatory. The patch does not seem to be in its final version either, but at least it allows a controlled and efficient execution of some features of the module. Here will see some of its contributions. To install and enable the module, we proceed to download it through composer and activate it with drush: 

```yaml
composer require drupal/migrate_devel
drush on migrate_devel -y
```
And to apply the patch we can download it with wget and apply it with git apply:  

```bash
cd /web/modules/contrib/migrate_devel/
wget https://www.drupal.org/files/issues/2018-10-08/migrate_devel-drush9-2938677-6.patch 
git apply migrate_devel-drush9-2938677-6.patch
```
Or place it directly in the patch area of our composer.json file if we have the patch management extension enabled: [https://github.com/cweagans/composer-patches](https://github.com/cweagans/composer-patches).  

Using: 
```bash
composer require cweagans/composer-patches 
```
And place the new patch inside the "extra" section of our composer.json file:  

![Drupal Debugging adding the patch](../../images/post/davidjguru_drupal_migrations_debugging_three.png)  

The launch of a migration process with the parameters provided by Migrate Devel will generate an output of values per console that we can easily check, for example using --migrate-debug:  

![Drupal Devel Output first part](../../images/post/davidjguru_drupal_migrations_debugging_four.png)  
![Drupal Devel Output second part](../../images/post/davidjguru_drupal_migrations_debugging_five.png)

This is a partial view of the processing of a single row of migrated data, showing the data source, the values associated with this row and the final destination ID, which is the identifier stored in the migration mapping table for process tracking: 


Now we can see in the record that for the value 1 in origin (first array of values), the identifier 117 was assigned for the load in destination. This identifier will also be the internal id of the new entity (in this case taxonomy term) created within Drupal as a result of the migration. This way you can relate the id of the migration with the new entity created and stored. 

**How does it work?**, Migrate Devel creates an event subscriber, a class that implements EventSubscriberInterface and keeps listening to events generated from the event system of the Drupal's Migrate API, present in the migrate module of Drupal's core: 

```text
Called from +56 /var/www/html/web/modules/contrib/migrate_devel/src/EventSubscriber/MigrationEventSubscriber.php
```
The call is made from the class where events are heard and actions from the module's Event classes are read. Many events are defined there [modules/migrate/src/Event](https://git.drupalcode.org/project/drupal/-/tree/9.0.x/core/modules/migrate/src/Event ), but in particular, two that are listened to by Migrate Devel:  

1. MigratePostRowSaveEvent.php 
2. MigratePreRowSaveEvent.php

What are the two Drush options offered by Migrate Devel, and in both cases results in a call to the Kint library dump() function provided by the Devel module to print messages.  

You can get more information about creating events and event subscribers in Drupal here in The Russian Lullaby: [Building Symfony events for Drupal](https://www.therussianlullaby.com/blog/building-symfony-events-for-drupal/).  



## 3.2- Debug Process Plugin



## 3.3- Log Process Plugin



## 4- Advanced Debugging (Xdebug, methods and some combinations)

## 4.1- Xdebug configuration


## 4.2- Drush print


## 4.3- Callback Plugin + var_dump()


## 5- :wq! 


##### Recommended song

{{< youtube Z2tvlp7RnlM >}}