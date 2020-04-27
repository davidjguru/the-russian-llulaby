---
title: "Thinking about Drupal migrations (III)"
date: 2020-05-04
draft: false

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
Everything around Test-driven development (TDD) is a very interesting and very motivating world and besides, these are topics with a certain antiquity. So it is very easy to find related contents. But the relationship between this area and Drupal is even more interesting: there are multiple options to implement testing of different types and different orientation (Unit Test, Kernel, JavaScript, functional ...) so it can be complex to introduce in this world. Today I want to take advantage of the experience of porting to Drupal 8 a small contributed module to share examples about browser-focused Functional Testing in Drupal 8 (or Drupal 9).  We will learn together with simple use cases. It may be a small slice (Functional Testing of a very small features) of a big cake (Testing in Drupal), but it will be a nice entry point. Follow me. 

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