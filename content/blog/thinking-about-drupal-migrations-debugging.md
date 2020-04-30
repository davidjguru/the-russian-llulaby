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
The systems and subsystems related to Drupal's migration API are certainly exciting. In the previous articles in this series, I wanted to draw as complete a map as possible (part one) of the vast amount of resources, possibilities and experts. In the second part I wanted to expose some basic mechanics of the migration processes in Drupal and knowing that this opens the door to thousands of options, possibilities and techniques....I didn't want to let a third article go by without sharing some experiences diagnosing, debugging and solving problems in migrations. It is very easy (more than desired) for something to go wrong during a migration, so I prefer - before we go any further - that we share experiences in repairing migration processes. 
 
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