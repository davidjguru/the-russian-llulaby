---
title: "Thinking about Drupal migrations (III)"
date: 2020-05-04
draft: true

# post thumb
image: "images/post/davidjguru_drupal_migrations_debugging_main.jpg"

# meta description
description: "Thinking about Drupal Migrations (III) - Debugging"

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
The systems and subsystems related to Drupal's migration API are certainly exciting. In the previous articles in this series, I wanted to draw as complete a map as possible (part one) of the vast amount of resources, possibilities and referenced experts. In the second part I wanted to expose some basic mechanics of the migration processes in Drupal and knowing that this opens the door to thousands of options, possibilities and techniques....I didn't want to let a third article go by without sharing some experiences diagnosing, debugging and solving problems in migrations. It is very easy (more than desired) for something to go wrong during a migration, so I prefer - before we go any further - that we share experiences about repairing migration processes. 
 
--------------------------------------------------------------------------------------

**Picture from Unsplash, user [Pan Xiaozhen, @zhenhappy](https://unsplash.com/@zhenhappy)**

  
---------------------------------------------------------------------------------
**Table of Contents**

<!-- TOC -->
[1- Introduction](#1--introduction)  
[2- Basic Resources - Core Modules](#2--basic-resources---core-modules)  
[3- Other Basic Resources - Contrib Modules](#3--other-basic-resources---contrib-modules)  
[4- Extra Resources - Contrib Modules for Plugins](#4--extra-resources---contrib-modules-for-plugins)  
[5- Migration Runners - Contrib Modules Drush-Related](#5--migrations-runners---contrib-modules-drush-related)  
[6- Authors you should know](#6--authors-you-should-know)  
[7- :wq!](#7--wq)
<!-- /TOC -->

-----------------------------------------------------------------------------------------

**This article is part of a series of posts about Drupal Migrations**

<!-- TOC -->
[1- Thinking about Drupal Migrations (I): Resources](https://therussianlullaby.com/blog/thinking-about-drupal-migrations-resources/)  

[2- Thinking about Drupal Migrations (II): Examples](https://therussianlullaby.com/blog/thinking-about-drupal-migrations-examples)  

[3- Thinking about Drupal Migrations (III): Debugging](https://therussianlullaby.com/blog/thinking-about-drupal-migrations-debugging)  



<!-- /TOC -->

---------------------------------------------------------------------------------

## 1- Introduction
Well, I would like to start by posing a scenario: imagine that we must migrate a list of taxonomy terms to our Drupal Installation. The source of the data is important, since it defines what type of Plugins we will have to use (source Plugins for the extract, but it can also have influence for the processing and for the final load in destination).

The imagined scenario of this article will be the following: we have "inherited" a migration task already initiated by a previous partner 
To practice with other Plugins and other migration models, I thought we can take a Google spreadsheet as a source of data. Let's not forget the frame we're in: 

![ETL Scheme and Drupal Migrate API overview](../../images/post/davidjguru_drupal_migrations_ETL_scheme.png)  

So now, to perform source data extraction, we'll need a spreadsheet with exposed values. I've created a Google spreadsheet [here at this address](https://docs.google.com/spreadsheets/d/1bKGbPbgeuXaBfcKetaDqoDimmYcerQY_hT1rqzw4TbM/edit?usp=sharing). This Google Spreadsheet contains some columns with values related with fields of a taxonomy term, ready for migration.  

In order to processing the data source we'll use the [Migrate Google Sheets contrib module from Drupal.org](https://www.drupal.org/project/migrate_google_sheets), so you can run your Composer to download the resource. Just launch: 
```
composer require drupal/migrate_google_sheets
drush en migrate_google_sheet
```

## 2- First Block: Check your files

## 3- Second Block: Know your resources

## 4- Third Block: Debugging, debugging, debugging


## 5- Read More

* [PHPUnit in Drupal 8](https://www.drupal.org/docs/8/testing/phpunit-in-drupal-8)
* [PHPUnit Browser Test tutorial in Drupal.org](https://www.drupal.org/node/2783189).  
* [Automated Test using PHPUnit and Nightwatch.js in Drupal.org](https://api.drupal.org/api/drupal/core%21core.api.php/group/testing/8.8.x).  
* [Running PHPUnit tests in Drupal, from Drupal.org documentation](https://www.drupal.org/node/2116263).  
* [Running Drupal's PHPUnit test suites on DDEV, by Matt Glaman](https://glamanate.com/blog/running-drupals-phpunit-test-suites-ddev).  
* [Mink at a glance, from Mink documentation](http://mink.behat.org/en/latest/at-a-glance.html).  
* [Developing Web Applications with Behat and Mink](https://docs.behat.org/en/v2.5/cookbook/behat_and_mink.html)  


# 6- :wq! 

##### Recommended song

{{< youtube bGISz52QGEE >}}
